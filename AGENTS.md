# AGENTS.md

## Naturaleza del proyecto

`platform-ai-architecture` es un repo documental. Aqui no se implementan productos ni servicios; aqui se documenta el portafolio AI, su learning path, sus decisiones y sus conceptos compartidos.

## Jerarquia documental

1. `README.md`
   - mapa general del repo y del portafolio.
2. `docs/roadmap/learning-path.md`
   - fuente primaria del learning path y de la cobertura de puntos.
3. `docs/adr/`
   - decisiones cerradas de arquitectura.
4. `docs/projects/`
   - ficha por proyecto del portafolio.
5. `docs/glossary/`
   - definiciones estables de conceptos.
6. `docs/discovery/`
   - contexto, notas de definicion y preguntas abiertas.

## Reglas para agentes

- No tratar este repo como repo de implementacion.
- Antes de proponer cambios, leer en este orden:
  1. `docs/roadmap/learning-path.md`
  2. `docs/adr/README.md`
  3. la ficha relevante en `docs/projects/`
  4. el termino relevante en `docs/glossary/`
- Si una decision ya esta cerrada en ADR, no reabrirla sin explicitar por que.
- Si una definicion cambia, sincronizar:
  - `README.md`
  - `docs/projects/` afectados
  - `docs/glossary/` afectados
  - `docs/adr/` si se trata de una decision estructural

## Skills

- Skill local disponible en este repo:
  - `.agents/skills/architecture-decision-records/SKILL.md`
- Usar `architecture-decision-records` cuando se cree, actualice o superseda una decision significativa.
- El lock de skills vive en `skills-lock.json` para trazabilidad.

## Convencion de documentacion

- `docs/discovery/` es para exploracion y definicion abierta.
- `docs/adr/` es para decisiones cerradas.
- `docs/projects/` describe el estado objetivo y las fronteras de cada proyecto.
- `docs/glossary/` fija el significado de los terminos para evitar ambiguedades.

## Criterio de cierre

No consideres una actualizacion documental completa si:

- el learning path no refleja el cambio,
- el proyecto afectado no tiene su ficha actualizada,
- o falta ADR para una decision estructural nueva.
