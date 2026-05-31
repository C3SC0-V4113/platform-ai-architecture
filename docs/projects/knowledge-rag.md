# knowledge-rag

## Proposito

App RAG standalone para ingestion, chunking, embeddings, retrieval y citas sobre un corpus universitario curado compuesto por Google Drive y repositorios publicos de GitHub.

Sigue siendo una habilidad importante del portafolio, pero ya no se posiciona como parte del ecosistema central. Su rol inicial es demostrar el punto `2` del learning path con un corpus externo real y acotado.

## Punto(s) del learning path

- principal: `2`;
- aplica partes de `5`.

## Tipo de proyecto

- app RAG standalone.

## Arquetipo frontend si aplica

- consola de ingest opcional con estado de sincronizacion de fuentes `Drive` y `GitHub`, indexacion y pruebas de retrieval con citas.

## Stack recomendado

- `Fastify`
- `TypeScript`
- `PostgreSQL`
- `pgvector`
- `Google Drive API`
- `GitHub` como fuente publica de repositorios
- `Supabase` como plataforma recomendada al inicio

## Que guarda

- referencias a carpetas de Google Drive y repositorios publicos de GitHub;
- metadata academica por `materia`, `semestre`, `origen` y `tipo_fuente`;
- texto extraido;
- chunks;
- embeddings;
- citas;
- estado de indexacion y sincronizacion.

## Que no guarda

- la copia maestra del archivo como source of truth unico;
- sesiones de `auth-service`;
- mensajes de chat del ecosistema;
- definiciones de tools MCP.

## Con quien habla

- `Google Drive`;
- `GitHub`;
- proveedores de embeddings o chat;
- base vectorial y storage propios.

## Riesgos

- sobreajustarlo a una carpeta o repo puntual y perder capacidad de generalizacion;
- depender de cambios de estructura, permisos o limites de Google Drive;
- introducir ruido por indexar repositorios completos sin filtros suficientes;
- convertirlo en almacenamiento generico de adjuntos y perder frontera conceptual.

## Estado esperado en el portafolio

Proyecto visible del punto `2`, especializado en retrieval y grounding, pero operado fuera del ecosistema central del portafolio. Su primera forma debe ser una app RAG demostrable sobre un corpus universitario curado de `Google Drive` y `GitHub`, organizado principalmente por `materia` y `semestre`; cualquier integracion futura con `other-gpt`, `mcp-server` o `ai-gateway` queda como evolucion opcional posterior.
