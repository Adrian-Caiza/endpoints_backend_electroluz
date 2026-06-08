# GET /configs/:configKey

## Descripción
Obtiene una configuración puntual usando su clave.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `configKey`: clave de configuración.

## Query Params (obligatorios)
- `companyId`: id de la empresa donde se buscará la clave.

Ejemplo:

`GET /configs/iva_porcentaje?companyId=4ff4db6b-f18f-4ecd-83b3-b997fa77a01e`

## Respuesta Exitosa
`200 OK`

```json
{
  "cfid": "5b9d0e5e-fd3f-451d-bf68-4db51f52f6b0",
  "cfemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "cfclave": "iva_porcentaje",
  "cfvalor": "15"
}
```

## Comportamiento relevante
- Solo permite consultar configuraciones a un usuario con rol `administrador`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe ser empresa padre (`empadre = true`).
- Si la clave no existe para la empresa indicada, responde `404`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `configKey` ausente: `400 Bad Request`.
- `companyId` ausente: `400 Bad Request`.
- Usuario sin rol `administrador`: `User is not admin`.
- Configuración no encontrada: `404 Not Found`.
