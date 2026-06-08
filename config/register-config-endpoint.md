# POST /configs

## Descripción
Crea una configuración para una empresa.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json`

## Body (obligatorios)
- `cfemid`: id de la empresa a la que pertenecerá la configuración.
- `cfclave`: clave única de configuración.
- `cfvalor`: valor asociado a la clave.

## Ejemplo (JSON)

```json
{
  "cfemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "cfclave": "iva_porcentaje",
  "cfvalor": "15"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "cfid": "5b9d0e5e-fd3f-451d-bf68-4db51f52f6b0",
  "cfemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "cfclave": "iva_porcentaje",
  "cfvalor": "15"
}
```

## Comportamiento relevante
- Solo permite crear configuraciones a un usuario con rol `administrador`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe ser empresa padre (`empadre = true`).
- No permite duplicar `cfclave` dentro de la misma empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `cfemid`, `cfclave` o `cfvalor` ausentes: error de validación.
- Usuario sin rol `administrador`: `User is not admin`.
- Empresa no activa o no padre: error de negocio.
- Clave duplicada: `Config already exists with that key`.
