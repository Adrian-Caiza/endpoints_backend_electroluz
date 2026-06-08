# GET /configs

## Descripción
Obtiene todas las configuraciones de una empresa.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params
- `companyId` (opcional): id de la empresa a consultar. Si no se envía, usa la empresa del usuario autenticado.

Ejemplos:

`GET /configs`

`GET /configs?companyId=4ff4db6b-f18f-4ecd-83b3-b997fa77a01e`

## Respuesta Exitosa
`200 OK`

```json
[
  {
    "cfid": "5b9d0e5e-fd3f-451d-bf68-4db51f52f6b0",
    "cfemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
    "cfclave": "iva_porcentaje",
    "cfvalor": "15"
  },
  {
    "cfid": "6c3f8e47-cd2a-4f25-91d2-94cc2b8a6b1d",
    "cfemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
    "cfclave": "moneda",
    "cfvalor": "USD"
  }
]
```

## Comportamiento relevante
- Solo permite consultar configuraciones a un usuario con rol `administrador`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe ser empresa padre (`empadre = true`).
- Si envías `companyId`, la consulta se ejecuta para esa empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- Usuario sin rol `administrador`: `User is not admin`.
- Empresa no activa o no padre: error de negocio.
