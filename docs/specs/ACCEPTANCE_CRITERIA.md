# Criterios de aceptación

Estos criterios son verificables en la fase de implementación; los criterios fiscales quedan bloqueados hasta resolver sus fuentes **POR VERIFICAR**.

| ID | Criterio |
|---|---|
| AC-01 | Dado un comando válido de emisión, el servidor asigna ID/correlativo y el cliente no puede imponer el estado ni los totales calculados. |
| AC-02 | Una solicitud repetida con la misma clave de idempotencia no crea un segundo comprobante ni un segundo envío lógico. |
| AC-03 | Un XML que no pasa el XSD local o reglas de perfil no se firma ni se envía; se devuelve diagnóstico correlacionable. |
| AC-04 | Un timeout de SUNAT/OSE queda como resultado desconocido y se puede consultar antes de cualquier reintento. |
| AC-05 | Cada CDR/respuesta se conserva íntegro y correlacionado al envío y comprobante; nunca sobrescribe la evidencia anterior. |
| AC-06 | Los movimientos de inventario registran producto, dirección, cantidad, unidad, fecha, actor y referencia de origen. |
| AC-07 | La migración preserva el registro bruto; numeraciones ambiguas quedan en cuarentena y no se transforman en identificadores fiscales. |
| AC-08 | Las validaciones locales de RUC distinguen formato/checksum de consulta oficial y registran indisponibilidad del servicio externo. |
| AC-09 | Los totales reportados son reproducibles a partir de líneas, tributos, notas, moneda y estado CPE. |
| AC-10 | Los artefactos entregables MSI y APK se instalan/actualizan en sus plataformas objetivo y pasan pruebas de firma/distribución aprobadas. Los criterios concretos de SO y firma se definirán al cerrar ADR de stack. |
| AC-11 | Ningún secreto real, certificado privado o credencial SOL aparece en código, Git, logs o ejemplos. |
| AC-12 | El JSON Schema y OpenAPI se validan con herramientas independientes; los `$ref` locales resuelven y ejemplos validan contra contratos. |

**Puerta legal:** AC fiscales se aceptan solo tras aprobación de versiones normativas, catálogos, fórmulas, XPath del perfil, reglas de emisión, canales y códigos CDR por responsables tributario/técnico.