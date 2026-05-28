# platform-ai-architecture

Repositorio documental del portafolio AI. Este repo es la fuente de verdad para el learning path, las decisiones arquitect?nicas, el glosario compartido y las fichas de cada proyecto del portafolio.

## Proposito

- documentar el learning path que guia el portafolio;
- mapear qu? proyecto cubre cada punto del roadmap;
- registrar decisiones cerradas mediante ADRs;
- mantener definiciones estables para conceptos como `ai-gateway`, `knowledge-rag`, `mcp-server` y `openclaw-ops`;
- servir como punto de arranque para conversaciones futuras sobre arquitectura.

## Navegacion

- `docs/roadmap/learning-path.md`: version limpia del learning path y su cobertura.
- `docs/projects/`: ficha por proyecto del portafolio.
- `docs/adr/`: decisiones arquitectonicas cerradas.
- `docs/glossary/`: definiciones de conceptos compartidos.
- `docs/discovery/`: contexto y preguntas abiertas en definicion.
- `AGENTS.md`: reglas para agentes y mantenimiento documental.

## Resumen del learning path

El learning path enumera habilidades importantes del portafolio; no define prioridad ni dificultad por posicion. La secuencia practica de entrega puede variar segun los recursos que se quieran exponer primero.

| Punto | Capability | Proyecto principal | Consumidores / soporte |
| --- | --- | --- | --- |
| 1 | OpenClaw como interfaz operativa | `openclaw-ops` | `mcp-server`, `cost-console`, `auth-service` |
| 2 | RAG y conocimiento recuperable | `knowledge-rag` | `other-gpt`, `mcp-server`, `ai-gateway` |
| 3 | MCP y tools reutilizables | `mcp-server` | `other-gpt`, `openclaw-ops`, `auth-service` |
| 4 | Frontend conversacional | `other-gpt` | `auth-service`, `ai-gateway` |
| 5 | Seguridad y gobierno | transversal | todos los proyectos |
| 6 | Orquestacion multi-provider | `ai-gateway` | `other-gpt`, `cost-console`, `mcp-server` |
| 7 | Calculadora de tokens, embeddings y costo RAG | `cost-console` | `ai-gateway`, `other-gpt`, `openclaw-ops` |
| 8 | Voz | `other-gpt` | `ai-gateway` |

## Proyectos del portafolio

- `other-gpt`: satelite de chat multimodal y futura voz; recurso publico/demostrable temprano.
- `auth-service`: producto base de autenticacion centralizada con aislamiento por proyecto; necesario para exponer proyectos publicos con roles y operar accesos desde OpenClaw.
- `ai-gateway`: orquestacion multi-provider, control-plane de proveedores y politicas de inferencia; importante, pero puede madurar despues de la primera exposicion publica.
- `knowledge-rag`: ingest, chunking, embeddings, retrieval y citas; importante, pero secundario hasta aclarar mejor su caso de uso.
- `mcp-server`: superficie estandar de tools para apps y agentes; puede ser necesario temprano para operar auth desde OpenClaw.
- `openclaw-ops`: interfaz operativa basada en OpenClaw y Telegram; recurso publico/demostrable temprano y primera superficie admin para acciones delegadas de auth.
- `cost-console`: playground fullstack para calcular costo de chat, embeddings y consultas vectoriales, con persistencia propia en PostgreSQL; recurso publico/demostrable temprano.
- `admin-console`: panel administrativo de usuarios, sesiones, permisos y presupuestos; ultima prioridad practica.

## Decisiones registradas

- [ADR 0001](./docs/adr/0001-use-platform-ai-architecture-as-portfolio-source-of-truth.md): este repo es la fuente documental del portafolio.
- [ADR 0002](./docs/adr/0002-adopt-multi-repo-portfolio-with-shared-packages.md): el portafolio adopta estrategia multi-repo con paquetes compartidos.
- [ADR 0003](./docs/adr/0003-centralize-auth-with-project-isolation.md): auth-service controla identidad con aislamiento por proyecto.
- [ADR 0004](./docs/adr/0004-separate-chat-persistence-assets-rag-and-mcp.md): separar persistencia de chat, registro de assets, RAG y MCP.
- [ADR 0005](./docs/adr/0005-use-ai-gateway-as-provider-control-plane.md): usar ai-gateway como control-plane de proveedores y respetar configuracion explicita.
- [ADR 0006](./docs/adr/0006-auth-service-runtime-quality-and-hosting.md): fijar runtime, tooling de calidad, hosting inicial y OpenClaw como superficie admin de auth.
- [ADR 0007](./docs/adr/0007-define-cost-console-as-token-and-rag-cost-playground.md): definir cost-console como playground fullstack de costos de chat y RAG con PostgreSQL desde el inicio.

## Como continuar

1. Leer `docs/roadmap/learning-path.md` para entender el objetivo del portafolio.
2. Revisar `docs/adr/` para conocer decisiones cerradas.
3. Consultar `docs/projects/` y `docs/glossary/` antes de proponer nuevos cambios.
4. Registrar nuevas decisiones importantes con ADRs y actualizar este `README.md` cuando cambie el mapa del portafolio.
