# GET /checkouts

## Descripción
Obtiene el listado paginado de cajas de la empresa del usuario autenticado, incluyendo datos básicos de su sucursal.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /checkouts?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "cjid": "e1b3da39-d5d5-47d6-a351-0e61e586f732",
      "cjemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
      "cjsuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
      "cjidentificador": "001",
      "cjfchregistro": "2026-05-19T22:10:00.000Z",
      "cjestado": "activo",
      "sucursal": {
        "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
        "sunombre": "Sucursal Centro",
        "suidentificador": "001",
        "suestado": "activo"
      }
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 5,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna cajas de la misma empresa del usuario autenticado.
- Incluye por cada caja los datos básicos de sucursal: `suid`, `sunombre`, `suidentificador`, `suestado`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` o `pageSize` inválidos (no entero positivo): error de validación.
- Usuario sin permisos para el endpoint: error de autorización.
