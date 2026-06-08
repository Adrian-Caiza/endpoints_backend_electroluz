# PATCH /configs/:configKey

## Descripción
Actualiza el valor de una configuración existente.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `configKey`: clave de configuración.

## Query Params (obligatorios)
- `companyId`: id de la empresa donde se actualizará la clave.

## Body (obligatorio)
- `cfvalor`: nuevo valor de la configuración.

## Ejemplo (JSON)

```json
{
  "cfvalor": "16"
}
```

Ejemplo de ruta:

`PATCH /configs/iva_porcentaje?companyId=4ff4db6b-f18f-4ecd-83b3-b997fa77a01e`

## Respuesta Exitosa
`200 OK`

```json
{
  "cfid": "5b9d0e5e-fd3f-451d-bf68-4db51f52f6b0",
  "cfemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "cfclave": "iva_porcentaje",
  "cfvalor": "16"
}
```

## Comportamiento relevante
- Solo permite actualizar configuraciones a un usuario con rol `administrador`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe ser empresa padre (`empadre = true`).
- Debe enviarse `cfvalor`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `configKey` ausente: `400 Bad Request`.
- `companyId` ausente: `400 Bad Request`.
- `cfvalor` ausente: error de validación.
- Usuario sin rol `administrador`: `User is not admin`.
- Configuración no encontrada: `404 Not Found`.
