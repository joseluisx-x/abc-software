# ADR-001: Stack tecnológico multiplataforma

- **Estado:** Propuesto, decisión pendiente de prototipo.
- **Contexto:** el enunciado exige MSI para Windows y APK para Android. El árbol físico contiene marcador .NET backend y marcador Flutter desktop/mobile, lo cual es una señal inicial, no una decisión aprobada.
- **Decisión provisional:** evaluar backend .NET y cliente Flutter, verificando que Flutter cubra Android y Windows con empaquetado MSI externo. Mantener .NET MAUI y Kotlin Multiplatform en la comparación documentada en `docs/architecture/ARCHITECTURE.md`.
- **Alternativas:** .NET MAUI; Flutter; Kotlin Multiplatform; clientes separados con dominio/contratos compartidos.
- **Consecuencias:** requiere prototipo de firma XMLDSig, SOAP, almacenamiento seguro, modo de red elegido, actualización, MSI firmado y APK firmado antes de cerrar la elección.
- **Pendiente:** experiencia del equipo, versión de toolchain, estrategia de instalador/distribución y restricciones de dispositivos.