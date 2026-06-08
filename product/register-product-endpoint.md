# POST /products

## Descripción
Crea un nuevo producto para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json` (sin imagen)
- `multipart/form-data` (con imagen)

## Body (obligatorios)
- `prdtoemid`: id de la empresa.
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
- `imagen` (opcional, archivo): imagen del producto.

## Reglas de imagen (si se envía)
- Campo: `imagen`
- Formatos permitidos: `.png`, `.jpg`
- Tamaño máximo: `5MB`
- Si no se envía, se usa imagen por defecto.

## Ejemplo (JSON)

```json
{
  "prdtoemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "prdtoctgriaid": "85cdca30-3008-466b-b896-52d588cdda10",
  "prdtomrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
  "prdtoprovid": "a052ef32-3c4b-41da-871a-dfec99d8afa3",
  "prdtomdiaid": "7f4f7a99-f7dc-4b10-9326-9e4ec8300e89",
  "prdtocodigo": "PRD-0001",
  "prdtonombre": "Taladro Inalambrico 20V",
  "prdtopreciocompra": 120.5,
  "prdtoprecioventa": 165.99,
  "prdtostockminimo": 5,
  "prdtostockmaximo": 50
}
```

## Respuesta Exitosa
`201 Created`

```json
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
```

## Comportamiento relevante
- Solo permite crear productos en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- `prdtoctgriaid`, `prdtomrcid`, `prdtoprovid` y `prdtomdiaid` deben existir en la misma empresa.
- No permite repetir `prdtocodigo` dentro de la misma empresa.
- No permite repetir `prdtonombre` dentro de la misma empresa.
- Si no se envía imagen, el backend guarda la ruta por defecto `/uploads/productos/product.png`.
- `prdtoimagen` en la respuesta se entrega como URL pública del backend.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `prdtoemid` ausente o vacío: `500 Internal Server Error` con mensaje `Company id is required`.
- `prdtoctgriaid` ausente o vacío: `500 Internal Server Error` con mensaje `Product category id is required`.
- `prdtomrcid` ausente o vacío: `500 Internal Server Error` con mensaje `Product brand id is required`.
- `prdtoprovid` ausente o vacío: `500 Internal Server Error` con mensaje `Product proveedor id is required`.
- `prdtomdiaid` ausente o vacío: `500 Internal Server Error` con mensaje `Product medida id is required`.
- `prdtocodigo` ausente o vacío: `500 Internal Server Error` con mensaje `Product code is required`.
- `prdtonombre` ausente o vacío: `500 Internal Server Error` con mensaje `Product name is required`.
- Campos numéricos vacíos o inválidos: `500 Internal Server Error` con mensaje `Number is required` o `Value must be a valid number`.
- Imagen inválida: `500 Internal Server Error` con mensaje `Image size exceeds the allowed limit` o `Only PNG and JPG images are allowed`.
- Empresa distinta a la del usuario autenticado: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Categoría inexistente: `500 Internal Server Error` con mensaje `Product category does not exist`.
- Marca inexistente: `500 Internal Server Error` con mensaje `Product brand does not exist`.
- Proveedor inexistente: `500 Internal Server Error` con mensaje `Product proveedor does not exist`.
- Medida inexistente: `500 Internal Server Error` con mensaje `Product medida does not exist`.
- Producto duplicado por código: `500 Internal Server Error` con mensaje `Product already exists with that code`.
- Producto duplicado por nombre: `500 Internal Server Error` con mensaje `Product already exists with that name`.
