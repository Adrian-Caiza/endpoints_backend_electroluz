# GET /alerts/events

## Descripción
Abre una conexión SSE (`Server-Sent Events`) para recibir alertas nuevas de la empresa del usuario autenticado en tiempo real.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params
No requiere query params.

Ejemplo:

`GET /alerts/events`

## Respuesta Exitosa
`200 OK`

Responde como stream con content type:

`text/event-stream`

Al abrir la conexión, el servidor envía un evento inicial:

```text
event: connected
data: {}
```

Cuando detecta nuevas alertas visibles, envía eventos `new-alert`:

```text
event: new-alert
data: {"alid":"8b6e1f4a-4ad8-4d9e-9a0f-8e58b0df1111","alemid":"4ff4db6b-f18f-4ecd-83b3-b997fa77a01e","branch":{"suid":"9a9fbc8e-1ed5-45a5-8e4d-7e5c31790001","sunombre":"Sucursal Norte","suidentificador":"NORTE-01"},"product":{"prdtoid":"0b8f4ef4-cd8c-4ebf-a385-16ef7f380001","prdtocodigo":"MART-001","prdtonombre":"Martillo 16oz"},"altipo":"stock_bajo","almensaje":"Stock bajo en Sucursal Norte: Martillo 16oz (MART-001) - Actual: 2, Mínimo: 5","alcantidadactual":2,"alstockminimo":5,"alstockmaximo":20,"alvisible":true,"alvisto":false,"alfchcreacion":"2026-06-06T22:55:11.000Z"}
```

Además, el servidor envía líneas de keepalive periódicas:

```text
:keepalive
```

## Comportamiento relevante
- Solo emite alertas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.
- La conexión queda abierta hasta que el cliente la cierre.
- El servidor hace polling interno de alertas recientes cada 5 segundos.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe, empleado or admin`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.

