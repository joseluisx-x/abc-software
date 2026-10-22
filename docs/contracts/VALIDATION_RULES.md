# Reglas de validación de contratos

| Regla | Capa | Comportamiento |
|---|---|---|
| JSON Schema 2020-12 | API | Validar tipos, requeridos, formatos, enums estructurales y `additionalProperties` antes de entrar a aplicación. |
| IDs de catálogo | Dominio/perfil SUNAT | Validar contra catálogo versionado vigente; no mantener enums normativos congelados sin fecha/fuente. |
| RUC | Value object + adaptador externo | Validar formato/checksum local; consulta de estado es separada, fechada y puede ser indisponible. |
| Serie/correlativo | Dominio | Compatibilidad tipo/serie y unicidad transaccional; formato legal exacto **POR VERIFICAR**. |
| Cantidad/precio | Dominio | Cantidad positiva y unidad coherente; importes calculados por servidor con decimal exacto. |
| Afectación/tributos | Dominio tributario | Cat. 07 para afectación IGV, Cat. 08 para sistema ISC (mapeo vigente **POR VERIFICAR**); validar compatibilidad por línea. |
| Totales | Dominio | Reconciliar líneas, bases, tributos, descuentos/cargos, anticipos y pagar según perfil oficial. |
| XML | Adaptador UBL | Validar XSD local y reglas de perfil antes de firma; XSD no sustituye validaciones semánticas. |
| Firma | Adaptador de seguridad | Verificar digest, referencia, certificado y política; algoritmos/perfil SUNAT **POR VERIFICAR**. |
| Idempotencia | API/aplicación | Misma clave y mismo payload devuelve el efecto previamente registrado; misma clave con payload distinto produce conflicto. |
| Migración | Aplicación | Preservar bruto y procedencia; datos ambiguos pasan a cuarentena y requieren resolución explícita. |

No se deben convertir las respuestas de SUNAT/OSE a “aceptado” sin comprobar origen, integridad y correlación del CDR.