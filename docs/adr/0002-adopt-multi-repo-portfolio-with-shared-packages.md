# ADR 0002: Adopt Multi-Repo Portfolio with Shared Packages

- Date: 2026-05-24
- Status: Accepted

## Context

El portafolio incluye productos y servicios con ciclos de vida distintos: chat, autenticacion, orquestacion, costos, MCP, RAG y operaciones con OpenClaw. Mantenerlos en un solo repo acoplaria despliegues, ownership y decisiones tecnicas que no siempre cambian juntas.

Tambien existe necesidad de reutilizar contratos y clientes sin copiar codigo manualmente entre proyectos.

## Decision Drivers

- Separar despliegues y ownership por proyecto.
- Mantener al chat como satelite, no como centro estructural.
- Reutilizar contratos sin duplicacion manual.
- Hacer visible cada capability del roadmap como pieza del portafolio.

## Considered Options

### Option 1: Monorepo

- Pros: cambios atomicos y tooling unificado.
- Cons: mayor acoplamiento, el chat tenderia a absorber decisiones de toda la plataforma.

### Option 2: Multi-repo with shared packages

- Pros: mejor separacion de productos, despliegues independientes, narrativa mas clara de portafolio.
- Cons: versionado y sincronizacion mas estrictos.

## Decision

Se adopta una estrategia `multi-repo`.

La reutilizacion se resuelve mediante paquetes compartidos, preferiblemente npm privados o GitHub Packages, por ejemplo:

- `@org/contracts`
- `@org/auth-sdk`
- `@org/ai-sdk`
- `@org/provider-catalog`

## Consequences

### Positive

- Cada proyecto puede evolucionar y desplegarse de forma independiente.
- `other-gpt` deja de ser el centro implicito del portafolio.
- La arquitectura del roadmap se hace mas visible como sistema de proyectos relacionados.

### Negative

- Requiere disciplina alta de versionado.
- Incrementa el costo documental y de CI/CD por proyecto.

## Related Decisions

- ADR 0001 establece este repo como fuente documental del enfoque multi-repo.
- ADR 0003 y ADR 0004 refinan responsabilidades dentro de la estrategia multi-repo.
