# POST /proformas

## Descripción
Registra una nueva proforma en estado `emitida` para la empresa del usuario autenticado.

Responsabilidades del endpoint:
- Validar permisos de empresa y usuario.
- Validar consistencia de subtotales/totales enviados por frontend.
- Generar identificador secuencial (`prfmaidentificador`).
- Guardar cabecera y detalle de la proforma.
- Devolver el documento completo para consumo de frontend.

## Ruta
`POST /proformas`

## Autenticación
Requiere JWT:

`Authorization: Bearer <token>`

## Content-Type
`application/json`

## Body

### Cabecera
- `prfmasuid` (string, requerido): id de sucursal.
- `prfmacjid` (string, requerido): id de caja.
- `prfmaclnteid` (string, requerido): id de cliente.
- `prfmampid` (string, requerido): id de método de pago.
- `prfmasubtotal` (number, requerido): subtotal calculado por frontend.
- `prfmadescuento` (number, opcional, default `0`): descuento total.
- `prfmatotal` (number, requerido): total final calculado por frontend.
- `dprfmaproductos` (array, requerido): detalle de productos/servicios.

### Item (`dprfmaproductos[]`)
- `dprfmaesinventariable` (boolean, requerido).
- `dprfmacodigo` (string, condicional):
`requerido` cuando `dprfmaesinventariable = true`; `opcional` cuando `false`.
- `dprfmadescripcion` (string, requerido).
- `dprfmacantidad` (number, requerido, > 0).
- `dprfmapreciounitario` (number, requerido, > 0).
- `dprfmapreciototal` (number, requerido, > 0).

## Reglas de consistencia
Se validan antes de guardar:
- `sum(dprfmaproductos[].dprfmapreciototal) == prfmasubtotal`
- `prfmasubtotal - prfmadescuento == prfmatotal`

Se usa tolerancia numérica de `0.0001` para evitar falsos negativos por coma flotante.

## Reglas de negocio
- Empresa del usuario autenticado: debe existir y estar `activa`.
- Usuario autenticado: debe existir, pertenecer a la empresa, estar `activo` y tener rol `jefe` o `empleado`.
- Sucursal: debe existir y estar `activa`.
- Caja: debe existir, estar `activa` y pertenecer a la sucursal enviada.
- Cliente: debe existir y estar `activo`.
- Método de pago: debe existir y estar `activo`.
- Secuencia: debe existir para combinación empresa+sucursal.
- No se permiten códigos duplicados entre items inventariables.
- Todo item con `dprfmaesinventariable = true` debe usar el código de un producto existente y `activo` de la empresa autenticada.

## Ejemplo Request

```json
{
  "prfmasuid": "2079f9a4-2676-4601-9c87-cfb81edb70e4",
  "prfmacjid": "2dbe2030-46d6-4960-bd5e-fafb6719540f",
  "prfmaclnteid": "f969e3c1-cd5f-4286-8b5a-81ae9badfa3e",
  "prfmampid": "12421636-dde1-44f1-b36c-9bd81bee22af",
  "prfmasubtotal": 35,
  "prfmadescuento": 0,
  "prfmatotal": 35,
  "dprfmaproductos": [
    {
      "dprfmaesinventariable": true,
      "dprfmacodigo": "012344678901",
      "dprfmadescripcion": "Brocha 2\"",
      "dprfmacantidad": 1,
      "dprfmapreciounitario": 25,
      "dprfmapreciototal": 25
    },
    {
      "dprfmaesinventariable": false,
      "dprfmadescripcion": "Servicio de instalación y ajuste",
      "dprfmacantidad": 1,
      "dprfmapreciounitario": 10,
      "dprfmapreciototal": 10
    }
  ]
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "proforma": {
    "prfmaid": "ce3bb915-e4e2-4649-8e82-9dff0e5e7946",
    "prfmaidentificador": "FE01-002-001-23",
    "prfmaestado": "emitida",
    "prfmafchregistro": "2026-05-25T02:18:46.630Z",
    "prfmafchactualizacion": "2026-05-25T02:18:46.630Z",
    "emisor": {
      "empresa": {
        "emid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
        "emlogo": "http://localhost:3000/uploads/empresas/logo.png",
        "emrznsocial": "Ferreconst ElectroLuz K y B",
        "emruc": "1709639106001",
        "emcorreo": "electrokyb@gmail.com",
        "emcodigo": "FE01"
      },
      "sucursal": {
        "suid": "2079f9a4-2676-4601-9c87-cfb81edb70e4",
        "sunombre": "Sucursal Centro",
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
        "dprfmaid": "1c2d17f2-7637-4777-a6c3-118b788fe6b3",
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
      "prfmadescuento": 0,
      "prfmatotal": 35
    },
    "documento": {
      "docnombre": "FE01-002-001-23_2026-05-25.pdf",
      "docurl": "http://localhost:3000/uploads/proformas/1709639106001/FE01-002-001-23_2026-05-25.pdf"
    }
  }
}
```

## Errores comunes
- `401 Unauthorized`: token inválido o ausente.
- `409 Conflict`:
`Proforma has duplicated products in items`, `Inventariable product code does not exist`, `Inventariable product is not active`, `Subtotal does not match item totals`, `Total does not match subtotal minus discount`, `Sequence does not exist for this company and branch`, `Proforma total cannot be negative`.
- `500 Internal Server Error` con mensajes de validación/negocio (comportamiento actual del middleware para errores sin `statusCode`):
`Branch id is required`, `Checkout id is required`, `Client id is required`, `Playment method id is required`, `Product code is required for inventariable items`, `dprfmaesinventariable must be a boolean`, `Quantity must be greater than zero`, `Unit price must be greater than zero`, `Item total must be greater than zero`, `Company does not exist`, etc.

## Notas técnicas
- La proforma siempre se crea en estado `emitida`.
- El backend no recalcula `dprfmapreciototal`; valida lo enviado por frontend.
- `prfmasubtotal` se persiste en cabecera (`proforma`).
- La respuesta real incluye `proforma.documento` con la referencia al PDF generado.
- `prfmaidentificador` se arma así:
`{emcodigo}-{suidentificador}-{cjidentificador}-{secuencia}`.
