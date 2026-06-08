# PATCH /checkouts/:id/status

## Descripción
Actualiza únicamente el estado de una caja por su identificador de fila (`cjid`), limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador interno de la caja (`cjid`).

Ejemplo:

`PATCH /checkouts/e1b3da39-d5d5-47d6-a351-0e61e586f732/status`

## Body
- `cjestado` (requerido): estado de la caja.

Valores permitidos para `cjestado`:
- `activo`
- `inactivo`
- `eliminado`

## Ejemplo (JSON)

```json
{
  "cjestado": "inactivo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "cjid": "e1b3da39-d5d5-47d6-a351-0e61e586f732",
  "cjemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "cjsuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "cjidentificador": "002",
  "cjfchregistro": "2026-05-19T22:10:00.000Z",
  "cjestado": "inactivo",
  "sucursal": {
    "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
    "sunombre": "Sucursal Centro",
    "suidentificador": "001",
    "suestado": "activo"
  }
}
```

## Comportamiento relevante
- Solo permite actualizar cajas de la misma empresa del usuario autenticado.
- Incluye en la respuesta los datos básicos de sucursal: `suid`, `sunombre`, `suidentificador`, `suestado`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` puede actualizar una caja.
- Si la caja está en estado `eliminado`, no se permite actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `cjestado` inválido o ausente: error de validación.
- Caja no encontrada: `404 Not Found`.
- Intento de actualización sin rol `jefe`: error de autorización.
