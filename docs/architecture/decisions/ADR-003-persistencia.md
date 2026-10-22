# ADR-003: Persistencia y retención documental

- **Estado:** Pendiente de decisión.
- **Contexto:** se deben conservar registros legados, CPE, XML firmado, intentos, tickets/CDR y movimientos de inventario; fuente/destino de datos y volumen no están definidos.
- **Decisión provisional:** no seleccionar motor todavía. Requerir transacciones ACID para reservar correlativo y persistir estados, unicidad por emisor/serie, migración auditable, respaldos, cifrado, consultas por fecha/estado y archivado de XML/CDR con hash.
- **Alternativas a evaluar:** SQL relacional embebido o servidor; almacenamiento separado para documentos; esquema híbrido según despliegue y disponibilidad.
- **Criterios:** operación en Windows/Android, multiusuario, trabajo sin conexión si se requiere, recuperación, retención legal, backup/restore y costo operativo.
- **Pendiente:** tecnología, hosting, RPO/RTO, retención y sincronización offline/online **POR VERIFICAR**.