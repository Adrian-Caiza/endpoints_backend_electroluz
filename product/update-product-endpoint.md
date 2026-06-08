# PATCH /products/:id

## Descripción
Actualiza un producto por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json` (sin imagen)
- `multipart/form-data` (con imagen)

## Path Param
- `id`: identificador del producto.

Ejemplo:

`PATCH /products/f8a0a2ab-9fbe-4fcf-b4d4-6888e0c4f410`

## Body (todos opcionales)
- `prdtoctgriaid`: id de la categoría.
- `prdtomrcid`: id de la marca.
- `prdtoprovid`: id del proveedor.
- `prdtomdiaid`: id de la medida.
- `prdtocodigo`: código del producto.
- `prdtonombre`: nombre del producto.
- `prdtopreciocompra`: precio de compra.
- `prdtoprecioventa`: precio de venta.
- `prdtostockminimo`: stock mínimo.
- `prdtostockmaximo`: stock máximo.
- `prdtoestado`: estado del producto (`activo`, `inactivo`, `eliminado`).
- `imagen` (opcional, archivo): imagen del producto.

## Reglas de imagen (si se envía)
- Campo: `imagen`
- Formatos permitidos: `.png`, `.jpg`
- Tamaño máximo: `5MB`

## Ejemplo (JSON)

```json
{
  "prdtoctgriaid": "d9475c59-4fcc-469a-ad8f-3f9f28190058",
  "prdtomrcid": "4ba16156-e626-4fe3-8b5c-0d8ab3f19674",
  "prdtoprovid": "6d50fa31-54c0-4ea3-a798-19f4801b70de",
  "prdtomdiaid": "51ed3597-837e-42d1-9bf7-79a30e00f51d",
  "prdtocodigo": "012345678901",
  "prdtonombre": "Brocha 1\"",
  "prdtopreciocompra": 1.25,
  "prdtoprecioventa": 1.8,
  "prdtostockminimo": 10,
  "prdtostockmaximo": 50,
  "prdtoestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "prdtoid": "46ab42f3-60fc-42af-8d52-ff769ea40c78",
  "prdtoemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "categoria": {
    "ctgriaid": "d9475c59-4fcc-469a-ad8f-3f9f28190058",
    "ctgnombre": "Herramientas Pequeñas",
    "ctgriadescripcion": "Descripción actualizada"
  },
  "marca": {
    "mrcid": "4ba16156-e626-4fe3-8b5c-0d8ab3f19674",
    "mrcnombre": "Acme Tools"
  },
  "proveedor": {
    "provid": "6d50fa31-54c0-4ea3-a798-19f4801b70de",
    "provnombre": "Proveedor Centro"
  },
  "medida": {
    "mdiaid": "51ed3597-837e-42d1-9bf7-79a30e00f51d",
    "mdianombre": "Unidad",
    "mdiaabreviatura": "UND"
  },
  "prdtocodigo": "012345678901",
  "prdtonombre": "Brocha 1\"",
  "prdtopreciocompra": 1.25,
  "prdtoprecioventa": 1.8,
  "prdtostockminimo": 10,
  "prdtostockmaximo": 50,
  "prdtoimagen": "http://localhost:3000/uploads/productos/cemento_test.png",
  "prdtofchregistro": "2026-05-23T23:21:23.477Z",
  "prdtoestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar productos de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- Si el producto está en estado `eliminado`, no se permite actualizar.
- Si se envía `prdtoctgriaid`, `prdtomrcid`, `prdtoprovid` o `prdtomdiaid`, se valida que existan en la misma empresa.
- Si se envía `prdtocodigo`, se valida que no exista duplicado dentro de la misma empresa.
- Si se envía `prdtonombre`, se valida que no exista duplicado dentro de la misma empresa.
- Debe enviarse al menos un campo para actualizar.
- `prdtoimagen` en la respuesta se entrega como URL pública del backend.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request` con mensaje `Product id is required`.
- Body vacío o sin cambios aplicables: `500 Internal Server Error` con mensaje `At least one field is required to update product`.
- `prdtoctgriaid` vacío: `500 Internal Server Error` con mensaje `Product category id is required`.
- `prdtomrcid` vacío: `500 Internal Server Error` con mensaje `Product brand id is required`.
- `prdtoprovid` vacío: `500 Internal Server Error` con mensaje `Product proveedor id is required`.
- `prdtomdiaid` vacío: `500 Internal Server Error` con mensaje `Product medida id is required`.
- `prdtocodigo` vacío: `500 Internal Server Error` con mensaje `Product code is required`.
- `prdtonombre` vacío: `500 Internal Server Error` con mensaje `Product name is required`.
- `prdtoestado` inválido: `500 Internal Server Error` con mensaje `Product status must be activo, inactivo or eliminado`.
- Campos numéricos vacíos o inválidos: `500 Internal Server Error` con mensaje `Number is required` o `Value must be a valid number`.
- Imagen inválida: `500 Internal Server Error` con mensaje `Image size exceeds the allowed limit` o `Only PNG and JPG images are allowed`.
- Categoría inexistente: `500 Internal Server Error` con mensaje `Product category does not exist`.
- Marca inexistente: `500 Internal Server Error` con mensaje `Product brand does not exist`.
- Proveedor inexistente: `500 Internal Server Error` con mensaje `Product proveedor does not exist`.
- Medida inexistente: `500 Internal Server Error` con mensaje `Product medida does not exist`.
- Producto duplicado por código: `500 Internal Server Error` con mensaje `Product already exists with that code`.
- Producto duplicado por nombre: `500 Internal Server Error` con mensaje `Product already exists with that name`.
- Producto en estado `eliminado`: `500 Internal Server Error` con mensaje `Deleted product cannot be updated`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Producto no encontrado: `404 Not Found` con mensaje `Product not found`.
