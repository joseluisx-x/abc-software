# Hito 0: Plan de trabajo inicial

> **Proyecto:** ABC Software Corp — facturación electrónica para comercialización mayorista y minorista de abarrotes en Perú  
> **Fase:** 1 — diseño Spec-First  
> **Fecha:** 2026-10-22  
> **Estado:** Hitos 0–7 elaborados por autorización expresa del usuario para generar los artefactos restantes; revisión normativa pendiente

## 1. Estructura detectada

| Ruta | Estado observado | Uso acordado |
|---|---|---|
| `/reference/` | Existe. Incluye `enunciado.md` con el contexto y restricciones del proyecto, además de esquemas UBL 2.1 organizados en `ubl/common/` y `ubl/maindoc/`. | Fuentes de entrada. No modificar. |
| `/specs/` | Existe y contiene este plan. | Única ubicación de artefactos de diseño de esta fase. |
| `/src/` | No aparece en la estructura actual. | Espacio reservado para producción en Fase 2; no crear código en esta fase. |
| `/test/` | No aparece en la estructura actual. | Espacio reservado para pruebas en Fase 2; no crear pruebas en esta fase. |

El enunciado local confirma el giro de comercialización de abarrotes, la necesidad de migrar los registros heredados, emitir comprobantes electrónicos UBL, integrarse con SUNAT/OSE y distribuir en Windows (MSI) y Android (APK). Los XSD locales servirán como referencia estructural UBL; no sustituyen las reglas de negocio ni las disposiciones SUNAT. Los detalles no establecidos —como la modalidad de envío efectiva y los datos disponibles para migración— quedan **POR VERIFICAR** antes de convertirse en decisiones de diseño.

## 2. Alcance preliminar

La fase producirá exclusivamente especificaciones estructuradas en Markdown, OpenAPI/JSON Schema y diagramas Mermaid bajo `/specs/`. No se implementará lógica de negocio ni se escribirán archivos fuera de esa ruta.

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
| 0 | `00-plan.md` | Alcance preliminar, fuentes normativas, catálogos, estructura y secuencia de trabajo. | Esperar aprobación del Hito 0. |
| 1 | `01-analisis-ubl.md` | Analizar `Invoice` y dependencias XSD locales, cardinalidades, uso SUNAT, extensiones y reglas externas al XSD. | Esperar aprobación del Hito 1. |
| 2 | `02-casos-de-uso.md` | Actores, flujos principales/alternativos, condiciones, Mermaid y priorización MoSCoW. | Esperar aprobación del Hito 2. |
| 3 | `03-modelo-dominio.md` y `/schemas/*.json` | Modelo de dominio, reglas de cálculo y RUC con fuentes, mapeo legado y DTOs JSON Schema. | Esperar aprobación del Hito 3. |
| 4 | `04-arquitectura.md` | Evolución MVC a arquitectura hexagonal, contextos delimitados y ADR de plataformas. | Esperar aprobación del Hito 4. |
| 5 | `05-api.yaml` | Contrato OpenAPI 3.1, endpoints, adaptadores SUNAT/OSE, ejemplos, errores y referencias a schemas. | Esperar aprobación del Hito 5. |
| 6 | `06-matriz-trazabilidad.md` | Trazabilidad entre norma, atributo legal, XPath UBL, DTO, dominio, validación y caso de uso. | Esperar aprobación del Hito 6. |
| 7 | `07-revision.md` | Auditoría cruzada de consistencia y consolidación de pendientes **POR VERIFICAR**. | Cierre sujeto a aprobación del Hito 7. |

## 5. Criterios de trabajo

1. Toda afirmación legal o de cálculo tributario incluirá la referencia normativa concreta disponible; si no puede confirmarse, se marcará **POR VERIFICAR**.
2. Los XSD UBL locales sustentan estructura y tipos XML, pero no se presentarán como prueba suficiente de cumplimiento SUNAT.
3. Se respetarán los nombres y límites de alcance definidos para cada hito. No se iniciará un artefacto posterior sin aprobación del anterior.
4. El idioma de los artefactos será español; los identificadores técnicos conservarán la nomenclatura estándar.
5. No se añadirá código de producción ni pruebas durante esta fase. Los esquemas y contratos de datos son especificaciones, no lógica ejecutable de negocio.

## 6. Decisiones pendientes para hitos posteriores

- Texto y requisitos concretos del enunciado: el archivo local está vacío; confirmar si existe una versión que deba incorporarse.
- Modalidad de envío: SUNAT directo, OSE o ambos.
- Plataformas objetivo y restricciones de despliegue; MSI y APK constan en la solicitud, pero sus requisitos operativos se detallarán en el Hito 4.
- Formato, volumen y calidad de los datos legados que se migrarán.
- Interpretación vigente y aplicabilidad de RS 097-2012/SUNAT y RS 234-2006/SUNAT al alcance final.
- Versiones oficiales vigentes de los catálogos y cualquier código cuyo mapeo no quede confirmado.

## 7. Estado

**HITOS 0–7 ELABORADOS — PENDIENTES DE REVISIÓN DEL USUARIO Y COTEJO NORMATIVO OFICIAL.** La elaboración en lote se realizó por instrucción expresa del usuario. El cierre no certifica cumplimiento SUNAT ni reemplaza la verificación de los elementos marcados **POR VERIFICAR**.