# GET /proformas/:id/pdf

## Descripción
Obtiene la referencia al documento PDF de una proforma específica.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Params
- `id` (string, requerido): id de la proforma (`prfmaid`).

Ejemplo:

`GET /proformas/66ff3afe-65bb-49f2-80cf-ae279b7fcf5b/pdf`

## Respuesta Exitosa
`200 OK`

```json
{
  "proforma": {
    "prfmaid": "66ff3afe-65bb-49f2-80cf-ae279b7fcf5b",
    "prfmaidentificador": "FE01-002-001-34",
    "documento": {
      "docnombre": "FE01-002-001-34_2026-05-25.pdf",
      "docurl": "http://localhost:3000/uploads/proformas/1709639106001/FE01-002-001-34_2026-05-25.pdf"
    }
  }
}
```

## Comportamiento relevante
- Solo retorna proformas de la misma empresa del usuario autenticado.
- Si la proforma existe pero no tiene PDF disponible, el endpoint responde `404 Proforma pdf document not found`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.

## Errores comunes
- `401 Unauthorized`: token inválido o ausente.
- `404 Not Found`: `Proforma not found` o `Proforma pdf document not found`.
- `400 Bad Request`: `Proforma id is required`.
