# GET /checkouts/:id

## Descripción
Obtiene una caja por su identificador dentro de una sucursal específica, limitada a la empresa del usuario autenticado, incluyendo datos básicos de la sucursal.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la caja (`cjidentificador`).

## Query Param (obligatorio)
- `cjsuid`: id de la sucursal donde se buscará la caja.

Ejemplo:

`GET /checkouts/001?cjsuid=30d3385c-445d-4f2f-97f9-3d3f88c052f1`

## Respuesta Exitosa
`200 OK`

```json
{
  "cjid": "e1b3da39-d5d5-47d6-a351-0e61e586f732",
  "cjemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "cjsuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "cjidentificador": "001",
  "cjfchregistro": "2026-05-19T22:10:00.000Z",
  "cjestado": "activo",
  "sucursal": {
    "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
    "sunombre": "Sucursal Centro",
    "suidentificador": "001",
    "suestado": "activo"
  }
}
```

## Comportamiento relevante
- La búsqueda se hace por `cjemid` (empresa del usuario autenticado), `cjsuid` (query param) y `cjidentificador` (path param).
- Esto permite repetir `cjidentificador` en sucursales distintas sin colisiones.
- Incluye datos básicos de sucursal: `suid`, `sunombre`, `suidentificador`, `suestado`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente: `400 Bad Request`.
- `cjsuid` ausente: `400 Bad Request`.
- Caja no encontrada para esa empresa+sucursal+identificador: error de negocio.
