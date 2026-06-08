# GET /medidas

## Descripción
Obtiene el listado paginado de medidas de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /medidas?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "mdiaid": "7f4f7a99-f7dc-4b10-9326-9e4ec8300e89",
      "mdiaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
      "mdianombre": "Unidad",
      "mdiaabreviatura": "UND",
      "mdiafchregistro": "2026-05-23T17:29:01.621Z",
      "mdiaestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 5,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna medidas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede listar medidas.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
