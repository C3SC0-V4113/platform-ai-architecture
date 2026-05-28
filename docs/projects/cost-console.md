# cost-console

## Proposito

Playground fullstack de costos del portafolio.

Su trabajo principal es estimar costos de chat, embeddings y consultas con base vectorial, separando tokens de `input`, `output`, contexto arrastrado y etapas del pipeline RAG.

Puede exponerse temprano como producto demostrable por si mismo. No depende de otras apps del portafolio para ser util, aunque mas adelante pueda integrarse con `auth-service`, `ai-gateway` u otros consumidores.

## Punto(s) del learning path

- principal: `7`;
- aplica partes de `6`.

## Tipo de proyecto

- producto fullstack analitico.

## Arquetipo frontend

- playground tecnico con calculadoras, comparativas, escenarios guardados y explicacion paso a paso del costo.

## Stack recomendado

- `Next.js`
- `TypeScript`
- `PostgreSQL`
- `Prisma`

## Nucleo funcional

El proyecto debe cubrir de forma visible y explicita estas capacidades:

- calcular costo de chat separando:
  - tokens de `input`;
  - tokens de `output`;
  - tokens de contexto reenviado en cada llamada;
- calcular costo de embeddings para cargas iniciales hacia una base vectorial;
- calcular costo recurrente de consultas RAG, incluyendo:
  - embeddings de la consulta;
  - contexto recuperado;
  - costo posterior de la llamada al modelo generativo;
- comparar escenarios completos de arquitectura RAG antes de implementar otros servicios del portafolio.

## Backend interno

`cost-console` no debe quedar como calculadora estatica.

Desde la primera fase debe incluir backend propio para:

- centralizar el motor de calculo;
- exponer una API interna para escenarios, catalogos y resultados;
- guardar configuraciones, snapshots y reglas;
- permitir una futura extraccion si el dominio de costos crece.

La UI no debe duplicar logica economica que luego se vuelva dificil de mantener.

## Persistencia esperada

`PostgreSQL` es obligatorio desde el inicio para mantener el proyecto escalable y trazable.

Dominios persistentes minimos esperados:

- `pricing_catalog`;
- `pricing_snapshot`;
- `chat_cost_scenario`;
- `embedding_ingestion_scenario`;
- `vector_query_scenario`;
- `rag_architecture_scenario`;
- `calculation_result`;
- `recommendation_rule` si luego aparece esa capa;
- `project_scope` para futura integracion con auth.

Los resultados pueden regenerarse, pero los escenarios, snapshots y reglas deben persistirse.

## Vistas principales

### Chat Cost Playground

Vista principal para estimar costo de un request o una sesion conversacional.

Componentes principales:

- selector de proveedor y modelo;
- editor de mensajes:
  - `system`;
  - historial;
  - mensaje actual;
  - contexto adicional;
- panel de conteo de tokens:
  - `input tokens`;
  - `output tokens`;
  - `context tokens`;
  - total por request;
- panel de costo:
  - costo por llamada;
  - costo por sesion;
  - costo por multiples interacciones;
- comparador entre modelos.

Funcionalidades esperadas:

- recalculo en tiempo real;
- guardar escenarios;
- duplicar escenarios para comparacion;
- explicar como el historial incrementa el costo en cada turno.

### Embeddings Ingestion Calculator

Vista para estimar el costo de vectorizar una carga inicial de documentos.

Componentes principales:

- formulario de volumen documental;
- configuracion de chunking:
  - `chunk size`;
  - `overlap`;
  - limpieza estimada;
- selector de modelo de embeddings;
- resumen derivado:
  - documentos;
  - chunks;
  - tokens a vectorizar;
  - requests esperados;
- resultado de costo total y unitario.

Funcionalidades esperadas:

- modelar varios esquemas de chunking;
- guardar configuraciones de ingest;
- comparar costo entre modelos de embeddings;
- persistir presets para distintos casos de carga inicial.

### Vector Query Cost Playground

Vista para estimar el costo recurrente de consultas RAG.

Componentes principales:

- formulario de consulta:
  - tamano promedio del query;
  - frecuencia de consultas;
  - `top-k`;
  - tamano promedio de chunks recuperados;
- diagrama del pipeline:
  - embedding de query;
  - retrieval;
  - contexto reenviado al LLM;
- breakdown por etapa:
  - costo del embedding de consulta;
  - tokens del contexto recuperado;
  - costo de la respuesta final;
- proyeccion temporal:
  - costo por consulta;
  - costo diario;
  - costo mensual.

Funcionalidades esperadas:

- separar costo de retrieval y costo de generacion;
- variar `top-k` y ver impacto;
- persistir escenarios de consulta;
- comparar una estrategia RAG mas densa contra una mas economica.

### End-to-End RAG Scenario Builder

Vista para simular el costo completo de una arquitectura RAG.

Componentes principales:

- constructor de escenario:
  - volumen de ingestion;
  - chunking;
  - modelo de embeddings;
  - patron de consultas;
  - modelo generativo final;
- resumen de ciclo de vida:
  - costo inicial;
  - costo por consulta;
  - costo mensual esperado;
- comparador de arquitecturas;
- panel de tradeoffs.

Funcionalidades esperadas:

- comparar estrategias completas;
- guardar arquitecturas como escenarios nombrados;
- reutilizar pricing persistido;
- mostrar donde esta el costo dominante.

### Pricing Catalog

Vista de administracion del pricing usado por el motor de calculo.

Componentes principales:

- tabla de precios por proveedor, modelo y capacidad;
- snapshot o versionado;
- estado de validez;
- editor de registros de pricing.

Funcionalidades esperadas:

- crear y editar pricing;
- versionar cambios;
- recalcular escenarios pasados con pricing historico;
- dejar trazabilidad de que precios alimentaron cada simulacion.

## Que guarda

- pricing por proveedor, modelo y capacidad;
- snapshots historicos de pricing;
- escenarios guardados de chat, embeddings y RAG;
- resultados de calculo regenerables;
- configuraciones de chunking, retrieval y comparacion;
- reglas futuras de recomendacion o scoring;
- auditoria de cambios sobre pricing y reglas.

## Que no guarda

- identidad maestra;
- assets canonicos;
- corpus RAG.

## Con quien habla

El proyecto puede funcionar sin hablar con otras apps del portafolio.

Integraciones secundarias o posteriores:

- `auth-service` para exposicion privada o multiusuario;
- `ai-gateway` como consumidor futuro de calculos o recomendaciones;
- `openclaw-ops` como consumidor operativo posterior;
- `other-gpt` de forma parcial si necesita comparar estrategias de costo.

## Riesgos

- reducirlo a una calculadora estatica sin persistencia ni trazabilidad;
- convertirlo en gateway o mezclarlo con routing de inferencia;
- depender demasiado pronto de integraciones externas para demostrar valor;
- mezclar costo de retrieval con costo de almacenamiento y perder claridad conceptual.

## Estado esperado en el portafolio

Proyecto visible del punto `7`, util por si mismo como playground tecnico para calcular costo de chat y RAG.

Debe poder demostrar valor sin `ai-gateway`, `knowledge-rag` o `auth-service`, pero nacer con backend propio y `PostgreSQL` para no quedar atrapado como demo sin capacidad de crecer.
