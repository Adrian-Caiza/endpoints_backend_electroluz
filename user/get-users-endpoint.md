# GET /users

## Descripción
Obtiene el listado paginado de usuarios de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /users?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "usid": "2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07",
      "usemid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
      "usnombre": "Juan Perez",
      "usapodo": "jperez",
      "uscorreo": "juan.perez@empresa.com",
      "usimagen": "https://api.tudominio.com/uploads/usuarios/user.png",
      "usrol": "empleado",
      "usfchregistro": "2026-05-18T18:20:10.000Z",
      "usestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 5,
  "totalPages": 1
}
```

## Comportamiento relevante
- El backend transforma `usimagen` a URL pública completa.
- Solo retorna usuarios de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Los roles permitidos para este endpoint son `administrador`, `jefe` o `empleado`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
