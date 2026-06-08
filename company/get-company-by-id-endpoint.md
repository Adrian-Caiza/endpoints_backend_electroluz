# GET /companies/:id

## Descripción
Obtiene una empresa por su identificador.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la empresa.

Ejemplo:

`GET /companies/0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a`

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
- El backend transforma `emlogo` a URL pública completa.
- El usuario autenticado debe pertenecer a la empresa y estar activo.
- Solo permite consultar la misma empresa del usuario autenticado.
- Los roles permitidos para este endpoint son `jefe`, `empleado` o `administrador`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- Empresa no encontrada: `500 Internal Server Error` con mensaje `Company not found` (comportamiento actual del servicio).
- Intento de consultar otra empresa: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa no activa: `500 Internal Server Error` con mensaje `Company is not active`.
