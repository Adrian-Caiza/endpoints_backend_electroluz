# GET /alerts

## Descripción
Obtiene el listado paginado de alertas visibles de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params
- `page` (requerido): número de página (entero positivo).
- `pageSize` (requerido): cantidad de registros por página (entero positivo).
- `suid` (opcional): id de la sucursal para filtrar alertas de una sucursal específica.

Ejemplos:

`GET /alerts?page=1&pageSize=20`

`GET /alerts?page=1&pageSize=20&suid=9a9fbc8e-1ed5-45a5-8e4d-7e5c31790001`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "alid": "8b6e1f4a-4ad8-4d9e-9a0f-8e58b0df1111",
      "alemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
      "branch": {
        "suid": "9a9fbc8e-1ed5-45a5-8e4d-7e5c31790001",
        "sunombre": "Sucursal Norte",
        "suidentificador": "NORTE-01"
      },
      "product": {
        "prdtoid": "0b8f4ef4-cd8c-4ebf-a385-16ef7f380001",
        "prdtocodigo": "MART-001",
        "prdtonombre": "Martillo 16oz"
      },
      "altipo": "stock_bajo",
      "almensaje": "Stock bajo en Sucursal Norte: Martillo 16oz (MART-001) - Actual: 2, Mínimo: 5",
      "alcantidadactual": 2,
      "alstockminimo": 5,
      "alstockmaximo": 20,
      "alvisible": true,
      "alvisto": false,
      "alfchcreacion": "2026-06-06T22:55:11.000Z"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 1,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna alertas de la misma empresa del usuario autenticado.
- Si envías `suid`, el servicio valida que la sucursal exista en la empresa autenticada.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.
- Solo retorna alertas con `alvisible = true`.
- El resultado se ordena por `alfchcreacion` descendente.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- `suid` inexistente en la empresa autenticada: `500 Internal Server Error` con mensaje `Branch does not exist`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe, empleado or admin`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.

