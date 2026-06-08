# Estructura de Respuesta: Proforma

## Objetivo
Documentar el objeto JSON de respuesta del módulo de proformas para que frontend/PDF usen una estructura estable.

La respuesta real viene envuelta en un objeto raíz llamado `proforma`.

## Estructura General

```json
{
  "proforma": {
    "prfmaid": "uuid",
    "prfmaidentificador": "FE01-002-001-34",
    "prfmaestado": "emitida",
    "prfmafchregistro": "2026-05-25T04:35:56.719Z",
    "prfmafchactualizacion": "2026-05-25T04:35:56.719Z",
    "emisor": { ... },
    "receptor": {
      "cliente": { ... }
    },
    "detalle": [ ... ],
    "metodoPago": { ... },
    "total": { ... },
    "documento": {
      "docnombre": "FE01-002-001-34_2026-05-25.pdf",
      "docurl": "http://localhost:3000/uploads/proformas/1709639106001/FE01-002-001-34_2026-05-25.pdf"
    }
  }
}
```

## Sección: `proforma`
Datos principales del documento proforma.

- `prfmaid` (`string`): ID único de la proforma.
- `prfmaidentificador` (`string`): identificador legible/secuencial de la proforma.
- `prfmaestado` (`emitida | pagada | anulada`): estado actual.
- `prfmafchregistro` (`Date`): fecha de emisión.
- `prfmafchactualizacion` (`Date`): fecha de última actualización.

## Sección: `proforma.emisor`
Información de quien emite la proforma.

### `proforma.emisor.empresa`
- `emid` (`string`): empresa emisora.
- `emruc` (`string | null`): RUC de la empresa.
- `emcodigo` (`string | null`): código de empresa.
- `emcorreo` (`string | null`): correo de empresa.
- `emlogo` (`string | null`): URL pública del logo (parseada con `toPublicImageUrl`).
- `emrznsocial` (`string | null`): razón social.

### `proforma.emisor.sucursal`
- `suid` (`string`): sucursal emisora.
- `suidentificador` (`string | null`): identificador de sucursal.
- `sunombre` (`string | null`): nombre de sucursal.

### `proforma.emisor.caja`
- `cjid` (`string`): caja/punto de emisión.
- `cjidentificador` (`string | null`): identificador de caja.

### `proforma.emisor.usuario`
- `usid` (`string`): usuario emisor.
- `usnombre` (`string | null`): nombre del usuario.
- `usrol` (`string | null`): rol del usuario.

## Sección: `proforma.receptor`
Información del cliente receptor de la proforma.

### `proforma.receptor.cliente`
- `clnteid` (`string`): ID del cliente.
- `clnteidentificacion` (`string | null`): número de identificación.
- `clntenombre` (`string | null`): nombre o razón social del cliente.
- `clntecorreo` (`string | null`): correo del cliente.
- `clntedireccion` (`string | null`): dirección del cliente.
- `clntetelefono` (`string | null`): teléfono del cliente.

## Sección: `proforma.metodoPago`
Método de pago asociado a la proforma.

### `proforma.metodoPago`
- `mpid` (`string`): ID del método de pago.
- `mpnombre` (`string | null`): nombre del método de pago.

## Sección: `proforma.total`
Resumen monetario.

- `prfmasubtotal` (`number`): suma de líneas detalle antes de descuento.
- `prfmadescuento` (`number`): descuento aplicado.
- `prfmatotal` (`number`): total final.

## Sección: `proforma.detalle`
Arreglo de ítems de la proforma.

Cada elemento contiene:

- `dprfmaid` (`string`): ID del detalle.
- `dprfmatipoitem` (`inventariable | manual`): tipo del item.
- `producto` (`object`): siempre usa este nombre, tanto para items manuales como inventariables.
- `producto.dprfmacodigo` (`string | null`)
- `producto.dprfmadescripcion` (`string | null`)
- `producto.dprfmacantidad` (`number`): cantidad.
- `producto.dprfmapreciounitario` (`number`): precio unitario.
- `producto.dprfmapreciototal` (`number`): total de línea.

## Sección: `proforma.documento`
Referencia al PDF asociado.

- `docnombre` (`string | null`): nombre del archivo PDF.
- `docurl` (`string | null`): URL pública del PDF.

## Nota para listado (`GET /proformas`)
En el listado paginado, cada elemento de `items` devuelve la misma estructura completa de `ProformaResponseDto`:

- `items[].proforma.emisor`
- `items[].proforma.receptor`
- `items[].proforma.metodoPago`
- `items[].proforma.detalle`
- `items[].proforma.total`
- `items[].proforma.documento`
