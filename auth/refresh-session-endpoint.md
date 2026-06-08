# POST /auth/refresh

## Descripción
Renueva la sesión usando el `refreshToken` y retorna un nuevo par de tokens.

## Autenticación
No requiere `Authorization` header.

## Content-Type
- `application/json`

## Body (obligatorio)
- `refreshToken`: token de refresco vigente.

## Ejemplo (JSON)

```json
{
  "refreshToken": "<opaque_refresh_token>"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "accessToken": "<new_jwt_access_token>",
  "refreshToken": "<new_opaque_refresh_token>"
}
```

## Comportamiento relevante
- El refresh token se valida por hash en base de datos.
- Si el token es válido, se revoca el token actual y se genera uno nuevo (rotación).
- Se valida que usuario y empresa estén activos.
- El nuevo refresh token se emite guardando `ip` y `user-agent` del request cuando están disponibles.
- Si el refresh token ya expiró, primero se revoca y luego se responde `401`.

## Errores comunes
- Refresh token inválido/revocado/expirado: `401 Unauthorized`.
- `refreshToken` ausente: `500 Internal Server Error` con mensaje `Refresh token is required`.
- Usuario inactivo: `401 Unauthorized`.
- Empresa inactiva: `401 Unauthorized`.
- Error inesperado: `500 Internal Server Error`.
