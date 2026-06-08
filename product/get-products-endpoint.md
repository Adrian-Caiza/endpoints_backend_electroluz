# GET /products

## Descripción
Obtiene el listado paginado de productos de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /products?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "prdtoid": "f8a0a2ab-9fbe-4fcf-b4d4-6888e0c4f410",
      "prdtoemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
      "categoria": {
        "ctgriaid": "85cdca30-3008-466b-b896-52d588cdda10",
        "ctgnombre": "Pintura",
        "ctgriadescripcion": null
      },
      "marca": {
        "mrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
        "mrcnombre": "Truper"
      },
      "proveedor": {
        "provid": "a052ef32-3c4b-41da-871a-dfec99d8afa3",
        "provnombre": "DeWalt Centro Historico"
      },
      "medida": {
        "mdiaid": "7f4f7a99-f7dc-4b10-9326-9e4ec8300e89",
        "mdianombre": "Unidad",
        "mdiaabreviatura": "UND"
      },
      "prdtocodigo": "PRD-0001",
      "prdtonombre": "Taladro Inalambrico 20V",
      "prdtopreciocompra": 120.5,
      "prdtoprecioventa": 165.99,
      "prdtostockminimo": 5,
      "prdtostockmaximo": 50,
      "prdtoimagen": "https://api.tudominio.com/uploads/productos/product.png",
      "prdtofchregistro": "2026-05-23T17:29:01.621Z",
      "prdtoestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 5,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna productos de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- `prdtoimagen` en la respuesta se entrega como URL pública del backend.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
