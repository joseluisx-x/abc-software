# Especificación de API

La definición normativa del contrato HTTP es `docs/api/openapi.yaml` (OpenAPI 3.1.0). Este resumen no sustituye el YAML.

## Convenciones

- Prefijo lógico `/api/v1`; URL de despliegue pendiente.
- Autenticación Bearer; token, expiración y autorización requieren decisión de seguridad.
- Errores con `api-error.schema.json`, `correlationId` y códigos internos definidos en `docs/contracts/ERROR_CATALOG.md`.
- Importes son `number` en JSON por limitación del formato; la aplicación debe parsearlos a decimal exacto y fijar escala antes de serializar.
- Los documentos de emisión reciben comando, no estado ni totales calculados por el cliente.

## Integración SUNAT/OSE

El API público separa creación de CPE, envío y consulta de estado. El puerto de salida interno acepta canal (`SUNAT_DIRECT`/`OSE`), idempotencia y el documento empaquetado/firmado. En modo síncrono devuelve estado final o desconocido; en asíncrono retorna aceptación técnica y ticket/ID para consulta posterior. Contrato real, nombres de operaciones, códigos, paquetes, límites, autenticación y plazo de consulta **POR VERIFICAR** contra el canal oficial seleccionado.

## Pendientes de contrato

- Modelar endpoints para notas, baja y carga de resumen con el tipo de documento/servicio oficial correcto; el esquema UBL Invoice no representa NC/ND/RA/RC.
- Añadir CDR como artefacto descargable, autorización de acceso, retención y tamaño máximo.
- Acordar versionado, paginación, formato de fecha/hora, límites y taxonomía de estado.
- Resolver todos los `$ref` y validar ejemplos con validador independiente antes de congelar el contrato.