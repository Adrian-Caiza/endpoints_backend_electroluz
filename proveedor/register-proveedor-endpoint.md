# POST /proveedores

## Descripción
Crea un nuevo proveedor para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Body
- `provemid`: id de la empresa.
- `provnombre`: nombre del proveedor.
- `provtelefono`: teléfono del proveedor (formato móvil de 10 dígitos, por ejemplo `0984653471`).
- `provctgriaid`: id de la categoría (opcional, puede ser `null`).
- `provmrcid`: id de la marca (opcional, puede ser `null`).
- `provcorreo`: correo del proveedor (opcional, puede ser `null`).

## Ejemplo

```json
{
  "provemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "provctgriaid": "85cdca30-3008-466b-b896-52d588cdda10",
  "provmrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
  "provnombre": "DeWalt Centro Historico",
  "provtelefono": "0984653471",
  "provcorreo": null
}
```

## Respuesta Exitosa
`201 Created`

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
  "provnombre": "DeWalt Centro Historico",
  "provtelefono": "0984653471",
  "provcorreo": null,
  "provfchregistro": "2026-05-23T17:29:01.621Z",
  "provestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear proveedores en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- No permite repetir `provnombre` dentro de la misma empresa.
- Si `provctgriaid` es enviado, la categoría debe existir en la misma empresa y estar activa.
- Si `provmrcid` es enviado, la marca debe existir en la misma empresa y estar activa.
- `categoria` y `marca` pueden ser `null` cuando no se envían relaciones.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `provemid` ausente o vacío: `500 Internal Server Error` con mensaje `Company id is required`.
- `provnombre` ausente o vacío: `500 Internal Server Error` con mensaje `Proveedor name is required`.
- `provtelefono` ausente o vacío: `500 Internal Server Error` con mensaje `Proveedor phone is required`.
- `provtelefono` con formato inválido: `500 Internal Server Error` con mensaje `Proveedor phone must be valid`.
- `provcorreo` con formato inválido: `500 Internal Server Error` con mensaje `Proveedor email must be valid`.
- `provctgriaid` vacío: `500 Internal Server Error` con mensaje `Proveedor category id is required`.
- `provmrcid` vacío: `500 Internal Server Error` con mensaje `Proveedor brand id is required`.
- Empresa distinta a la del usuario autenticado: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Proveedor duplicado por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Proveedor already exists with that name`.
- Categoría inexistente: `500 Internal Server Error` con mensaje `Proveedor category does not exist`.
- Categoría inactiva o eliminada: `500 Internal Server Error` con mensaje `Proveedor category is not active`.
- Marca inexistente: `500 Internal Server Error` con mensaje `Proveedor brand does not exist`.
- Marca inactiva o eliminada: `500 Internal Server Error` con mensaje `Proveedor brand is not active`.
