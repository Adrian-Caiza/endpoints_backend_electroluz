# GET /companies

## Descripción
Obtiene el listado paginado de empresas.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /companies?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "emid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
      "emruc": "1790012345001",
      "emrznsocial": "Ferreteria Central S.A.",
      "emcorreo": "contacto@ferreteriacentral.com",
      "emlogo": "https://api.tudominio.com/uploads/empresas/company.png",
      "emcodigo": "FC01",
      "emfchregistro": "2026-05-17T15:20:10.000Z",
      "emestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 57,
  "totalPages": 3
}
```

## Comportamiento relevante
- El backend transforma `emlogo` a URL pública completa.
- Solo un usuario con rol `administrador` puede listar empresas.
- La empresa del usuario autenticado debe estar activa.
- La empresa del usuario autenticado debe ser empresa padre.
- Si no hay registros en la página solicitada, `items` puede venir vacío.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol `administrador`: `500 Internal Server Error` con mensaje `User is not admin`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa no activa: `500 Internal Server Error` con mensaje `Company is not active`.
- Empresa autenticada no es empresa padre: `500 Internal Server Error` con mensaje `Company is not parent`.
