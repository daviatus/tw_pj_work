---
title: Petstore API – kisállat lekérdezése
description: Petstore API bemutatása lépésről lépésre, hogyan lehet törölni egy kisállatot.
sidebar_position: 5
sidebar_label: Törlés
---

# Kisállat törlése
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
