# POST /auth/login

## Descripción
Inicia sesión de usuario y retorna un `accessToken` y un `refreshToken`.

## Autenticación
No requiere token JWT.

## Content-Type
- `application/json`

## Body (obligatorios)
- `emruc`: RUC de la empresa.
- `usapodo`: apodo del usuario.
- `uspassword`: contraseña del usuario.

## Ejemplo (JSON)

```json
{
  "emruc": "1709639106001",
  "usapodo": "jperez",
  "uspassword": "secreto123"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "accessToken": "<jwt_access_token>",
  "refreshToken": "<opaque_refresh_token>",
  "company": {
    "emid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
    "emruc": "1709639106001",
    "emrznsocial": "Ferreconst ElectroLuz K y B",
    "emlogo": "http://localhost:3000/uploads/empresas/company.png",
    "emestado": "activo",
    "empadre": false
  },
  "user": {
    "usid": "2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07",
    "usemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
    "usnombre": "Juan Perez",
    "usapodo": "jperez",
    "uscorreo": "juan.perez@empresa.com",
    "usimagen": "http://localhost:3000/uploads/usuarios/user.png",
    "usrol": "jefe",
    "usestado": "activo"
  }
}
```

## Comportamiento relevante
- `accessToken` se usa para endpoints protegidos (`Authorization: Bearer <token>`).
- `refreshToken` se usa para renovar sesión en `POST /auth/refresh`.
- El backend valida usuario y empresa en estado `activo`.
- El backend registra metadatos del token de refresco con `ip` y `user-agent` cuando están disponibles.

## Errores comunes
- Credenciales inválidas: `401 Unauthorized`.
- Bloqueo temporal por 3 intentos fallidos: `429 Too Many Requests`.
- RUC ausente: `500 Internal Server Error` con mensaje `Company RUC is required`.
- RUC inválido: `500 Internal Server Error` con mensaje `RUC must be valid`.
- `usapodo` ausente: `500 Internal Server Error` con mensaje `Nickname is required`.
- `uspassword` ausente: `500 Internal Server Error` con mensaje `Password is required`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is inactive`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is inactive`.
- Error inesperado: `500 Internal Server Error`.

## Notas de seguridad (login)
- El bloqueo temporal se aplica por combinación `emruc + usapodo + ip`.
- Después de 3 intentos fallidos consecutivos, el login se bloquea por 15 minutos.
- El conteo de intentos también aplica cuando el usuario no existe, para evitar enumeración de cuentas.
- Un login exitoso reinicia el contador y desbloquea la combinación evaluada.
- El mensaje del bloqueo es `Too many failed login attempts. Try again in 15 minutes`.
