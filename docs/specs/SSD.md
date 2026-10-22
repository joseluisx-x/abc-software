# Hito 0: Plan de trabajo inicial

> **Proyecto:** ABC Software Corp — facturación electrónica para comercialización mayorista y minorista de abarrotes en Perú  
> **Fase:** 1 — diseño Spec-First  
> **Fecha:** 2026-10-22  
> **Estado:** Hitos 0–7 elaborados por autorización expresa del usuario para generar los artefactos restantes; revisión normativa pendiente

## 1. Estructura detectada

| Ruta | Estado observado | Uso acordado |
|---|---|---|
| `/docs/compliance/reference/` | Contiene `enunciado.md` y XSD UBL 2.1 en `ubl/common/` y `ubl/maindoc/`. | Fuentes de entrada preservadas durante la reorganización. |
| `/docs/` | Contiene especificaciones, dominio, contratos, arquitectura, API, compliance y migración. | Documentación de diseño y contratos. |
| `/src/backend/` | Esqueleto .NET con capas Api, Application, Domain, Infrastructure y `AbcSoftware.sln`. | Marcadores únicamente; aún no es una solución productiva compilable. |
| `/src/clients/mobile_desktop/` | Esqueleto Flutter con `lib/`, `test/` y `pubspec.yaml`. | Marcadores únicamente; aún no es una aplicación ejecutable. |
| `/tests/` | Carpetas de pruebas unitarias, integración, contratos y aceptación. | Reservadas para la fase de implementación. |

El enunciado local confirma el giro de comercialización de abarrotes, la necesidad de migrar los registros heredados, emitir comprobantes electrónicos UBL, integrarse con SUNAT/OSE y distribuir en Windows (MSI) y Android (APK). Los XSD locales servirán como referencia estructural UBL; no sustituyen las reglas de negocio ni las disposiciones SUNAT. Los detalles no establecidos —como la modalidad de envío efectiva y los datos disponibles para migración— quedan **POR VERIFICAR** antes de convertirse en decisiones de diseño.

## 2. Alcance preliminar

Los artefactos Spec-First se organizaron bajo `/docs/`. El esqueleto físico de `/src/` y `/tests/` no implica que exista lógica de negocio implementada ni cambia la condición de validar los contratos antes de producción.

Se organizará el diseño alrededor de comprobantes electrónicos, reglas tributarias, inventario heredado, interoperabilidad SUNAT/OSE y las plataformas de entrega que el usuario confirme. La aplicabilidad concreta de cada obligación, los plazos, códigos y validaciones se contrastarán con fuentes oficiales antes de declararse como regla. Todo dato incierto permanecerá identificado como **POR VERIFICAR**.

## 3. Marco normativo y catálogos a estructurar

| Referencia | Tratamiento previsto | Fuente / estado |
|---|---|---|
| RS 097-2012/SUNAT | Analizar el marco del SEE y los requisitos aplicables a emisión, comprobantes y documentación electrónica dentro del alcance aprobado. | Resolución y anexos oficiales; verificar texto vigente, modificaciones y aplicabilidad antes de fijar reglas. |
| RS 234-2006/SUNAT | Evaluar su relación con libros y registros vinculados a asuntos tributarios y con los requerimientos de reportabilidad del sistema. No asumir que define por sí sola reglas de emisión de CPE. | Resolución y modificatorias oficiales; alcance concreto **POR VERIFICAR**. |
| Catálogo 01 | Tipos de comprobante/documento requeridos por los flujos acordados. | Catálogo oficial SUNAT vigente **POR VERIFICAR**. |
| Catálogo 02 | Códigos de moneda. | Catálogo oficial SUNAT vigente **POR VERIFICAR**. |
| Catálogo 05 | Códigos de tributos. | Catálogo oficial SUNAT vigente **POR VERIFICAR**. |
| Catálogo 06 | Tipos de documento de identidad. | Catálogo oficial SUNAT vigente **POR VERIFICAR**. |
| Catálogo 07 | Afectación al IGV. | Catálogo oficial SUNAT vigente **POR VERIFICAR**. |
| Catálogo 08 | Sistema de cálculo del ISC. | Catálogo oficial SUNAT vigente **POR VERIFICAR**. |

**Discrepancia que se conservará como control de diseño:** el requerimiento describe el Catálogo 08 como afectación tributaria. En la clasificación SUNAT conocida, el Catálogo 07 corresponde a tipo de afectación del IGV y el Catálogo 08 al sistema de cálculo del ISC. Para evitar propagar un mapeo incorrecto, el diseño tratará la afectación IGV bajo el 07 y el sistema ISC bajo el 08, sujeto a cotejo con la publicación oficial vigente antes de cerrar los artefactos posteriores. **Fuente de contraste:** catálogos SUNAT asociados al SEE y documentación/anexos de la RS 097-2012/SUNAT; versión exacta vigente **POR VERIFICAR**.

## 4. Cronograma de diseño y puertas de aprobación

El flujo previsto es secuencial y normalmente cada hito queda detenido hasta recibir **“Aprobado hito X”**. La instrucción posterior del usuario **“genera los demás archivos”** autorizó continuar en lote desde el Hito 1 hasta el Hito 7. Esa autorización no sustituye la revisión de fuentes oficiales ni convierte los pendientes **POR VERIFICAR** en reglas confirmadas.

| Orden | Hito y artefacto | Trabajo previsto | Puerta |
|---:|---|---|---|
| 0 | `docs/specs/SSD.md` | Alcance preliminar, fuentes, catálogos y secuencia. | Elaborado. |
| 1 | `docs/compliance/UBL_VALIDATION_STRATEGY.md` | XSD Invoice, cardinalidades, perfil, firma y reglas externas. | Elaborado; perfil legal por verificar. |
| 2 | `docs/specs/USE_CASES.md` | Actores, flujos, errores, condiciones y MoSCoW. | Elaborado. |
| 3 | `docs/domain/DOMAIN_MODEL.md` y `docs/contracts/SCHEMAS/` | Modelo, reglas parametrizadas, migración y DTOs. | Elaborado; fuentes tributarias pendientes. |
| 4 | `docs/architecture/ARCHITECTURE.md` | MVC, Hexagonal/Clean, contextos y ADR de stack. | Elaborado; stack requiere validación. |
| 5 | `docs/api/openapi.yaml` | Contrato OpenAPI, endpoints y referencias a schemas. | Elaborado; resolver refs tras reubicación. |
| 6 | `docs/compliance/SUNAT_UBL_TRACEABILITY.md` | Relación norma, XPath, DTO, dominio, regla y caso de uso. | Elaborado; elementos **POR VERIFICAR** pendientes. |
| 7 | `docs/compliance/SOURCES_AND_PENDING_RULES.md` | Auditoría documental y pendientes normativos/técnicos. | Elaborado; no certifica cumplimiento. |

## 5. Criterios de trabajo

1. Toda afirmación legal o de cálculo tributario incluirá la referencia normativa concreta disponible; si no puede confirmarse, se marcará **POR VERIFICAR**.
2. Los XSD UBL locales sustentan estructura y tipos XML, pero no se presentarán como prueba suficiente de cumplimiento SUNAT.
3. Se respetarán los nombres y límites de alcance definidos para cada hito. No se iniciará un artefacto posterior sin aprobación del anterior.
4. El idioma de los artefactos será español; los identificadores técnicos conservarán la nomenclatura estándar.
5. Los archivos base en `/src/` son marcadores de estructura; no se consideran código productivo ni implementación de lógica de negocio.

## 6. Decisiones pendientes para hitos posteriores

- Formato, volumen y calidad de los datos legados y reglas de operación aún no definidas por el enunciado.
- Modalidad de envío: SUNAT directo, OSE o ambos.
- Plataformas objetivo y restricciones de despliegue; MSI y APK constan en la solicitud, pero sus requisitos operativos se detallarán en el Hito 4.
- Formato, volumen y calidad de los datos legados que se migrarán.
- Interpretación vigente y aplicabilidad de RS 097-2012/SUNAT y RS 234-2006/SUNAT al alcance final.
- Versiones oficiales vigentes de los catálogos y cualquier código cuyo mapeo no quede confirmado.

## 7. Estado

**HITOS 0–7 ELABORADOS — PENDIENTES DE REVISIÓN DEL USUARIO Y COTEJO NORMATIVO OFICIAL.** La elaboración en lote se realizó por instrucción expresa del usuario. El cierre no certifica cumplimiento SUNAT ni reemplaza la verificación de los elementos marcados **POR VERIFICAR**.