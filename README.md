# ABC Software Corp

Sistema en preparación para facturación electrónica e inventario de una empresa peruana de comercialización mayorista y minorista de abarrotes.

## Estructura

- `docs/`: especificaciones, dominio, contratos, arquitectura, API, compliance y migración.
- `docs/compliance/reference/`: enunciado original y XSD UBL 2.1 preservados como fuentes de referencia.
- `src/backend/`: esqueleto de backend .NET organizado por Api, Application, Domain e Infrastructure.
- `src/clients/mobile_desktop/`: esqueleto de cliente Flutter para evaluar Windows y Android.
- `tests/`: áreas reservadas para unitarias, integración, contratos y aceptación.
- `deploy/`: áreas reservadas para paquetes Windows y Android.
- `tools/`: utilidades y scripts del proyecto.

## Estado

Los `.txt` bajo `src/` son marcadores de estructura, no implementación. La solución .NET no contiene proyectos todavía; el paquete Flutter requiere completar `lib/` y plataformas antes de poder ejecutarse. Las decisiones de stack, persistencia, SUNAT/OSE y reglas tributarias no confirmadas están en `docs/specs/OPEN_QUESTIONS.md` y `docs/compliance/SOURCES_AND_PENDING_RULES.md`.

## Preparación

No hay un comando de compilación o ejecución del producto hasta crear proyectos reales y aprobar los ADRs. Para contribuir, revisar primero `docs/specs/SSD.md`, `docs/architecture/ARCHITECTURE.md` y `docs/compliance/UBL_VALIDATION_STRATEGY.md`.

No incluir secretos ni certificados privados en el repositorio. Usar el gestor de secretos del entorno y `/.env.example` solo como plantilla no secreta.