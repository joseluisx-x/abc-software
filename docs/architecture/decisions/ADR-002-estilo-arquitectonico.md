# ADR-002: Estilo arquitectónico

- **Estado:** Aceptado como dirección de diseño; implementación no iniciada.
- **Contexto:** el enunciado solicita un monolito MVC inicial que permita evolución a Hexagonal/Clean y eventual extracción de microservicios.
- **Decisión:** monolito modular con controllers MVC delgados, casos de uso, dominio independiente y adaptadores para persistencia, UBL, firma e integración tributaria. Definir puertos desde Application/Domain; infraestructura depende de esos contratos.
- **Alternativas:** MVC acoplado; microservicios desde inicio; Clean Architecture completa desde el primer despliegue.
- **Consecuencias:** despliegue sencillo al inicio; límites y contratos facilitan pruebas y extracción futura. Evitar extraer microservicios sin necesidad de escala/equipo demostrada.