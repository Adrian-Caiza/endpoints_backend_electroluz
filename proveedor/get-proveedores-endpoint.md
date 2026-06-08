# GET /proveedores

## Descripción
Obtiene el listado paginado de proveedores de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /proveedores?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "provid": "a052ef32-3c4b-41da-871a-dfec99d8afa3",
      "provemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
      "categoria": {
        "ctgriaid": "85cdca30-3008-466b-b896-52d588cdda10",
        "ctgnombre": "Pintura",
        "ctgriadescripcion": null
      },
      "marca": {
        "mrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
        "mrcnombre": "Truper"
      },
      "provnombre": "DeWalt Centro Historico",
      "provtelefono": "0984653471",
      "provcorreo": null,
      "provfchregistro": "2026-05-23T17:29:01.621Z",
      "provestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 5,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna proveedores de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- `categoria` y `marca` pueden ser `null` cuando el proveedor no tiene esas relaciones asignadas.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
