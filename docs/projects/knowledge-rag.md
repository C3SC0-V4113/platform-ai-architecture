# knowledge-rag

## Proposito

Servicio de ingestion, chunking, embeddings, retrieval y citas para conocimiento recuperable.

Sigue siendo una habilidad importante del portafolio, pero su implementacion puede quedar secundaria hasta que exista un caso de uso mas claro y demostrable.

## Punto(s) del learning path

- principal: `2`;
- aplica partes de `5`.

## Tipo de proyecto

- backend de conocimiento.

## Arquetipo frontend si aplica

- consola de ingest opcional con formularios de carga, estado de indexacion y pruebas de retrieval.

## Stack recomendado

- `Fastify`
- `TypeScript`
- `PostgreSQL`
- `pgvector`
- `Supabase` como plataforma recomendada al inicio

## Que guarda

- referencias a fuentes de conocimiento;
- texto extraido;
- chunks;
- embeddings;
- citas;
- ACLs de consulta;
- estado de indexacion.

## Que no guarda

- la copia maestra del archivo como source of truth unico;
- sesiones de auth;
- mensajes de chat;
- definiciones de tools MCP.

## Con quien habla

- `asset/document registry`;
- `ai-gateway`;
- `mcp-server`;
- `other-gpt` de forma indirecta.

## Riesgos

- convertirlo en almacenamiento generico de adjuntos y perder frontera conceptual;
- indexar sin permisos claros.
- implementarlo por obligacion conceptual antes de entender que conocimiento concreto debe recuperar.

## Estado esperado en el portafolio

Proyecto visible del punto `2`, especializado en retrieval y grounding. Debe madurar cuando se defina mejor que valor aporta al portafolio y como se integra con permisos, assets y experiencias visibles.
