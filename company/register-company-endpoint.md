# POST /companies

## Descripción
Registra una nueva empresa.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json` (sin imagen)
- `multipart/form-data` (con imagen)

## Body
- `emruc`: RUC de la empresa.
- `emrznsocial`: razón social.
- `emcorreo`: correo de la empresa.
- `emcodigo`: código alfanumérico de 4 caracteres.
- `imagen` (opcional, archivo): logo de la empresa.

## Reglas de imagen (si se envía)
- Campo: `imagen`
- Formatos permitidos: `.png`, `.jpg`
- Tamaño máximo: `5MB`
- Si no se envía, se usa logo por defecto.

## Ejemplo (JSON)

`POST /companies`

```json
{
  "emruc": "1790012345001",
  "emrznsocial": "Ferreteria Central S.A.",
  "emcorreo": "contacto@ferreteriacentral.com",
  "emcodigo": "FC01"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "emid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
  "emruc": "1790012345001",
  "emrznsocial": "Ferreteria Central S.A.",
  "emcorreo": "contacto@ferreteriacentral.com",
  "emlogo": "https://api.tudominio.com/uploads/empresas/company.png",
  "emcodigo": "FC01",
  "emfchregistro": "2026-05-23T17:29:01.621Z",
  "emestado": "activo"
}
```

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `emruc` ausente: `500 Internal Server Error` con mensaje `Company RUC is required`.
- `emruc` inválido: `500 Internal Server Error` con mensaje `RUC must be valid`.
- `emrznsocial` ausente: `500 Internal Server Error` con mensaje `Company social reason is required`.
- `emcorreo` ausente o inválido: `500 Internal Server Error` con mensaje `Company email must be valid`.
- `emcodigo` ausente o inválido: `500 Internal Server Error` con mensaje `Company code must be exactly 4 alphanumeric characters`.
- `imagen` inválida por tamaño: `500 Internal Server Error` con mensaje `Image size exceeds the allowed limit`.
- `imagen` inválida por tipo: `500 Internal Server Error` con mensaje `Only PNG and JPG images are allowed`.
- Usuario sin rol `administrador`: `500 Internal Server Error` con mensaje `User is not admin`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa autenticada no es empresa padre: `500 Internal Server Error` con mensaje `Company is not parent`.
- Empresa duplicada por RUC: `500 Internal Server Error` con mensaje `Company already exists with that RUC`.
- Empresa duplicada por correo: `500 Internal Server Error` con mensaje `Company already exists with that email`.
- Empresa duplicada por código: `500 Internal Server Error` con mensaje `Company already exists with that code`.
