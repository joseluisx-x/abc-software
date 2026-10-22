# Hito 1: Análisis estructural UBL 2.1

> **Proyecto:** ABC Software Corp — facturación electrónica (Perú)  
> **Estado:** Borrador de diseño; requiere revisión normativa SUNAT  
> **Fuente estructural primaria:** XSD UBL 2.1 locales en `docs/compliance/reference/ubl/`

## 1. Alcance y criterio de lectura

El documento describe el tipo `InvoiceType` declarado en `reference/ubl/maindoc/UBL-Invoice-2.1.xsd` y los tipos importados de los módulos CommonBasicComponents (CBC), CommonAggregateComponents (CAC), CommonExtensionComponents (EXT) y XMLDSig. Separa:

- **Cardinalidad XSD:** cantidad que permite o exige el esquema UBL 2.1.
- **Uso SUNAT:** requisito del perfil de facturación electrónica peruano, que no se deduce del XSD genérico. Donde no se ha cotejado el anexo y sus modificatorias vigentes se indica **POR VERIFICAR**.

El enunciado fuente está preservado en `docs/compliance/reference/enunciado.md`. Los XSD permiten analizar estructura, pero no contienen el perfil de validación SUNAT aplicable a CPE.

## 2. Mapeo jerárquico del elemento raíz

`Invoice` es el elemento global cuyo tipo es `InvoiceType`. Su contenido es una **secuencia ordenada**: un XML puede fallar validación aunque contenga los elementos si los ubica fuera del orden establecido.

```text
Invoice : InvoiceType
└── sequence (en este orden)
    ├── ext:UBLExtensions?                         UBLExtensionsType
    │   └── ext:UBLExtension+                      UBLExtensionType
    │       ├── cbc:ID? / cbc:Name? / metadatos EXT opcionales
    │       └── ext:ExtensionContent               ExtensionContentType
    │           └── un elemento de namespace distinto de EXT
    ├── cbc:UBLVersionID?                          UBLVersionIDType
    ├── cbc:CustomizationID?                       CustomizationIDType
    ├── cbc:ProfileID? / cbc:ProfileExecutionID?
    ├── cbc:ID                                     IDType
    ├── cbc:CopyIndicator? / cbc:UUID?
    ├── cbc:IssueDate                              IssueDateType
    ├── cbc:IssueTime? / cbc:DueDate?
    ├── cbc:InvoiceTypeCode?                       InvoiceTypeCodeType
    ├── cbc:Note* / cbc:TaxPointDate?
    ├── cbc:DocumentCurrencyCode?                  DocumentCurrencyCodeType
    ├── cbc:TaxCurrencyCode? / cbc:PricingCurrencyCode? /
    │   cbc:PaymentCurrencyCode? / cbc:PaymentAlternativeCurrencyCode?
    ├── cbc:AccountingCostCode? / cbc:AccountingCost? /
    │   cbc:LineCountNumeric? / cbc:BuyerReference?
    ├── cac:InvoicePeriod*                         PeriodType
    ├── cac:OrderReference?                        OrderReferenceType
    ├── cac:BillingReference*                      BillingReferenceType
    ├── cac:DespatchDocumentReference* /
    │   cac:ReceiptDocumentReference* /
    │   cac:StatementDocumentReference* /
    │   cac:OriginatorDocumentReference* /
    │   cac:ContractDocumentReference* /
    │   cac:AdditionalDocumentReference* /
    │   cac:ProjectReference*
    ├── cac:Signature*                             SignatureType
    ├── cac:AccountingSupplierParty                SupplierPartyType
    ├── cac:AccountingCustomerParty                CustomerPartyType
    ├── cac:PayeeParty? / cac:BuyerCustomerParty? /
    │   cac:SellerSupplierParty? / cac:TaxRepresentativeParty?
    ├── cac:Delivery* / cac:DeliveryTerms?
    ├── cac:PaymentMeans* / cac:PaymentTerms* / cac:PrepaidPayment*
    ├── cac:AllowanceCharge*
    ├── cac:TaxExchangeRate? / cac:PricingExchangeRate? /
    │   cac:PaymentExchangeRate? / cac:PaymentAlternativeExchangeRate?
    ├── cac:TaxTotal*                              TaxTotalType
    ├── cac:WithholdingTaxTotal*
    ├── cac:LegalMonetaryTotal                     MonetaryTotalType
    └── cac:InvoiceLine+                           InvoiceLineType
        ├── cbc:ID                                 obligatorio en XSD
        ├── cbc:InvoicedQuantity? / cbc:LineExtensionAmount
        ├── referencias, precio y cargos opcionales según sus tipos
        ├── cac:TaxTotal*                          TaxTotalType
        └── cac:Item                               ItemType (1..1)
```

`?` = 0..1; `*` = 0..n; `+` = 1..n. En `InvoiceType`, el XSD exige `cbc:ID`, `cbc:IssueDate`, `cac:AccountingSupplierParty`, `cac:AccountingCustomerParty`, `cac:LegalMonetaryTotal` y una o más `cac:InvoiceLine`. No exige por sí solo los demás elementos raíz.

### Cardinalidades directas de `InvoiceType`

| Cardinalidad XSD | Elementos directos |
|---|---|
| 1..1 | `cbc:ID`, `cbc:IssueDate`, `cac:AccountingSupplierParty`, `cac:AccountingCustomerParty`, `cac:LegalMonetaryTotal` |
| 1..n | `cac:InvoiceLine` |
| 0..1 | `ext:UBLExtensions`, `cbc:UBLVersionID`, `cbc:CustomizationID`, `cbc:ProfileID`, `cbc:ProfileExecutionID`, `cbc:CopyIndicator`, `cbc:UUID`, `cbc:IssueTime`, `cbc:DueDate`, `cbc:InvoiceTypeCode`, `cbc:TaxPointDate`, campos CBC de moneda, costos y conteo, `cac:OrderReference`, partes opcionales, `cac:DeliveryTerms`, tipos de cambio |
| 0..n | `cbc:Note`, períodos y referencias, `cac:Signature`, entregas, medios/términos de pago, pagos anticipados, cargos/descuentos, totales de tributos y retenciones |

La tabla anterior registra únicamente los elementos hijo inmediatos de `InvoiceType`; las cardinalidades internas de cada agregado se validan contra su tipo CAC correspondiente.

## 3. Elementos principales: tipo, uso y propósito

**Criterio SUNAT:** “Obligatorio” significa candidato obligatorio según el perfil de CPE que debe confirmarse con la normativa vigente; “Condicional” significa requerido solo cuando aplica al escenario; “No usado” indica fuera del subconjunto funcional propuesto. El estado legal final de cada fila permanece **POR VERIFICAR** contra anexos, reglas y catálogos SUNAT vigentes. La cardinalidad informada aquí sí es la del XSD local.

| Elemento UBL XML | Tipo XSD | Cardinalidad XSD | Propósito técnico | Uso SUNAT preliminar |
|---|---|---:|---|---|
| `ext:UBLExtensions` | `ext:UBLExtensionsType` | 0..1 | Aloja extensiones no cubiertas por el modelo base. | Obligatorio en el perfil firmado propuesto; exigencia exacta **POR VERIFICAR**. |
| `cbc:UBLVersionID` | `cbc:UBLVersionIDType` | 0..1 | Identifica versión UBL declarada. | Obligatorio; valor/perfil **POR VERIFICAR** con norma vigente. |
| `cbc:CustomizationID` | `cbc:CustomizationIDType` | 0..1 | Identifica la personalización del documento. | Obligatorio; valor peruano **POR VERIFICAR**. |
| `cbc:ProfileID` | `cbc:ProfileIDType` | 0..1 | Identifica perfil de proceso. | Condicional / valor **POR VERIFICAR**. |
| `cbc:ID` | `cbc:IDType` | 1..1 | Identificación externa del comprobante. | Obligatorio; relación RUC-tipo-serie-correlativo se valida fuera del XSD. |
| `cbc:IssueDate` | `cbc:IssueDateType` | 1..1 | Fecha de emisión. | Obligatorio; coherencia temporal/plazo **POR VERIFICAR** con normativa vigente. |
| `cbc:IssueTime` | `cbc:IssueTimeType` | 0..1 | Hora de emisión. | Condicional / obligatoriedad **POR VERIFICAR**. |
| `cbc:DueDate` | `cbc:DueDateType` | 0..1 | Fecha de vencimiento. | Condicional; no usar como fecha de emisión. |
| `cbc:InvoiceTypeCode` | `cbc:InvoiceTypeCodeType` | 0..1 | Clasifica tipo de factura según perfil. | Obligatorio para CPE; códigos/listID **POR VERIFICAR** en Catálogo 01 y perfil. |
| `cbc:DocumentCurrencyCode` | `cbc:DocumentCurrencyCodeType` | 0..1 | Moneda principal del comprobante. | Obligatorio en el perfil propuesto; códigos según Catálogo 02, versión **POR VERIFICAR**. |
| `cbc:Note` | `cbc:NoteType` | 0..n | Texto complementario. | Condicional; no sustituye campos codificados. |
| `cac:BillingReference` | `cac:BillingReferenceType` | 0..n | Enlaza documentos de facturación asociados. | Condicional; reglas según tipo de comprobante/motivo **POR VERIFICAR**. |
| `cac:Signature` | `cac:SignatureType` | 0..n | Metadatos de negocio sobre signatario/referencia de firma. | Condicional o requerido por perfil; confirmar Anexo aplicable. No equivale por sí solo a la firma XMLDSig. |
| `cbc:CopyIndicator` | `cbc:CopyIndicatorType` | 0..1 | Indica si el documento es copia. | No usado en el subconjunto de emisión inicial; no implica prohibición SUNAT. |
| `cac:AccountingSupplierParty` | `cac:SupplierPartyType` | 1..1 | Datos del proveedor/emisor. | Obligatorio; identificación y domicilio fiscal según perfil SUNAT **POR VERIFICAR**. |
| `cac:AccountingCustomerParty` | `cac:CustomerPartyType` | 1..1 | Datos del cliente/receptor. | Obligatorio como agregado XSD; campos mínimos según tipo de comprobante **POR VERIFICAR**. |
| `cac:Delivery` | `cac:DeliveryType` | 0..n | Datos de entrega. | Condicional al escenario/perfil; regla exacta **POR VERIFICAR**. |
| `cac:PaymentMeans` | `cac:PaymentMeansType` | 0..n | Medio o instrucción de pago. | Condicional; codificación SUNAT **POR VERIFICAR**. |
| `cac:PaymentTerms` | `cac:PaymentTermsType` | 0..n | Condiciones/plazos de pago. | Condicional; tratamiento de contado/crédito y cuotas **POR VERIFICAR**. |
| `cac:PayeeParty` | `cac:PartyType` | 0..1 | Tercero beneficiario del pago. | No usado en el alcance inicial; condición de uso SUNAT **POR VERIFICAR**. |
| `cac:AllowanceCharge` | `cac:AllowanceChargeType` | 0..n | Descuentos y cargos globales. | Condicional; reglas de signo, base y motivo **POR VERIFICAR**. |
| `cac:TaxExchangeRate` | `cac:ExchangeRateType` | 0..1 | Tipo de cambio entre moneda de documento e impuestos. | No usado en flujo PEN inicial; condicional si hay moneda distinta, reglas **POR VERIFICAR**. |
| `cac:TaxTotal` | `cac:TaxTotalType` | 0..n | Totales por esquema tributario, con subtotales. | Obligatorio según tributos aplicables; Catálogo 05 y datos por tributo **POR VERIFICAR**. |
| `cac:WithholdingTaxTotal` | `cac:TaxTotalType` | 0..n | Total de tributos retenidos. | No usado en alcance base; régimen/representación **POR VERIFICAR**. |
| `cac:LegalMonetaryTotal` | `cac:MonetaryTotalType` | 1..1 | Agregado de importes del documento. | Obligatorio; coherencia aritmética no garantizada por XSD. |
| `cac:InvoiceLine` | `cac:InvoiceLineType` | 1..n | Detalle de bienes/servicios. | Obligatorio; perfil de campos por línea **POR VERIFICAR**. |
| `cbc:InvoicedQuantity` (en línea) | `cbc:InvoicedQuantityType` | 0..1 | Cantidad facturada y unidad. | Condicional/obligatorio por perfil; unidad y precisión **POR VERIFICAR**. |
| `cbc:LineExtensionAmount` (línea) | `cbc:LineExtensionAmountType` | 1..1 | Importe de línea neto de impuestos conforme definición UBL. | Obligatorio por línea; cálculo debe reconciliar con detalle. |
| `cac:Item` (en línea) | `cac:ItemType` | 1..1 | Identidad y descripción del bien/servicio. | Obligatorio; atributos comerciales exigibles **POR VERIFICAR**. |
| `cac:Price` (en línea) | `cac:PriceType` | 0..1 | Precio asociado a la línea. | Condicional estructural; obligatoriedad en perfil SUNAT **POR VERIFICAR**. |

Los tipos CBC son tipos nominales definidos en `UBL-CommonBasicComponents-2.1.xsd`; sus restricciones internas incluyen atributos como `currencyID`/`unitCode` en cantidades, medidas o importes. No se debe asumir que todos los importes emplean el mismo tipo XSD.

### 3.1 Elementos de totales y tributos relevantes

| Ubicación | Elemento | Tipo común | Cardinalidad en agregado | Significado XSD |
|---|---|---|---:|---|
| `cac:TaxTotal` | `cbc:TaxAmount` | `cbc:TaxAmountType` | 1..1 | Total de impuesto del esquema. |
| `cac:TaxTotal` | `cac:TaxSubtotal` | `cac:TaxSubtotalType` | 0..n | Subtotal por categoría/esquema. |
| `cac:TaxSubtotal` | `cbc:TaxableAmount` | `cbc:TaxableAmountType` | 0..1 | Base neta a la que se aplica tasa. |
| `cac:TaxSubtotal` | `cbc:TaxAmount` | `cbc:TaxAmountType` | 1..1 | Importe del subtotal tributario. |
| `cac:TaxSubtotal` | `cac:TaxCategory` | `cac:TaxCategoryType` | 1..1 | Categoría tributaria, tasa y/o motivo. |
| `cac:TaxCategory` | `cac:TaxScheme` | `cac:TaxSchemeType` | 1..1 | Esquema/código de tributo. |
| `cac:LegalMonetaryTotal` | `cbc:LineExtensionAmount`, `TaxExclusiveAmount`, `TaxInclusiveAmount` | tipos CBC Amount | 0..1 cada uno | Agregados monetarios antes/después de impuestos, según definición UBL. |
| `cac:LegalMonetaryTotal` | `cbc:PayableAmount` | `cbc:PayableAmountType` | 1..1 | Importe a pagar. |

La cardinalidad del agregado `TaxTotal` es 0..n en el root, aunque cada instancia exige `TaxAmount`. Que sea válido frente al XSD no demuestra que el documento incluya todos los tributos debidos o que las bases y sumas sean correctas.

## 4. Firma digital y metadatos dentro de `ext:UBLExtensions`

### 4.1 Estructura definida por los XSD

`InvoiceType` permite cero o un `ext:UBLExtensions`. Si se incluye, el tipo `UBLExtensionsType` exige una o más extensiones. Cada `UBLExtensionType` admite metadatos EXT opcionales (`cbc:ID`, `cbc:Name`, agencia, versión, URI y motivo) y exige exactamente un `ext:ExtensionContent`. Este contenido contiene exactamente un elemento raíz de namespace distinto de EXT (`xsd:any`, `processContents="lax"`).

Una estructura orientativa para la firma XMLDSig es:

```xml
<ext:UBLExtensions>
  <ext:UBLExtension>
    <ext:ExtensionContent>
      <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <ds:SignedInfo>
          <ds:CanonicalizationMethod Algorithm="..."/>
          <ds:SignatureMethod Algorithm="..."/>
          <ds:Reference URI="">
            <ds:Transforms>...</ds:Transforms>
            <ds:DigestMethod Algorithm="..."/>
            <ds:DigestValue>...</ds:DigestValue>
          </ds:Reference>
        </ds:SignedInfo>
        <ds:SignatureValue>...</ds:SignatureValue>
        <ds:KeyInfo><ds:X509Data><ds:X509Certificate>...</ds:X509Certificate></ds:X509Data></ds:KeyInfo>
      </ds:Signature>
    </ext:ExtensionContent>
  </ext:UBLExtension>
</ext:UBLExtensions>
```

Es una **plantilla estructural**, no una receta de algoritmos aprobada por SUNAT. El XSD local incorpora la gramática XMLDSig y XAdES, pero no determina por sí mismo el perfil de firma aceptado, algoritmos permitidos, política de certificados, canonicalización o política de revocación. Valores concretos: **POR VERIFICAR** con la guía técnica SUNAT vigente.

### 4.2 Firma comercial UBL y firma criptográfica

- `cac:Signature` está en la secuencia del `Invoice` y utiliza `cac:SignatureType`. Ese agregado contiene, entre otros, un `cbc:ID` requerido, datos opcionales del signatario y de un adjunto de firma. Es metadato empresarial UBL.
- `ds:Signature` es el bloque criptográfico XMLDSig. Se ubica como contenido de extensión conforme al perfil peruano a confirmar. Su `SignedInfo` referencia métodos, transformaciones y digest; `SignatureValue` lleva la firma Base64; `KeyInfo/X509Data` puede transportar certificado.
- `sac:SignatureInformation` es otro agregado UBL disponible vía módulos de firma: el tipo local contiene `cbc:ID?`, `sbc:ReferencedSignatureID?` y `ds:Signature?`. No se asumirá que reemplaza la ubicación de `ds:Signature` requerida por el perfil SUNAT.
- No incluir datos privados, clave privada ni contraseñas en XML, schemas o ejemplos.

**Fuentes estructurales:** `UBL-CommonExtensionComponents-2.1.xsd`, `UBL-ExtensionContentDataType-2.1.xsd`, `UBL-CommonAggregateComponents-2.1.xsd`, `UBL-CommonSignatureComponents-2.1.xsd`, `UBL-SignatureAggregateComponents-2.1.xsd` y `UBL-xmldsig-core-schema-2.1.xsd`, todos bajo `docs/compliance/reference/ubl/common/`. **Fuente normativa peruana para ubicación/perfil de firma:** RS 097-2012/SUNAT, anexos técnicos y modificatorias; número de numeral y perfil vigente **POR VERIFICAR**.

### 4.3 Extensiones y datos peruanos

UBL admite extensiones de namespace externo, pero no define en este XSD un vocabulario peruano de metadatos. Por tanto:

- La firma XMLDSig es candidata a contenido de extensión; ubicación, estructura exacta y cardinalidad exigidas por SUNAT: **POR VERIFICAR**.
- Los datos propios del perfil peruano (p. ej., metadatos específicos del emisor, información adicional, leyendas o referencias) se mapearán primero a elementos CBC/CAC estándar cuando el perfil SUNAT así lo defina. No crear nombres XML de extensión por inferencia.
- Toda extensión privada deberá declarar namespace, propósito, versión y reglas de validación; requerimiento de admisión por SUNAT/OSE: **POR VERIFICAR**.

## 5. Reglas de negocio que el XSD no valida por sí solo

El XSD puede comprobar forma, orden, cardinalidad, tipos simples y algunos atributos. No impone reglas semánticas cruzadas, vigencia registral ni consistencia tributaria. La siguiente lista es una matriz de controles de diseño; los requisitos SUNAT específicos deben cotejarse con RS 097-2012/SUNAT y sus anexos/modificatorias y guías vigentes.

| Control fuera del XSD | Ejemplo de validación de aplicación/perfil | Fuente que debe gobernar | Estado |
|---|---|---|---|
| RUC del emisor + tipo/serie/correlativo del ID | El emisor del XML debe corresponder al prefijo RUC del nombre de archivo; serie/prefijo y tipo de CPE deben ser compatibles; correlativo no se duplica. | RS 097-2012/SUNAT y anexos; reglas de nomenclatura/envío vigentes. | POR VERIFICAR los formatos exactos y excepciones. |
| Identidad de emisor | Validar RUC, estado/condición y autorización de emisión en fuentes SUNAT antes de emitir/enviar. | D. Leg. 943, RS 210-2004/SUNAT y consulta/padrón SUNAT. | POR VERIFICAR mecanismo/criterios vigentes. |
| Identidad de receptor | Coherencia entre `schemeID`, tipo documental, longitud/formato y datos del cliente según CPE. | Anexos de RS 097-2012/SUNAT y Catálogo 06. | POR VERIFICAR requisitos por tipo/documento y umbrales. |
| Serie y numeración | Prefijo/código de serie permitido para tipo de CPE, secuencia única por emisor/serie y ausencia de reutilización. | RS 097-2012/SUNAT y modificatorias. | POR VERIFICAR reglas exactas actuales. |
| Catálogos | Compatibilidad entre `InvoiceTypeCode`, `TaxScheme`, categoría/afectación, tipo de identificación y códigos de moneda. | Catálogos SUNAT 01, 02, 05, 06, 07 y 08, publicación vigente. | POR VERIFICAR versiones, códigos y `listID`. |
| Cálculo y sumatorias | Cantidad × precio, cargos/descuentos, bases, impuestos, totales por categoría y total a pagar deben reconciliarse. | TUO IGV/ISC, reglamento y anexos/guías de comprobantes SUNAT. | Fórmulas específicas: POR VERIFICAR en Hito 3. |
| Moneda y precisión | Coherencia entre moneda del documento, códigos de moneda en Amount y tipos de cambio cuando se usen. | Catálogo 02 y especificación técnica SUNAT vigente. | POR VERIFICAR reglas/decimales admitidos. |
| Referencias de notas | CPE afectado existente, tipo y motivo compatibles; importes y líneas coherentes con la modificación. | RS 097-2012/SUNAT, anexos y catálogos de motivos aplicables. | POR VERIFICAR. |
| Fecha y ventana de envío | Fecha no futura cuando aplique; envío dentro del plazo y zona horaria/fecha de operación coherentes. | RS 097-2012/SUNAT y modificatorias vigentes. | Plazos y excepciones POR VERIFICAR. |
| Integridad de firma | Validar digest, referencia al documento, certificado, cadena, período de validez y confianza; no basta con que `ds:Signature` sea XML válido. | Ley 27269 y reglamento; guía/perfil de firma SUNAT vigente. | Perfil técnico exacto POR VERIFICAR. |
| Estado y duplicidad | Procesar respuesta SUNAT/OSE, distinguir recibido/aceptado/observado/rechazado y prevenir reenvíos inconsistentes. | Servicios y guías oficiales SUNAT/OSE vigentes. | Estados/códigos POR VERIFICAR. |
| Coherencia con CDR | Correlacionar emisor, tipo, serie, correlativo y hash/identificadores entre XML transmitido y CDR recibido. | Anexo técnico y contrato de servicio SUNAT/OSE. | POR VERIFICAR. |

## 6. Referencias y pendientes normativos

- **UBL:** OASIS UBL 2.1, `UBL-Invoice-2.1.xsd` y esquemas comunes locales (versión indicada en encabezado de los XSD: 04-11-2013).
- **Perú:** RS 097-2012/SUNAT, anexos y modificatorias; RS 234-2006/SUNAT para libros y registros contables/tributarios, sin asumir que esta última determina los requisitos del XML de emisión.
- **Catálogos:** SUNAT 01, 02, 05, 06, 07 y 08 en versión vigente.
- **Firma:** XML Signature Syntax and Processing (W3C XMLDSig) y, solo si el perfil oficial lo pide, XAdES. Algoritmos y perfil peruano exacto: **POR VERIFICAR**.

**Resultado del hito:** estructura UBL base identificada. La clasificación obligatoria/condicional SUNAT, los códigos de catálogos, la ubicación normativa de firma y las reglas cruzadas quedan sujetos a cotejo con fuentes oficiales vigentes antes de usarse como criterio de aceptación de producción.
