# POST /checkouts

## Descripción
Crea una caja en una sucursal de la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Body
- `cjemid`: id de la empresa.
- `cjsuid`: id de la sucursal.
- `cjidentificador`: identificador de caja (exactamente 3 dígitos).

## Ejemplo

```json
{
  "cjemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "cjsuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "cjidentificador": "001"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "cjid": "e1b3da39-d5d5-47d6-a351-0e61e586f732",
  "cjemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "cjsuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "cjidentificador": "001",
  "cjfchregistro": "2026-05-19T22:10:00.000Z",
  "cjestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear cajas en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- La sucursal debe existir y estar activa.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.
- No permite repetir `cjidentificador` dentro de la misma sucursal (`409 Conflict`).

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `cjemid`, `cjsuid` o `cjidentificador` ausentes: error de validación.
- `cjidentificador` inválido (no 3 dígitos): error de validación.
- Sucursal no encontrada o inactiva: error de negocio.
- Caja duplicada en la misma sucursal: `409 Conflict` con mensaje `Checkout identifier already exists in this branch`.
