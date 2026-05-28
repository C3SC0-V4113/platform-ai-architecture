# ADR 0007: Define Cost Console as Token and RAG Cost Playground

- Date: 2026-05-28
- Status: Accepted

## Context

La definicion inicial de `cost-console` lo acercaba a un dashboard de costos y recomendacion economica. Esa descripcion era demasiado amplia y dejaba ambiguo el verdadero objetivo del punto `7` del learning path.

El learning path pide cubrir de forma explicita:

- costo de chats;
- tokens de `input`, `output` y contexto;
- costo de embeddings para carga inicial;
- costo de embeddings y contexto durante consultas a base vectorial.

Tambien existia duda sobre si `cost-console` debia depender temprano de otras apps del portafolio o si podia nacer como producto util por si mismo.

## Decision Drivers

- Alinear `cost-console` con el nucleo real del punto `7`.
- Evitar que el proyecto se convierta primero en dashboard administrativo y deje en segundo plano el calculo de tokens y RAG.
- Permitir una demo util sin exigir integraciones completas con otros servicios.
- Garantizar persistencia, trazabilidad y escalabilidad desde el inicio.
- Mantener la opcion de exponer calculos a otras apps sin mover el centro del proyecto.

## Decision

Se define `cost-console` como un producto fullstack analitico cuyo nucleo es un playground de costos para chat y RAG.

La primera version arquitectonica debe cubrir:

- calculo de costo de chat separando tokens de `input`, `output` y contexto reenviado;
- calculo de costo de embeddings para ingestion inicial;
- calculo de costo de consultas RAG, incluyendo embeddings de query, contexto recuperado y llamada final al modelo generativo;
- comparacion de escenarios completos de arquitectura RAG.

El producto nace con:

- frontend `Next.js` con `TypeScript`;
- backend propio para centralizar el motor de calculo;
- `PostgreSQL` como base de datos obligatoria;
- capacidad de persistir catalogos, snapshots, escenarios y reglas.

`cost-console` no depende de `ai-gateway`, `knowledge-rag` ni `auth-service` para demostrar valor en su primera fase.

Las integraciones con otras apps quedan como evolutivos posteriores:

- `auth-service` para exposicion privada o multiusuario;
- `ai-gateway` como consumidor futuro de calculos o recomendaciones;
- `mcp-server` o `openclaw-ops` como adaptadores operativos posteriores.

## Consequences

### Positive

- El proyecto queda alineado con el punto `7` real del learning path.
- La demo inicial puede ser util sin bloquearse por dependencias externas.
- El backend y `PostgreSQL` evitan que la app quede atrapada como calculadora estatica.
- El portafolio gana una herramienta visible para razonar sobre costo de chat, embeddings y RAG antes de implementar mas servicios.

### Negative

- El alcance inicial exige modelar costos tecnicos con mas detalle que un dashboard basico.
- Introducir persistencia desde el inicio agrega complejidad de datos y mantenimiento.
- La nocion de playground puede tentar a meter demasiadas comparaciones o features antes de cerrar bien el motor base.

## Implementation Notes

- Las vistas principales esperadas son `Chat Cost Playground`, `Embeddings Ingestion Calculator`, `Vector Query Cost Playground`, `End-to-End RAG Scenario Builder` y `Pricing Catalog`.
- La logica economica debe vivir en el backend interno; la UI no debe duplicarla.
- Los dominios persistentes minimos son `pricing_catalog`, `pricing_snapshot`, `chat_cost_scenario`, `embedding_ingestion_scenario`, `vector_query_scenario`, `rag_architecture_scenario`, `calculation_result` y `project_scope`.
- Los resultados pueden regenerarse, pero escenarios, snapshots y reglas deben persistirse.
- La integracion con `auth-service` no es requisito del motor de calculo, pero debe respetar aislamiento por proyecto cuando aparezca.

## Related Decisions

- ADR 0002 define el contexto multi-repo donde `cost-console` vive como producto propio.
- ADR 0004 separa retrieval, almacenamiento y tools, lo que ayuda a distinguir entre costo de RAG y source of truth de archivos.
- ADR 0005 deja `ai-gateway` como control-plane de proveedores y evita trasladar esa responsabilidad a `cost-console`.
