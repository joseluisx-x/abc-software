# Reglas de negocio

Las reglas siguientes son contratos de diseño. Ninguna fórmula marcada POR VERIFICAR debe implementarse como regla fiscal sin cotejo con la norma vigente y la guía técnica SUNAT.

| ID | Regla | Fuente / estado |
|---|---|---|
| BR-01 | Todo documento fiscal conserva emisor, tipo, serie, correlativo, fecha, XML firmado y cada respuesta recibida como evidencia enlazada. | RS 097-2012/SUNAT y perfil vigente POR VERIFICAR. |
| BR-02 | Un correlativo se reserva de forma atómica por emisor/serie y no se reutiliza tras una emisión confirmada. | RS 097-2012/SUNAT; reglas exactas POR VERIFICAR. |
| BR-03 | Tipo de CPE, serie, correlativo, RUC emisor y nombre de archivo/paquete deben ser coherentes. | Anexos/nomenclatura SUNAT POR VERIFICAR. |
| BR-04 | La afectación IGV se identifica con el Catálogo 07; el sistema ISC con Catálogo 08. | Catálogos SUNAT vigentes; códigos y versiones POR VERIFICAR. |
| BR-05 | IGV se calcula solo según categoría/base legal aplicable. La tasa general indicada como referencia es 18%; tasa efectiva y excepciones vigentes POR VERIFICAR. | D.S. 055-99-EF art. 17; Ley 29666; texto vigente POR VERIFICAR. |
| BR-06 | ISC depende del producto, sistema declarado y parámetros vigentes; no existe una fórmula universal para todo producto. | D.S. 055-99-EF arts. 50–67 y Apéndices III/IV, modificatorias; POR VERIFICAR. |
| BR-07 | ICBPER depende de bolsas gravadas, unidades y tarifa vigente en fecha de operación; su XML/base/totales requieren perfil oficial. | Ley 30884 art. 12 y modificatorias; POR VERIFICAR. |
| BR-08 | Importes se calculan con decimal exacto, preservan precisión intermedia y documentan redondeo. Escala/etapa/modo legal POR VERIFICAR. | Guía de emisión SUNAT vigente POR VERIFICAR. |
| BR-09 | Checksum de RUC no demuestra existencia, estado registral ni autorización; registrar fuente y fecha de cualquier consulta externa. | D. Leg. 943, RS 210-2004/SUNAT y servicio oficial POR VERIFICAR. |
| BR-10 | Ante timeout de envío, marcar resultado desconocido y consultar antes de retransmitir; nunca asumir aceptación. | Diseño de resiliencia; contrato SUNAT/OSE POR VERIFICAR. |
| BR-11 | Los datos legados ambiguos se conservan en staging/cuarentena; nunca se inventan identificadores fiscales para completar numeraciones informales. | Requisito de trazabilidad del enunciado. |
| BR-12 | El saldo de inventario deriva del historial de movimientos; correcciones son movimientos auditados, no edición destructiva del saldo. | Diseño propuesto; reglas comerciales POR VERIFICAR. |

## Cálculo

Para línea $i$, el valor neto candidato es $b_i = q_i p_i - d_i + c_i$. El cálculo de bases, impuestos por categoría, totales, ISC/ICBPER y redondeos se describe con parámetros y reservas en `docs/domain/DOMAIN_MODEL.md`; validar fórmulas por operación contra la ley y el perfil UBL/SUNAT antes de producción.