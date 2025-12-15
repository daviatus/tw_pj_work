---
title: Petstore API – Kisállat adatainak frissítése
description: Petstore API bemutatása lépésről lépésre, hogyan lehet frissíteni egy kisállatot.
sidebar_position: 4
sidebar_label: Adatok frissítése
---

# Kisállat adatainak frissítése
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