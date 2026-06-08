# POST /categories

## Descripción
Crea una nueva categoría para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Body
- `ctgriaemid`: id de la empresa.
- `ctgnombre`: nombre de la categoría.
- `ctgriadescripcion`: descripción de la categoría (opcional).

## Ejemplo

```json
{
  "ctgriaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "ctgnombre": "Herramientas Manuales",
  "ctgriadescripcion": "Productos para uso manual en ferretería"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "ctgriaid": "3c871a9e-0f6b-4ac9-a18f-0ecfe8b96d90",
  "ctgriaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "ctgnombre": "Herramientas Manuales",
  "ctgriadescripcion": "Productos para uso manual en ferretería",
  "ctgriafchregistro": "2026-05-21T18:40:00.000Z",
  "ctgriaestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear categorías en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede crear categorías.
- No permite repetir `ctgnombre` dentro de la misma empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `ctgriaemid` ausente: `500 Internal Server Error` con mensaje `Company id is required`.
- `ctgnombre` ausente: `500 Internal Server Error` con mensaje `Category name is required`.
- Intento de crear para otra empresa: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Categoría duplicada por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Category already exists with that name`.
