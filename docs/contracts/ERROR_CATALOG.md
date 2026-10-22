# Catálogo de errores de aplicación

Los códigos de esta tabla son identificadores internos propuestos; no son códigos CDR ni códigos oficiales SUNAT/OSE.

| HTTP | Código | Significado | Reintento |
|---:|---|---|---|
| 400 | `REQUEST_INVALID` | JSON/formato de solicitud inválido. | No, corregir solicitud. |
| 401 | `AUTHENTICATION_FAILED` | Token ausente, inválido o vencido. | Tras autenticación válida. |
| 403 | `PERMISSION_DENIED` | Usuario sin permiso para la operación. | No sin autorización. |
| 404 | `RESOURCE_NOT_FOUND` | CPE, cliente, producto o envío inexistente. | No, verificar identificador. |
| 409 | `STATE_CONFLICT` | Correlativo, idempotencia o estado incompatible. | Consultar estado; no reenviar a ciegas. |
| 422 | `BUSINESS_VALIDATION_FAILED` | Regla de dominio/perfil incumplida. | No, corregir datos. |
| 422 | `RUC_CHECK_FAILED` | Formato/checksum RUC inválido. | No, revisar número. |
| 422 | `XML_SCHEMA_INVALID` | Documento no pasa XSD o perfil. | No, corregir XML/datos. |
| 502 | `EXTERNAL_RESULT_UNKNOWN` | Timeout o respuesta externa no correlacionada. | Consultar antes de reintentar. |
| 502 | `EXTERNAL_SERVICE_REJECTED` | SUNAT/OSE informa rechazo; conservar código externo aparte. | Según diagnóstico oficial. |
| 503 | `EXTERNAL_SERVICE_UNAVAILABLE` | Canal/consulta no disponible. | Con backoff y clave idempotente. |
| 413 | `IMPORT_TOO_LARGE` | Lote excede límite de carga configurado. | Dividir lote. |

La respuesta API usa `api-error.schema.json` e incluye `correlationId`. La descripción externa se conserva separada del código interno.