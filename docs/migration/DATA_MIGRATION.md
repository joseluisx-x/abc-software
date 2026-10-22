# Migración de datos legados

## Fuente

El enunciado describe fichas operativas con `fecha`, `ingreso`, `salida`, `producto` y `nro_documento` informal. El formato real, significado, volumen, duplicados y unidades están **POR VERIFICAR** con el responsable del sistema origen.

## Mapeo inicial

| Campo origen | Destino candidato | Validación/transformación |
|---|---|---|
| `fecha` | Fecha de movimiento o fecha de comprobante | Determinar semántica y formatos; conservar original y zona/interpretación. |
| `ingreso` | Movimiento de inventario de entrada o valor contable separado | No adivinar unidad ni si es cantidad/importe; solicitar diccionario y muestras. |
| `salida` | Movimiento de inventario de salida o valor contable separado | Determinar semántica; validar cantidad y unidad por producto. |
| `producto` | `Producto`/SKU y línea de movimiento/venta | Normalizar con regla trazable; resolver duplicados con clave maestra. |
| `nro_documento` | Identificador original y posible referencia externa | No convertir a serie/correlativo CPE sin evidencia documental verificable. |

## Proceso

1. Extraer una copia de solo lectura y registrar lote, archivo, checksum, fecha y sistema origen.
2. Cargar registros brutos a staging sin sobrescritura.
3. Perfilar fechas, duplicados, productos, unidades, nulos, numeraciones informales y consistencia de entradas/salidas.
4. Transformar mediante reglas versionadas, conservando valor bruto, valor normalizado, regla y motivo.
5. Enviar inconsistencias/ambigüedades a cuarentena con resolución manual auditada.
6. Cargar entidades y movimientos con referencia al lote/fila fuente.
7. Conciliar conteos, cantidades y saldos por producto/período; obtener aprobación de Contabilidad/operación.
8. Ejecutar ensayo y rollback antes de la carga productiva.

## Controles de aceptación

- 100% de filas trazables a archivo y número de fila de origen.
- Ninguna numeración interna se presenta como CPE fiscal sin evidencia.
- Ningún registro rechazado desaparece: permanece en staging/cuarentena con causa.
- Conteos y saldos de apertura/cierre reconciliados y aprobados.
- Estrategia de retención, privacidad y disposición de fuente aprobada.