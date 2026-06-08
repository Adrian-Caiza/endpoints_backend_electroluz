# PATCH /companies/:id

## Descripción
Actualiza los datos de una empresa y retorna la empresa con los valores actualizados.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json` (sin imagen)
- `multipart/form-data` (con imagen)

## Path Param
- `id`: identificador de la empresa.

Ejemplo:

`PATCH /companies/0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a`

## Body
- `emrznsocial` (opcional): razón social.
- `emcorreo` (opcional): correo de la empresa.
- `imagen` (opcional, archivo): logo de la empresa.

## Reglas de imagen (si se envía)
- Campo: `imagen`
- Formatos permitidos: `.png`, `.jpg`
- Tamaño máximo: `5MB`

## Ejemplo (JSON)

```json
{
  "emrznsocial": "Ferreteria Central S.A.",
  "emcorreo": "contacto@ferreteriacentral.com"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "emid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
  "emruc": "1790012345001",
  "emrznsocial": "Ferreteria Central S.A.",
  "emcorreo": "contacto@ferreteriacentral.com",
  "emlogo": "https://api.tudominio.com/uploads/empresas/company.png",
  "emcodigo": "FC01",
  "emfchregistro": "2026-05-17T15:20:10.000Z",
  "emestado": "activo"
}
```

## Comportamiento relevante
- El endpoint solo permite actualizar la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Los roles permitidos para este endpoint son `jefe`, `empleado` o `administrador`.
- Si la empresa objetivo está `inactivo` o `eliminado`, no se permite actualizar.
- Debe enviarse al menos un campo para actualizar (`emrznsocial`, `emcorreo` o `imagen`).
- Si se envía `emcorreo` y ese correo ya existe, se retorna error.
- El backend transforma `emlogo` a URL pública completa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `emrznsocial` vacía: `500 Internal Server Error` con mensaje `Company social reason is required`.
- `emcorreo` inválido: `500 Internal Server Error` con mensaje `Company email must be valid`.
- `imagen` inválida por tamaño: `500 Internal Server Error` con mensaje `Image size exceeds the allowed limit`.
- `imagen` inválida por tipo: `500 Internal Server Error` con mensaje `Only PNG and JPG images are allowed`.
- Body sin campos para actualizar: `500 Internal Server Error` con mensaje `At least one field is required to update company`.
- Correo duplicado: `500 Internal Server Error` con mensaje `Company already exists with that email`.
- Empresa no encontrada: `404 Not Found`.
- Empresa inactiva o eliminada: `500 Internal Server Error` con mensaje `Inactive or deleted company cannot be updated`.
- Intento de actualizar otra empresa: `500 Internal Server Error` con mensaje `User cannot access another company`.
