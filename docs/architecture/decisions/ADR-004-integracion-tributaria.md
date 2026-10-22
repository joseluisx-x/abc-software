# ADR-004: Integración tributaria SUNAT/OSE

- **Estado:** Pendiente de selección de canal y homologación.
- **Contexto:** el sistema debe generar UBL, firmarlo, transmitirlo y consultar respuesta/CDR.
- **Decisión provisional:** definir un puerto `EnvioCpe` y adaptadores intercambiables para SUNAT directo y OSE; representar envío síncrono y asíncrono, ticket, resultado desconocido e idempotencia. Conservar bytes del XML/ZIP enviado y respuesta recibida con hash.
- **Alternativas:** acoplar lógica al endpoint de SUNAT; integrar con un solo OSE; soportar más de un adaptador tras decidir contractualmente.
- **Consecuencias:** aislamiento de credenciales/protocolos y mejor testabilidad; se requiere mapeo de errores/status por canal.
- **Pendiente:** canal, URL/WSDL/API oficial, auth, métodos, plazos, límites, códigos, CDR y homologación **POR VERIFICAR**.