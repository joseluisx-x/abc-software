# Idempotencia y reintentos

## Contrato propuesto

- Los comandos que crean comprobantes, envíos, resúmenes o lotes requieren `Idempotency-Key` aleatoria y estable para la misma intención.
- La clave se vincula al principal autenticado, operación y hash canónico del payload; no almacenar secretos en la clave.
- Misma clave + mismo payload: devolver el resultado original con el mismo ID lógico.
- Misma clave + payload distinto: conflicto `STATE_CONFLICT`.
- La reserva de correlativo, creación del comprobante y registro de una solicitud de envío deben formar una transacción local consistente.
- Antes de repetir una transmisión tras timeout, consultar el envío existente por correlación/ticket. Estado no concluyente permanece `UNKNOWN`; no reportar aceptación.
- Retención/TTL de claves, unicidad global, consistencia multi-dispositivo y política de reconciliación **POR VERIFICAR** según canal y persistencia elegidos.

## Estados de envío sugeridos

`CREATED -> SUBMITTED -> PENDING | ACCEPTED | OBSERVED | REJECTED | UNKNOWN`.

La transición de `UNKNOWN` requiere consulta/cotejo; no se debe cambiar directamente a `ACCEPTED` sin respuesta verificable.