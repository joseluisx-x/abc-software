# Catálogo de DTOs

Los contratos JSON Schema se encuentran en `docs/contracts/SCHEMAS/` (Draft 2020-12). Los identificadores de catálogo se transportan como cadenas para conservar ceros iniciales.

| DTO/schema | Uso | Campos principales |
|---|---|---|
| `customer.schema.json` | Alta/consulta de cliente y adquirente en CPE. | tipoDocumento, numeroDocumento, nombreLegal, direccion, email. |
| `product.schema.json` | Catálogo de productos y parámetros tributarios declarativos. | sku, descripcion, unidadMedida, precioUnitario, codigoAfectacionIGV, codigoSistemaISC. |
| `sale.schema.json` | Comando para solicitar emisión; no admite estado ni totales autoritativos. | tipoCpe, serie solicitada, fecha, moneda, cliente, lineas, referencia. |
| `invoice.schema.json` | Vista de comprobante persistido. | id, emisorRuc, tipo/serie/correlativo, estado, cliente, lineas calculadas, totales. |
| `invoice-line.schema.json` | Línea calculada de respuesta. | numeroLinea, cantidad, valorVenta, codigoAfectacionIGV, impuestos. |
| `inventory-movement.schema.json` | Movimiento interno de stock. | productoId, direccion, cantidad, unidadMedida, ocurridoEn, origen. |
| `daily-summary.schema.json` | Resumen diario y estado de procesamiento. | fechaResumen, estado, ticket, comprobantes. |
| `api-error.schema.json` | Sobre uniforme de error API. | code, message, correlationId, details, retryable. |

El esquema de respuesta no sustituye XML UBL. Los campos legales exactos, códigos de catálogos y tratamiento de notas están **POR VERIFICAR**.