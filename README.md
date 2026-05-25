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

| Punto | Capability | Proyecto principal | Consumidores / soporte |
| --- | --- | --- | --- |
| 1 | OpenClaw como interfaz operativa | `openclaw-ops` | `mcp-server`, `cost-console`, `auth-service` |
| 2 | RAG y conocimiento recuperable | `knowledge-rag` | `other-gpt`, `mcp-server`, `ai-gateway` |
| 3 | MCP y tools reutilizables | `mcp-server` | `other-gpt`, `openclaw-ops` |
| 4 | Frontend conversacional | `other-gpt` | `auth-service`, `ai-gateway` |
| 5 | Seguridad y gobierno | transversal | todos los proyectos |
| 6 | Orquestacion multi-provider | `ai-gateway` | `other-gpt`, `cost-console`, `mcp-server` |
| 7 | Costos, pricing y observabilidad | `cost-console` | `ai-gateway`, `other-gpt`, `openclaw-ops` |
| 8 | Voz | `other-gpt` | `ai-gateway` |

## Proyectos del portafolio

- `other-gpt`: satelite de chat multimodal y futura voz.
- `auth-service`: producto base de autenticacion centralizada con aislamiento por proyecto.
- `ai-gateway`: orquestacion multi-provider y politicas de inferencia.
- `knowledge-rag`: ingest, chunking, embeddings, retrieval y citas.
- `mcp-server`: superficie estandar de tools para apps y agentes.
- `openclaw-ops`: interfaz operativa basada en OpenClaw y Telegram.
- `cost-console`: dashboard de consumo, pricing y recomendacion de modelos.
- `admin-console`: panel administrativo de usuarios, sesiones, permisos y presupuestos.

## Decisiones registradas

- [ADR 0001](./docs/adr/0001-use-platform-ai-architecture-as-portfolio-source-of-truth.md): este repo es la fuente documental del portafolio.
- [ADR 0002](./docs/adr/0002-adopt-multi-repo-portfolio-with-shared-packages.md): el portafolio adopta estrategia multi-repo con paquetes compartidos.
- [ADR 0003](./docs/adr/0003-centralize-auth-with-project-isolation.md): auth-service controla identidad con aislamiento por proyecto.
- [ADR 0004](./docs/adr/0004-separate-chat-persistence-assets-rag-and-mcp.md): separar persistencia de chat, registro de assets, RAG y MCP.

## Como continuar

1. Leer `docs/roadmap/learning-path.md` para entender el objetivo del portafolio.
2. Revisar `docs/adr/` para conocer decisiones cerradas.
3. Consultar `docs/projects/` y `docs/glossary/` antes de proponer nuevos cambios.
4. Registrar nuevas decisiones importantes con ADRs y actualizar este `README.md` cuando cambie el mapa del portafolio.
