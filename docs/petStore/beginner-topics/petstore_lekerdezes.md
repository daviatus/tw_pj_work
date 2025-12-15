---
title: Petstore API – kisállat lekérdezése
description: Petstore API bemutatása lépésről lépésre, hogyan lehet lekérdezni egy kisállatot.
sidebar_position: 3
sidebar_label: Lekérdezés
---

# Létrehozott kisállat lekérdezése
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