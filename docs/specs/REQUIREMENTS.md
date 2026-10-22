# Requisitos del sistema

## Contexto

ABC Software Corp comercializa abarrotes y mercadería al por mayor y menor. El sistema legado registra fichas sencillas de almacén (`fecha`, `ingreso`, `salida`, `producto`, `nro_documento`). La solución debe formalizar la trazabilidad, emitir CPE en UBL y operar con SUNAT directamente o mediante OSE. El enunciado fuente está en `docs/compliance/reference/enunciado.md`.

## Requisitos funcionales

| ID | Requisito | Fuente / observación |
|---|---|---|
| RF-01 | Registrar clientes, productos, unidades y parámetros de catálogo con auditoría de cambios. | Diseño propuesto; campos finales por validar. |
| RF-02 | Registrar entradas y salidas de inventario y reconstruir existencias mediante movimientos trazables. | Enunciado; política de stock negativo POR VERIFICAR. |
| RF-03 | Emitir facturas y boletas con identificador fiscal, líneas, importes y referencia de cliente. | RS 097-2012/SUNAT y perfil vigente POR VERIFICAR. |
| RF-04 | Emitir notas de crédito/débito vinculadas al comprobante afectado y con motivo permitido. | RS 097-2012/SUNAT y catálogos de motivos POR VERIFICAR. |
| RF-05 | Generar XML UBL 2.1 y validar contra XSD antes de firmar o transmitir. | XSD UBL local; perfil SUNAT adicional POR VERIFICAR. |
| RF-06 | Firmar documentos y proteger la custodia de claves/certificados. | Ley 27269 y guía técnica aplicable POR VERIFICAR. |
| RF-07 | Enviar CPE, bajas o resúmenes al canal habilitado y consultar ticket/CDR/estado. | SUNAT/OSE y modalidad elegida POR VERIFICAR. |
| RF-08 | Validar formato/checksum de RUC y separar dicha comprobación de la consulta registral. | D. Leg. 943 y RS 210-2004/SUNAT; procedimiento POR VERIFICAR. |
| RF-09 | Importar datos legados conservando el valor original, procedencia, errores y conciliación. | Enunciado; formato/volumen de origen POR VERIFICAR. |
| RF-10 | Producir reportes financieros reproducibles con criterios de inclusión y estados de CPE visibles. | RS 234-2006/SUNAT como ámbito de análisis; libros/registros aplicables POR VERIFICAR. |

## Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Distribución objetivo nativa para Windows mediante MSI y Android mediante APK; firma, actualización y canales de distribución POR VERIFICAR. |
| RNF-02 | Mantener separación MVC de entrada y dominio/aplicación desacoplados para facilitar evolución hexagonal/Clean. |
| RNF-03 | Preservar trazabilidad e inmutabilidad de XML firmado, envío, respuesta y auditoría de usuario. |
| RNF-04 | Usar decimal exacto para importes y registrar la regla de redondeo aplicada. Precisión tributaria POR VERIFICAR. |
| RNF-05 | No registrar secretos, claves privadas ni contraseñas en repositorio, documentación o logs. |
| RNF-06 | Soportar reintentos idempotentes y estado desconocido ante timeout de servicios externos. |

## Restricciones

- La fase de diseño no autoriza lógica productiva hasta aprobar las especificaciones.
- RS 097-2012/SUNAT, RS 234-2006/SUNAT, catálogos y cálculos se contrastan contra fuentes oficiales vigentes.
- Catálogo 07 es el mapeo conocido para afectación IGV y Catálogo 08 para sistema ISC; versión y mapeo SUNAT definitivo POR VERIFICAR.
- El esqueleto en `src/` no prueba que el sistema compile ni que exista un stack decidido formalmente.