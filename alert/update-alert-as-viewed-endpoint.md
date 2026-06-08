# PATCH /alerts/:id/visto

## Descripción
Marca una alerta como vista dentro de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Params
- `id` (string, requerido): id de la alerta (`alid`).

Ejemplo:

`PATCH /alerts/8b6e1f4a-4ad8-4d9e-9a0f-8e58b0df1111/visto`

## Body
No requiere body.

## Respuesta Exitosa
`200 OK`

```json
{
  "message": "Alert marked as viewed"
}
```

## Comportamiento relevante
- Solo permite marcar alertas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.
- Si la alerta no existe en la empresa autenticada, el servicio responde error.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o vacío: `400 Bad Request` o `500 Internal Server Error` con mensaje `Alert id is required`.
- Alerta inexistente en la empresa autenticada: `500 Internal Server Error` con mensaje `Alert not found`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe, empleado or admin`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.

