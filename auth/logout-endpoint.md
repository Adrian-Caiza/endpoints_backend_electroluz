# POST /auth/logout

## Descripción
Cierra la sesión revocando el `refreshToken` recibido.

## Autenticación
No requiere `Authorization` header.

## Content-Type
- `application/json`

## Body (obligatorio)
- `refreshToken`: token de refresco que se desea revocar.

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
  "message": "Session closed successfully"
}
```

## Comportamiento relevante
- El backend revoca el refresh token por hash.
- Si el token ya estaba revocado o no existe, el endpoint mantiene respuesta de éxito para no filtrar estado de sesión.

## Errores comunes
- Body sin `refreshToken`: `500 Internal Server Error` con mensaje `Refresh token is required`.
- Error inesperado: `500 Internal Server Error`.
