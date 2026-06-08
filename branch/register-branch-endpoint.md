# POST /branches

## Descripción
Crea una sucursal para la empresa enviada en el body, siempre que coincida con la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Body
- `suemid`: id de la empresa.
- `sunombre`: nombre de la sucursal.
- `suidentificador`: identificador de sucursal (exactamente 3 dígitos).
- `sudireccion`: dirección de la sucursal (opcional).
- `sucorreo`: correo de la sucursal (opcional, formato válido).

## Ejemplo

```json
{
  "suemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "sunombre": "Sucursal Centro",
  "suidentificador": "001",
  "sudireccion": "Av. Principal y Calle 10",
  "sucorreo": "sucursal.centro@empresa.com"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "suemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "sunombre": "Sucursal Centro",
  "sudireccion": "Av. Principal y Calle 10",
  "sucorreo": "sucursal.centro@empresa.com",
  "suidentificador": "001",
  "sufchregistro": "2026-05-19T22:10:00.000Z",
  "suestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear sucursales en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` puede crear sucursales.
- No permite repetir `suidentificador` dentro de la misma empresa.
- Al crear la sucursal, también intenta crear su secuencia asociada.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `suemid` ausente: `500 Internal Server Error` con mensaje `Company id is required`.
- `sunombre` ausente: `500 Internal Server Error` con mensaje `Branch name is required`.
- `suidentificador` ausente: `500 Internal Server Error` con mensaje `Branch identifier is required`.
- `suidentificador` inválido: `500 Internal Server Error` con mensaje `Branch identifier must be exactly 3 digits number`.
- `sucorreo` inválido: `500 Internal Server Error` con mensaje `Branch email must be valid`.
- Usuario sin rol `jefe`: `500 Internal Server Error` con mensaje `User is not jefe`.
- Intento de crear para otra empresa: `500 Internal Server Error` con mensaje `User cannot create branch for another company`.
- Identificador duplicado: `500 Internal Server Error` con mensaje `Branch already exists with that identifier`.
