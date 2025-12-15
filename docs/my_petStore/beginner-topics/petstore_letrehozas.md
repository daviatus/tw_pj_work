---
title: Petstore API – új kisállat létrehozása
description: Petstore API bemutatása lépésről lépésre, hogyan lehet létrehozni egy kisállatot.
sidebar_position: 2
sidebar_label: Létrehozás
---


# Új kisállat létrehozása
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