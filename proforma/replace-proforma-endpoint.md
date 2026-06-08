# PUT /proformas/:id

## Descripcion
Actualiza una proforma completa en una sola operacion: receptor, metodo de
pago, importes y lineas del detalle.

Este endpoint es la ruta principal para guardar el formulario de edicion de
una proforma. La cabecera y el detalle se persisten atomicamente: si alguna
escritura falla, no se conserva una actualizacion parcial.

## Ruta
`PUT /proformas/:id`

## Autenticacion
Requiere JWT:

`Authorization: Bearer <token>`

## Content-Type
`application/json`

## Path Params
- `id` (string, requerido): identificador de la proforma (`prfmaid`).

## Body

### Cabecera editable
- `prfmaclnteid` (string, requerido): id del cliente receptor.
- `prfmampid` (string, requerido): id del metodo de pago.
- `prfmasubtotal` (number, requerido, >= 0): subtotal calculado por frontend.
- `prfmadescuento` (number, requerido, >= 0): descuento calculado por frontend.
- `prfmatotal` (number, requerido, >= 0): total calculado por frontend.
- `dprfmaproductos` (array, requerido, no vacio): representacion completa del detalle final.

No se actualizan por esta ruta: empresa, sucursal, caja, usuario emisor,
identificador secuencial, estado ni fechas enviadas por el cliente.

### Item (`dprfmaproductos[]`)
- `dprfmaid` (string, opcional): id de una linea existente que se desea conservar y actualizar.
- `dprfmaesinventariable` (boolean, requerido): indica si la linea corresponde a inventario.
- `dprfmacodigo` (string, condicional): requerido cuando `dprfmaesinventariable` es `true`.
- `dprfmadescripcion` (string, requerido).
- `dprfmacantidad` (number, requerido, > 0).
- `dprfmapreciounitario` (number, requerido, > 0).
- `dprfmapreciototal` (number, requerido, > 0).

## Conciliacion Del Detalle
- Una linea con `dprfmaid` se actualiza conservando su id.
- Una linea sin `dprfmaid` se inserta y recibe un nuevo id.
- Una linea guardada que no aparece en `dprfmaproductos` se elimina.
- Un `dprfmaid` repetido, inexistente o perteneciente a otra proforma produce `409 Conflict`.

El arreglo representa el detalle completo que debe quedar guardado; no es una
lista parcial de cambios.

## Reglas De Negocio
- Solo una proforma en estado `emitida` puede editarse.
- El cliente y el metodo de pago enviados deben existir y estar activos para la empresa autenticada.
- No se permiten codigos duplicados entre items inventariables.
- Un item inventariable requiere `dprfmacodigo`.
- El codigo de cada item inventariable debe identificar un producto existente y activo de la empresa autenticada.
- `sum(dprfmaproductos[].dprfmapreciototal) == prfmasubtotal`.
- `prfmasubtotal - prfmadescuento == prfmatotal`.
- Se usa tolerancia numerica de `0.0001` al validar importes.
- Esta operacion no modifica stock; el stock se descuenta al pagar la proforma.

## Ejemplo Request

```http
PUT /proformas/66ff3afe-65bb-49f2-80cf-ae279b7fcf5b
Authorization: Bearer <token>
Content-Type: application/json
```

```json
{
  "prfmaclnteid": "f969e3c1-cd5f-4286-8b5a-81ae9badfa3e",
  "prfmampid": "12421636-dde1-44f1-b36c-9bd81bee22af",
  "prfmasubtotal": 35,
  "prfmadescuento": 3,
  "prfmatotal": 32,
  "dprfmaproductos": [
    {
      "dprfmaid": "560214e7-69d2-487d-b959-aecc358582a9",
      "dprfmaesinventariable": false,
      "dprfmadescripcion": "Servicio de instalacion y ajuste",
      "dprfmacantidad": 1,
      "dprfmapreciounitario": 10,
      "dprfmapreciototal": 10
    },
    {
      "dprfmaesinventariable": true,
      "dprfmacodigo": "012344678901",
      "dprfmadescripcion": "Brocha 2\"",
      "dprfmacantidad": 1,
      "dprfmapreciounitario": 25,
      "dprfmapreciototal": 25
    }
  ]
}
```

En este ejemplo se actualiza la linea manual existente, se agrega la brocha y
se eliminan las lineas anteriores que no fueron enviadas.

## Respuesta Exitosa
`200 OK`

La respuesta utiliza la misma estructura completa de `POST /proformas` y
`GET /proformas/:id`:

```json
{
  "proforma": {
    "prfmaid": "66ff3afe-65bb-49f2-80cf-ae279b7fcf5b",
    "prfmaidentificador": "FE01-002-001-34",
    "prfmaestado": "emitida",
    "prfmafchregistro": "2026-05-25T04:35:56.719Z",
    "prfmafchactualizacion": "2026-05-25T14:40:00.000Z",
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
        "usnombre": "Erick Nunez",
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
          "dprfmadescripcion": "Servicio de instalacion y ajuste",
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

## Errores
- `400 Bad Request`: payload incompleto, detalle vacio, bandera/tipos invalidos, valor no positivo o codigo faltante en item inventariable.
- `401 Unauthorized`: token ausente o invalido.
- `404 Not Found`: la proforma no existe para la empresa autenticada.
- `409 Conflict`: la proforma no esta `emitida`, montos inconsistentes, total negativo, producto inventariable duplicado, codigo inventariable inexistente/inactivo o referencia de detalle invalida.
- `500 Internal Server Error`: error inesperado al persistir o consultar datos.

Ejemplo de conflicto de detalle:

```json
{
  "message": "Proforma detail does not belong to proforma"
}
```

## Ruta Unica De Edicion
`PUT /proformas/:id` es la unica ruta para modificar datos o detalle de una
proforma. Las operaciones `PATCH /proformas/:id/pay` y
`PATCH /proformas/:id/cancel` permanecen porque cambian el estado del
documento, no editan su contenido.
