---
title: Petstore API – Gyors felhasználói útmutató
description: Petstore API bemutatása lépésről lépésre, hogyan lehet létrehozni egy kisállatot, lekérdezni, frissíteni vagy törölni.
sidebar_position: 1
sidebar_label: Petstore API útmutató
---

# Petstore API – Gyors felhasználói útmutató
Demó API: https://petstore.swagger.io/

Ez az útmutató lépésről lépésre bemutatja, hogyan lehet egy kisállatot létrehozni, lekérdezni, frissíteni és törölni a **Petstore REST API** használatával.  
A példákban **cURL** parancsokat használunk, de bármely HTTP-klienst alkalmazhatsz (Postman, Swagger UI stb.).

---

## 1. Új kisállat létrehozása
**Végpont:** `POST /pet`

### Kérés példa (request)
```bash
curl -X POST "https://petstore.swagger.io/v2/pet" \
  -H "Content-Type: application/json" \
  -d '{
        "id": 12345678,
        "name": "Morzsi",
        "photoUrls": ["string"],
        "status": "available"
      }'
```

### Fontos mezők
| Mező | Leírás |
|------|--------|
| `id` | Egyedi numerikus ID |
| `name` | Kisállat neve |
| `status` | Állapot (`available`, `pending`, `sold`) |

### Válasz példák (response)
**Sikeres válasz**
```json
{
  "id": 12345678,
  "name": "Morzsi",
  "photoUrls": ["string"],
  "status": "available"
}
```
**Sikertelen válaszok**
- *Hiányzó vagy hibás request body -* ***HTTP 400 – Bad Request***

Ha a kliens nem küld érvényes JSON-t, vagy a body üres:

```json
{
  "code": 400,
  "type": "unknown",
  "message": "Invalid input"
}
```
- *Hibás JSON szerkezet -* ***HTTP 400 – Bad Request***

Rosszul formázott JSON esetén:

```json
{
  "code": 400,
  "type": "error",
  "message": "Malformed request"
}
```

- *Hiányzó kötelező mezők -* ***HTTP 405 – Method Not Allowed***

A demonstrációs API nem mindig szigorú, de elvileg ilyen választ adhat, ha pl. a `name` mező hiányzik:
```json
{
  "code": 405,
  "type": "unknown",
  "message": "Invalid input"
}
```

> **Megjegyzés:**  
> Petstore demó API esetén gyakori, ha a szerver nem tudja feldolgozni az adatot.

- *Szerverhiba -* ***HTTP 500 – Internal Server Error***

Ritka, de előfordulhat:

```json
{
  "code": 500,
  "type": "error",
  "message": "Internal server error"
}
```

---

## 2. Létrehozott kisállat lekérdezése
**Végpont:** `GET /pet/{petId}`

### Kérés példa (request)
```bash
curl -X GET "https://petstore.swagger.io/v2/pet/12345678"
```

### Válasz példák (response)
**Sikeres válasz**
```json
{
  "id": 12345678,
  "name": "Morzsi",
  "photoUrls": ["string"],
  "status": "available"
}
```

**Sikertelen válaszok**

- *A megadott `petId` nem található -* ***HTTP 404 – Not Found***

Ha az adott ID nem létezik az adatbázisban:

```json
{
  "code": 1,
  "type": "error",
  "message": "Pet not found"
}
```

- *Érvénytelen vagy hibás `petId` formátum -* ***HTTP 400 – Bad Request***

Ha például olyan ID kerül megadásra, ami nem értelmezhető számként:

```json
{
  "code": 400,
  "type": "unknown",
  "message": "Invalid ID supplied"
}
```

- *Szerverhiba -* ***HTTP 500 – Internal Server Error***

Ritka, de demo környezetben előfordulhat:

```json
{
  "code": 500,
  "type": "error",
  "message": "Internal server error"
}
```

---

## 3. Kisállat adatainak frissítése
**Végpont:** `PUT /pet`

A név módosítása: **"Buksi"**

### Kérés példák (request)
```bash
curl -X PUT "https://petstore.swagger.io/v2/pet" \
  -H "Content-Type: application/json" \
  -d '{
        "id": 12345678,
        "name": "Buksi",
        "photoUrls": ["string"],
        "status": "available"
      }'
```

> **Megjegyzés:**  
> A PUT művelethez az **egész objektumot** meg kell adni, nem csak a módosítandó mezőt.

### Válasz példák (response)
**Sikeres válasz**

```json
{
  "id": 12345678,
  "name": "Buksi",
  "photoUrls": ["string"],
  "status": "available"
}
```

**Sikertelen válaszok**

- *Hiányzó vagy hibás request body -* ***HTTP 400 – Bad Request***

Ha a kliens üres vagy érvénytelen JSON-t küld:

```json
{
  "code": 400,
  "type": "unknown",
  "message": "Invalid input"
}
```

- *A megadott `petId` nem található -* ***HTTP 404 – Not Found***

Ha létezik ugyan a kérés, de az azonosító nem szerepel a rendszerben:

```json
{
  "code": 1,
  "type": "error",
  "message": "Pet not found"
}
```

- *Hibás JSON formátum -* ***HTTP 400 – Bad Request***

Ha a body nem jól formázott:

```json
{
  "code": 400,
  "type": "error",
  "message": "Malformed request"
}
```

- *Hiányzó kötelező mezők -* ***HTTP 405 – Method Not Allowed***

A Petstore API PUT esetén teljes objektumot vár — ha fontos mezők hiányoznak:

```json
{
  "code": 405,
  "type": "unknown",
  "message": "Validation exception"
}
```

- *Szerverhiba -* ***HTTP 500 – Internal Server Error***

Ritka, de előfordulhat:

```json
{
  "code": 500,
  "type": "error",
  "message": "Internal server error"
}
```

---

## 4. Kisállat törlése
**Végpont:** `DELETE /pet/{petId}`

### Kérés példa (request)
```bash
curl -X DELETE "https://petstore.swagger.io/v2/pet/12345678"
```

### Válasz példák (response)
**Sikeres válasz**
```json
{
  "code": 200,
  "message": "12345678"
}
```
**Sikertelen válaszok**

- *A megadott `petId` nem található -* ***HTTP 404 – Not Found***

Ez akkor fordul elő, ha a törölni kívánt kisállat nem létezik az adatbázisban.

```json
{
  "code": 1,
  "type": "error",
  "message": "Pet not found"
}
```

- *Érvénytelen `petId` formátum -* ***HTTP 400 – Bad Request***

Ha például olyan azonosítót adsz meg, ami nem konvertálható számra.

```json
{
  "code": 400,
  "type": "unknown",
  "message": "Invalid ID supplied"
}
```

- *Szerveroldali hiba (ritka) -* ***HTTP 500 – Internal Server Error***

A Petstore egy demo API, de ha valamilyen belső hiba történik:

```json
{
  "code": 500,
  "type": "error",
  "message": "Internal server error"
}
```
---

# Összegzés
Ebben az útmutatóban megismertük a Petstore API négy alapműveletét:

1. **Létrehozás** – `POST /pet`  
2. **Lekérdezés** – `GET /pet/{petId}`  
3. **Frissítés** – `PUT /pet`  
4. **Törlés** – `DELETE /pet/{petId}`  

Mind a négy alapműveletnél példán keresztül bemutattunk egy lehetséges kérést és a hozzá tartozó sikeres, illetve lehetséges sikertelen válaszokat.

A Petstore API nem valós adatbázist használ, ezért nem garantált a tartósság.

## What's next?
- Postman bemutatása
- Swagger UI bemutatása.
