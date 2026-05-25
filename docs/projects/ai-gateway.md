# ai-gateway

## Proposito

Backend de orquestacion multi-provider del portafolio. Decide proveedor y modelo segun capacidad, politica, costo y latencia.

## Punto(s) del learning path

- principal: `6`;
- aplica partes de `5` y `7`.

## Tipo de proyecto

- backend de orquestacion.

## Arquetipo frontend si aplica

- no aplica como producto principal.

## Stack recomendado

- `Fastify`
- `TypeScript`
- `Vercel AI SDK`
- `Zod`

## Que guarda

- logs de requests;
- proveedor y modelo elegidos;
- costos asociados o referencias de costo;
- politicas de seleccion y routing;
- referencias a assets usados.

## Que no guarda

- la copia maestra de archivos;
- usuarios maestros;
- corpus RAG como source of truth.

## Con quien habla

- proveedores como OpenAI, Gemini, Claude y eventualmente ElevenLabs;
- `auth-service`;
- `asset/document registry`;
- `knowledge-rag` cuando necesite grounding;
- `cost-console` para pricing y observabilidad.

## Riesgos

- empezar dentro de un frontend y quedar acoplado a ese frontend;
- mezclar decisiones de policy con UI conversacional.

## Estado esperado en el portafolio

Proyecto propio, visible como la pieza principal del punto `6`.
