# POST /brands

## Descripción
Crea una nueva marca para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Body
- `mrcemid`: id de la empresa.
- `mrcnombre`: nombre de la marca.

## Ejemplo

```json
{
  "mrcemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mrcnombre": "Truper"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "mrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
  "mrcemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mrcnombre": "Truper",
  "mrcfchregistro": "2026-05-21T20:30:00.000Z",
  "mrcestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear marcas en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede crear marcas.
- No permite repetir `mrcnombre` dentro de la misma empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `mrcemid` ausente: `500 Internal Server Error` con mensaje `Company id is required`.
- `mrcnombre` ausente: `500 Internal Server Error` con mensaje `Brand name is required`.
- Intento de crear para otra empresa: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Marca duplicada por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Brand already exists with that name`.
