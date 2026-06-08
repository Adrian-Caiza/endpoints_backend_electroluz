# GET /playment-methods

## Descripción
Obtiene el listado paginado de métodos de pago de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /playment-methods?page=1&pageSize=20`

```bash
curl -X GET "http://localhost:3000/playment-methods?page=1&pageSize=20" \
  -H "Authorization: Bearer TU_TOKEN"
```

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "mpid": "2b2f66f5-94ad-49ff-8d43-9d7f56e8c11b",
      "mpemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
      "mpnombre": "Transferencia bancaria",
      "mpfchregistro": "2026-05-24T03:15:11.245Z",
      "mpestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 1,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna métodos de pago de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
