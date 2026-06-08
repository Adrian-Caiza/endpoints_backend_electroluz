# PATCH /proveedores/:id

## Descripción
Actualiza un proveedor por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del proveedor.

Ejemplo:

`PATCH /proveedores/a052ef32-3c4b-41da-871a-dfec99d8afa3`

## Body
- `provctgriaid` (opcional): id de la categoría. También puede enviarse `null`.
- `provmrcid` (opcional): id de la marca. También puede enviarse `null`.
- `provnombre` (opcional): nombre del proveedor.
- `provtelefono` (opcional): teléfono del proveedor (formato móvil de 10 dígitos, por ejemplo `0984653471`). También puede enviarse `null`.
- `provcorreo` (opcional): correo del proveedor. También puede enviarse `null`.
- `provestado` (opcional): estado del proveedor (`activo`, `inactivo`, `eliminado`).

## Ejemplo (JSON)

```json
{
  "provctgriaid": "85cdca30-3008-466b-b896-52d588cdda10",
  "provmrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
  "provnombre": "DeWalt Centro Norte",
  "provtelefono": "0984653471",
  "provcorreo": "contacto@dewalt-centro.com",
  "provestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
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
  "provnombre": "DeWalt Centro Norte",
  "provtelefono": "0984653471",
  "provcorreo": "contacto@dewalt-centro.com",
  "provfchregistro": "2026-05-23T17:29:01.621Z",
  "provestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar proveedores de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede actualizar un proveedor.
- Si el proveedor está en estado `eliminado`, no se permite actualizar.
- Si se envía `provnombre`, se valida que no exista duplicado dentro de la misma empresa.
- Si se envía `provctgriaid` (no `null`), se valida que la categoría exista en la misma empresa y esté activa.
- Si se envía `provmrcid` (no `null`), se valida que la marca exista en la misma empresa y esté activa.
- Debe enviarse al menos un campo para actualizar.
- `provctgriaid`, `provmrcid`, `provtelefono` y `provcorreo` pueden enviarse como `null`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request` con mensaje `Proveedor id is required`.
- Body vacío o sin cambios aplicables: `500 Internal Server Error` con mensaje `At least one field is required to update proveedor`.
- `provnombre` vacío: `500 Internal Server Error` con mensaje `Proveedor name is required`.
- `provtelefono` vacío: `500 Internal Server Error` con mensaje `Proveedor phone is required`.
- `provtelefono` inválido: `500 Internal Server Error` con mensaje `Proveedor phone must be valid`.
- `provcorreo` inválido: `500 Internal Server Error` con mensaje `Proveedor email must be valid`.
- `provctgriaid` vacío: `500 Internal Server Error` con mensaje `Proveedor category id is required`.
- `provmrcid` vacío: `500 Internal Server Error` con mensaje `Proveedor brand id is required`.
- `provestado` inválido: `500 Internal Server Error` con mensaje `Proveedor status must be activo, inactivo or eliminado`.
- Proveedor duplicado por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Proveedor already exists with that name`.
- Categoría inexistente: `500 Internal Server Error` con mensaje `Proveedor category does not exist`.
- Categoría inactiva o eliminada: `500 Internal Server Error` con mensaje `Proveedor category is not active`.
- Marca inexistente: `500 Internal Server Error` con mensaje `Proveedor brand does not exist`.
- Marca inactiva o eliminada: `500 Internal Server Error` con mensaje `Proveedor brand is not active`.
- Proveedor en estado `eliminado`: `500 Internal Server Error` con mensaje `Deleted proveedor cannot be updated`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Proveedor no encontrado: `500 Internal Server Error` con mensaje `Proveedor not found`.
