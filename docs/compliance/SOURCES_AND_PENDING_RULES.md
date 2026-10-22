# Hito 7: Auditoría de consistencia y cierre de fase

> **Estado:** auditoría documental de diseño. Cierre técnico condicionado al cotejo de fuentes oficiales y a la resolución de los puntos **POR VERIFICAR** listados abajo.

## 1. Alcance revisado

Se cruzan los artefactos definidos por el plan: análisis UBL, casos de uso, modelo de dominio, JSON Schemas, arquitectura, OpenAPI, matriz normativa y esta revisión. El enunciado fuente está preservado en `docs/compliance/reference/enunciado.md`.

| Artefacto | Control realizado |
|---|---|
| `UBL_VALIDATION_STRATEGY.md` | Cardinalidades raíz contrastadas con `UBL-Invoice-2.1.xsd`; perfil SUNAT separado del XSD. |
| `USE_CASES.md` | IDs CU-01 a CU-09 vinculados a actores, flujos, errores y MoSCoW. |
| `DOMAIN_MODEL.md` | Agregados, fórmulas parametrizadas, saneamiento legado y schemas documentados. |
| `contracts/SCHEMAS/*.json` | Schemas Draft 2020-12; comando de entrada separado del recurso calculado. Referencias por verificar con resolver. |
| `ARCHITECTURE.md` | MVC como hipótesis; target hexagonal y ADR sin afirmar implementación existente. |
| `api/openapi.yaml` | OpenAPI 3.1 para auth, clientes/RUC, productos, inventario, CPE/envíos, resúmenes y carga de legado. |
| `SUNAT_UBL_TRACEABILITY.md` | Requisitos, XPath, DTO, dominio, regla e ID de caso relacionados. |

## 2. Consistencia cruzada

| Eje | Resultado | Acción/observación |
|---|---|---|
| ID de casos de uso | Consistente: CU-01…CU-09 en casos, diagrama y matriz. | Mantener IDs al evolucionar API. |
| Campos API ↔ schemas | Los endpoints referencian `cliente`, `producto`, `comprobante-command`, `comprobante`, `movimiento-inventario`, `resumen-diario` y `api-error`; los nombres existen en `/schemas/`. | Revalidar `$ref` externos con validadores OpenAPI/JSON Schema en CI cuando se incorpore tooling. |
| Entrada y salida de comprobante | Comando no contiene estado/totales; recurso sí los contiene. | Correcto para impedir que el cliente proponga totales calculados; no es validación de autorización. |
| Líneas de comprobante de salida | Recurso usa `invoice-line.schema.json`, separado de la línea de entrada, y expone importe/desglose calculado. | Alineado estructuralmente; el mapeo fiscal de tributos continúa POR VERIFICAR. |
| Diagrama de dominio ↔ DTO | `Comprobante` usa `serie`, `correlativo`, `fechaEmision`, `estado` e `importePagar`, alineados con el recurso/schema. | Alineación nominal realizada; tipos de dominio siguen siendo conceptuales. |
| Taxonomía Cat. 07/08 | La afectación IGV está asociada a Cat. 07 y el sistema ISC a Cat. 08; el mapeo SUNAT del sistema ISC a XPath no se inventa. | Confirmar catálogos/perfil técnico oficial. |
| Totales y tributos | Fórmulas describen parámetros y dependencias, no dan algoritmos normativos cerrados ni crean códigos. | Cotejar cada variante y límites de redondeo antes de aceptación. |
| Firma y extensiones | Se distingue `cac:Signature` de `ds:Signature`; XPath se declara perfil-dependiente. | Confirmar ubicación, algoritmos, certificado y extensiones de acuerdo con guía vigente. |
| Estados síncronos/asíncronos | API diferencia resultado inmediato y ticket/consulta; timeout puede ser UNKNOWN. | Definir mapeo de todos los códigos CDR/OSE y política de reintento/idempotencia. |
| RS 234-2006/SUNAT | Se trata como referencia de libros/registros, no como regla automática para XML de emisión. | Precisar libros/formatos y obligación del contribuyente con Contabilidad. |
| Legado | Campos ambiguos pasan a staging/cuarentena y mantienen dato bruto. | Obtener muestra, diccionario, unidad y definición de `ingreso`/`salida`/`nro_documento`. |
| ADR | .NET MAUI es preferencia condicionada; Flutter/KMP siguen evaluables. | Prototipo obligatorio de firma, SOAP, MSI firmado y APK firmado antes de decisión final. |

## 3. Calidad y límites de validación

- Diagnósticos del editor: sin errores en los Markdown y JSON creados durante esta fase; el YAML OpenAPI tampoco reportó diagnósticos en el editor.
- Esta comprobación no equivale a validar todas las referencias remotas/relativas, ejemplos contra schemas, compatibilidad de OpenAPI con un validador independiente, XSD de un XML completo ni aceptación SUNAT/OSE.
- No se generó XML, código de negocio ni pruebas, de acuerdo con la regla de Fase 1.
- No hay entorno real de SUNAT/OSE, certificado de pruebas ni conjunto de CDR disponible.

## 4. Elementos POR VERIFICAR para mesa técnica

1. Texto vigente de RS 097-2012/SUNAT, sus anexos y modificatorias: campos obligatorios por Factura/Boleta/NC/ND, firma, nomenclatura, envío, plazos, baja y resumen.
2. Texto y alcance de RS 234-2006/SUNAT y modificatorias: libros/registros que aplican a ABC Software Corp, datos, formatos, períodos y relación con inventario/reportes.
3. Publicación vigente de Catálogos 01, 02, 05, 06, 07 y 08; códigos, atributos `listID`/`schemeID`, cardinalidad y compatibilidad.
4. Ubicación XPath peruana del sistema ISC, ICBPER y demás metadatos locales; las estructuras no quedan establecidas por el `Invoice` XSD genérico.
5. Tasa general IGV efectiva a fecha de cada emisión, regímenes especiales, base gravable por caso, reglas ISC por SKU y tarifa/supuestos vigentes ICBPER.
6. Rounding: precisión intermedia, escala serializada, empate, distribución de descuentos y tolerancia admitida por SUNAT/OSE.
7. Algoritmo oficial de checksum RUC, consulta registral permitida, estados admitidos y requisitos de datos del adquirente por comprobante.
8. Perfil XMLDSig/XAdES admitido: transformaciones, algoritmos, certificado, cadena, revocación, ubicación de firma y tratamiento de clave.
9. Servicio/canal habilitado: SUNAT directo u OSE, sincronía, contratos vigentes, códigos, ticket, CDR, límites, retry y estrategia de contingencia.
10. Requisitos de MSI (firma, actualización, arquitectura, elevación/per-user) y APK (firma, distribución/actualización, versiones Android) y stack multiplataforma final.
11. Formato real del legado, semántica de `ingreso`/`salida`, moneda/unidades, documentos fuente y método de conciliación.

## 5. Dictamen

**Cierre Spec-First: condicionado.** Los artefactos cubren el alcance documental solicitado y conservan incertidumbres explícitas; no deben tomarse como especificación legal certificada ni como contrato listo para producción hasta resolver los puntos anteriores y correr validación formal de OpenAPI/JSON Schema con ejemplos. Próxima puerta recomendada: revisión del usuario/Contabilidad sobre supuestos de negocio y validación normativa con fuentes SUNAT oficiales.