# ADR 0001: Use `platform-ai-architecture` as Portfolio Source of Truth

- Date: 2026-05-24
- Status: Accepted

## Context

Las decisiones del portafolio AI estaban distribuidas entre conversaciones, notas sueltas y repos de implementacion. Eso mezcla contexto exploratorio con codigo de productos concretos y hace dificil responder:

- que punto del learning path cubre cada proyecto,
- que decisiones ya estan cerradas,
- que terminos tienen significado estable,
- y como se relacionan los repos entre si.

## Decision Drivers

- Necesidad de una fuente unica de verdad documental.
- Separar arquitectura de portafolio de arquitectura local de cada producto.
- Facilitar continuidad de conversaciones y onboarding.
- Mantener trazabilidad de decisiones estructurales.

## Decision

Se adopta `platform-ai-architecture` como repositorio documental principal del portafolio AI.

Este repo sera responsable de:

- documentar el learning path y su cobertura,
- mantener ADRs del portafolio,
- mantener fichas de proyectos,
- definir glosario compartido,
- y almacenar contexto de discovery que aun no sea decision cerrada.

## Consequences

### Positive

- La documentacion del portafolio deja de depender de un repo de implementacion especifico.
- Hay una entrada unica para entender el estado del plan.
- Las decisiones pueden evolucionar sin contaminar repos de producto.

### Negative

- Requiere disciplina extra para mantener sincronizadas fichas y ADRs.
- Puede existir drift entre este repo y los repos de implementacion si no se actualiza cuando cambian fronteras reales.

## Related Decisions

- ADR 0002 define la estrategia multi-repo que este repo documenta.
- ADR 0004 define separaciones de responsabilidad que dependen de este repo como fuente documental.
