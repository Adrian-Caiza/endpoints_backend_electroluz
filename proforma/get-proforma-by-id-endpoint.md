# GET /proformas/:id

## Descripción
Obtiene una proforma específica por su identificador (`prfmaid`) para la empresa del usuario autenticado.

La respuesta usa la estructura completa de proforma (igual a `POST /proformas` y a cada item de `GET /proformas`).

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Params
- `id` (string, requerido): id de la proforma (`prfmaid`).

Ejemplo:

`GET /proformas/66ff3afe-65bb-49f2-80cf-ae279b7fcf5b`

## Respuesta Exitosa
`200 OK`

```json
{
  "proforma": {
    "prfmaid": "66ff3afe-65bb-49f2-80cf-ae279b7fcf5b",
    "prfmaidentificador": "FE01-002-001-34",
    "prfmaestado": "emitida",
    "prfmafchregistro": "2026-05-25T04:35:56.719Z",
    "prfmafchactualizacion": "2026-05-25T04:35:56.719Z",
    "emisor": {
      "empresa": {
        "emid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
        "emlogo": "http://localhost:3000/uploads/empresas/Mujer_portada.png",
        "emrznsocial": "Ferreconst ElectroLuz K y B",
        "emruc": "1709639106001",
        "emcorreo": "electrokyb@gmail.com",
        "emcodigo": "FE01"
      },
      "sucursal": {
        "suid": "2079f9a4-2676-4601-9c87-cfb81edb70e4",
        "sunombre": "Sucursal Centro Historico Basilica",
        "suidentificador": "002"
      },
      "caja": {
        "cjid": "2dbe2030-46d6-4960-bd5e-fafb6719540f",
        "cjidentificador": "001"
      },
      "usuario": {
        "usid": "428331bb-b892-4bff-b9dc-26260c68e7f4",
        "usnombre": "Erick Nuñez",
        "usrol": "jefe"
      }
    },
    "receptor": {
      "cliente": {
        "clnteid": "f969e3c1-cd5f-4286-8b5a-81ae9badfa3e",
        "clntenombre": "Juan Perez",
        "clnteidentificacion": "1712345678",
        "clntecorreo": "juan.perez@email.com",
        "clntetelefono": "0987654321",
        "clntedireccion": "Av. Principal y Calle 10"
      }
    },
    "metodoPago": {
      "mpid": "12421636-dde1-44f1-b36c-9bd81bee22af",
      "mpnombre": "Efectivo"
    },
    "detalle": [
      {
        "dprfmaid": "560214e7-69d2-487d-b959-aecc358582a9",
        "dprfmatipoitem": "manual",
        "producto": {
          "dprfmacodigo": null,
          "dprfmadescripcion": "Servicio de instalación y ajuste",
          "dprfmacantidad": 1,
          "dprfmapreciounitario": 10,
          "dprfmapreciototal": 10
        }
      },
      {
        "dprfmaid": "a0aee57a-277e-41d9-987d-4da410746a3b",
        "dprfmatipoitem": "inventariable",
        "producto": {
          "dprfmacodigo": "012344678901",
          "dprfmadescripcion": "Brocha 2\"",
          "dprfmacantidad": 1,
          "dprfmapreciounitario": 25,
          "dprfmapreciototal": 25
        }
      }
    ],
    "total": {
      "prfmasubtotal": 35,
      "prfmadescuento": 3,
      "prfmatotal": 32
    },
    "documento": {
      "docnombre": "FE01-002-001-34_2026-05-25.pdf",
      "docurl": "http://localhost:3000/uploads/proformas/1709639106001/FE01-002-001-34_2026-05-25.pdf"
    }
  }
}
```

## Comportamiento relevante
- Solo retorna proformas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- Si la proforma no existe o no pertenece al tenant del usuario, devuelve `404`.

## Errores comunes
- `401 Unauthorized`: token inválido o ausente.
- `404 Not Found`: `Proforma not found`.
- `400 Bad Request`: `Proforma id is required` (si el parámetro no llega correctamente).
- `500 Internal Server Error` con mensaje de negocio (comportamiento actual del middleware para errores sin `statusCode`).

## Notas
- Este endpoint no modifica estado ni detalle; solo lectura.
- `detalle[].dprfmatipoitem` se deriva de `dprfmaesinventariable`.
- El PDF queda anidado en `proforma.documento`.
