# Matriz de endpoints

La lista resume `docs/api/openapi.yaml`; los endpoints de consulta de NC/ND, baja y recuperación del CDR completo quedan por ampliar una vez confirmado el perfil SUNAT/OSE.

| Método | Ruta | Operación | Entrada/salida principal |
|---|---|---|---|
| POST | `/auth/token` | Obtener token | Credenciales / token |
| GET | `/clientes` | Buscar clientes | filtros / página |
| POST | `/clientes` | Crear cliente | `customer.schema.json` |
| POST | `/clientes/validaciones-ruc` | Validar formato y consultar RUC | RUC / resultado diferenciado |
| GET | `/productos` | Consultar catálogo | filtros / productos |
| POST | `/productos` | Crear producto | `product.schema.json` |
| GET | `/inventario/movimientos` | Consultar movimientos | filtros / movimientos |
| POST | `/inventario/movimientos` | Registrar entrada/salida | `inventory-movement.schema.json` |
| POST | `/comprobantes` | Crear comprobante y solicitar emisión | `sale.schema.json` / `invoice.schema.json` |
| GET | `/comprobantes/{comprobanteId}` | Consultar comprobante | UUID / invoice |
| POST | `/comprobantes/{comprobanteId}/envios` | Enviar por adaptador sync/async | canal/modo / resultado o ticket |
| GET | `/envios/{envioId}` | Consultar envío/CDR | UUID / estado correlacionado |
| POST | `/resumenes-diarios` | Crear y enviar resumen | fecha + IDs / ticket/estado |
| GET | `/resumenes-diarios` | Buscar resúmenes | fecha / lista |
| POST | `/migraciones/legado` | Ingesta de lote legado | filas originales / lote de staging |

Canales, requisitos legales y estados son configuración de adaptador y permanecen **POR VERIFICAR**.