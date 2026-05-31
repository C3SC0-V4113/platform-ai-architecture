# ADR 0009: Position Knowledge RAG as an External Google Drive RAG App

- Date: 2026-05-30
- Status: Accepted

## Context

El portafolio ya habia separado persistencia de chat, registro de assets, RAG y MCP en ADR 0004. Sin embargo, `knowledge-rag` seguia descrito como si fuera un consumidor natural de `other-gpt`, `mcp-server` y `ai-gateway`, aunque su caso de uso real todavia no estaba claro dentro del ecosistema central.

Al mismo tiempo, aparecio un caso concreto y demostrable para el punto `2` del learning path: construir una aplicacion RAG capaz de leer una carpeta propia de Google Drive, sincronizar sus archivos, generar embeddings y responder preguntas con citas.

Mantener `knowledge-rag` dentro del ecosistema central sin ese caso de uso cerrado seguiria mezclando dos conversaciones distintas:

- la plataforma conversacional y multi-provider del portafolio;
- una app RAG especializada sobre un corpus externo concreto.

## Decision Drivers

- Darle al punto `2` un caso de uso real y demostrable.
- Evitar que `ai-gateway`, `other-gpt` o `mcp-server` carguen dependencias tempranas hacia RAG sin necesidad.
- Mantener claras las fronteras del ecosistema central del portafolio.
- Aprovechar Google Drive como fuente concreta de documentos para ingestion y retrieval.
- Preservar `knowledge-rag` como proyecto visible del learning path, aun si vive fuera del ecosistema central.

## Decision

`knowledge-rag` se define como una app RAG externa al ecosistema central del portafolio.

Su caso de uso inicial sera:

- leer una carpeta especifica de Google Drive;
- sincronizar metadata y contenido extraido de esos archivos;
- ejecutar chunking, embeddings, indexacion y retrieval;
- responder preguntas con citas sobre ese corpus.

Adicionalmente:

- `knowledge-rag` sigue siendo el proyecto principal del punto `2` del learning path;
- no se considera dependencia temprana de `other-gpt`, `ai-gateway` ni `mcp-server`;
- cualquier integracion futura con el ecosistema central sera opcional y posterior, no parte de la definicion inicial del proyecto.

## Consequences

### Positive

- El punto `2` gana una demo concreta y facil de explicar.
- El ecosistema central evita una dependencia prematura hacia RAG.
- Google Drive ofrece una fuente de documentos realista para ingestion y grounding.
- `knowledge-rag` puede evolucionar con foco propio sin distorsionar la frontera de `ai-gateway`.

### Negative

- `knowledge-rag` deja de ser una capacidad integrada temprana del ecosistema central.
- La historia de permisos cambia: al inicio depende mas del alcance de Google Drive y del control propio de la app que de `auth-service`.
- Habra que documentar despues si conviene o no una integracion con `other-gpt` o `mcp-server`.

## Implementation Notes

- La fuente primaria del corpus es una carpeta propia de Google Drive, identificada por folder ID o referencia equivalente.
- La app debe persistir al menos referencias de archivos, texto extraido, chunks, embeddings, citas y estado de sincronizacion.
- La copia maestra del archivo sigue viviendo en Google Drive; `knowledge-rag` guarda derivados y referencias.
- La demostracion inicial del punto `2` debe incluir ingestion end-to-end y chat grounded sobre ese corpus.

## Related Decisions

- ADR 0004 separa persistencia de chat, assets, RAG y MCP.
- ADR 0005 mantiene a `ai-gateway` como control-plane multi-provider y evita convertirlo en owner de retrieval.
- ADR 0007 mantiene visible el costo de ingestion y retrieval aunque `knowledge-rag` viva fuera del ecosistema central.
