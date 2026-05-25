# ADR 0004: Separate Chat Persistence, Assets, RAG and MCP

- Date: 2026-05-24
- Status: Accepted

## Context

Durante la definicion del portafolio aparecio una confusion importante: usar `knowledge-rag` como si fuera al mismo tiempo almacenamiento de archivos, memoria de chat y retrieval. Tambien surgio la necesidad de que los adjuntos e imagenes generadas pudieran sobrevivir logout/login y ser reutilizables por distintos modelos sin volver a subirlos.

## Decision Drivers

- Persistencia real del chat y adjuntos por proyecto.
- Reutilizacion de assets entre proveedores de IA.
- Separacion clara entre source of truth de archivos, retrieval y tools.
- Mantener a `other-gpt` como satelite de experiencia, no como backend central de todo.

## Decision

Se separan cuatro responsabilidades:

1. `other-gpt`
   - satelite frontend del punto 4 y entrada futura al punto 8.
2. `chat persistence`
   - un chat continuo por usuario por proyecto.
3. `asset/document registry`
   - guarda el archivo can?nico y su metadata persistente.
4. `knowledge-rag`
   - indexa y recupera derivados semanticos de assets.
5. `mcp-server`
   - expone tools reutilizables; no es source of truth de datos.

Adicionalmente:

- los adjuntos seran persistentes por usuario y por proyecto;
- cada usuario tendra un chat continuo por proyecto;
- los assets podran existir asociados al chat y ademas entrar a una biblioteca persistente del usuario;
- `knowledge-rag` no sera el repositorio principal de archivos.

## Consequences

### Positive

- Se evita mezclar almacenamiento con retrieval.
- Los assets pueden reutilizarse entre OpenAI, Gemini, Claude u otros proveedores via `ai-gateway`.
- El portafolio gana fronteras mas claras entre experiencia, almacenamiento, retrieval y tools.

### Negative

- Se incrementa el numero de piezas conceptuales.
- Requiere modelar relaciones entre chat, asset registry y biblioteca de usuario.

## Related Decisions

- ADR 0002 define el contexto multi-repo donde estas piezas pueden existir separadas.
- ADR 0003 provee el aislamiento por proyecto necesario para esta persistencia.
