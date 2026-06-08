# POST /medidas

## Descripción
Crea una nueva medida para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Body
- `mdiaemid`: id de la empresa.
- `mdianombre`: nombre de la medida.
- `mdiaabreviatura`: abreviatura de la medida.

## Ejemplo

```json
{
  "mdiaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mdianombre": "Unidad",
  "mdiaabreviatura": "UND"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "mdiaid": "7f4f7a99-f7dc-4b10-9326-9e4ec8300e89",
  "mdiaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mdianombre": "Unidad",
  "mdiaabreviatura": "UND",
  "mdiafchregistro": "2026-05-23T17:29:01.621Z",
  "mdiaestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear medidas en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede crear medidas.
- No permite repetir `mdianombre` dentro de la misma empresa.
- No permite repetir `mdiaabreviatura` dentro de la misma empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `mdiaemid` ausente: `500 Internal Server Error` con mensaje `Company id is required`.
- `mdianombre` ausente: `500 Internal Server Error` con mensaje `Medida name is required`.
- `mdiaabreviatura` ausente: `500 Internal Server Error` con mensaje `Medida abbreviation is required`.
- Intento de crear para otra empresa: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Medida duplicada por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Medida already exists with that name`.
- Medida duplicada por abreviatura en la misma empresa: `500 Internal Server Error` con mensaje `Medida already exists with that abbreviation`.
