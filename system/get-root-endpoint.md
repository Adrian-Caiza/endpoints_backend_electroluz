# GET /

## Descripción
Endpoint base de verificación del servicio. Permite validar que la API está levantada.

## Autenticación
No requiere autenticación.

## Ejemplo
`GET /`

## Respuesta Exitosa
`200 OK`

```json
{
  "name": "esnt-backend-ferreteria",
  "version": "1.0.0"
}
```

## Errores comunes
- `500 Internal Server Error`: error inesperado en el servidor.
