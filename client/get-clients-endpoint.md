# GET /clients

## Descripción
Obtiene el listado paginado de clientes de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /clients?page=1&pageSize=20`

```bash
curl -X GET "http://localhost:3000/clients?page=1&pageSize=20" \
  -H "Authorization: Bearer TU_TOKEN"
```

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "clnteid": "9a4d1377-c64c-4fd7-bf7e-f23d2d9fd5f9",
      "clnteemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
      "clntetipoidentificacion": "cedula",
      "clnteidentificacion": "1712345678",
      "clntenombre": "Juan Perez",
      "clntecorreo": "juan.perez@email.com",
      "clntedireccion": "Av. Principal y Calle 10",
      "clntetelefono": "0987654321",
      "clntefchregistro": "2026-05-24T03:10:15.000Z",
      "clnteestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 1,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna clientes de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede listar clientes.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
