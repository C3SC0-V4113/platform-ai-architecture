# ai-gateway

## Proposito

Backend de orquestacion multi-provider del portafolio. Decide proveedor y modelo segun capacidad, politica, costo y latencia.

Sigue siendo una habilidad importante, pero su implementacion completa puede quedar secundaria mientras `other-gpt`, `cost-console`, `openclaw-ops` y `auth-service` habilitan una exposicion publica inicial.

Tambien actuara como control-plane de proveedores para capacidades multimodales: chat, imagenes, TTS, STT y llamadas realtime.

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
- referencias a assets usados;
- configuracion de adaptadores por proveedor y capacidad;
- metadata de sesiones realtime cuando aplique.

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

## Reglas de seleccion de proveedor

- Si la preferencia llega como `auto`, `ai-gateway` decide proveedor y modelo segun politica, costo, capacidad, latencia y disponibilidad.
- Si la preferencia llega con proveedor explicito, esa decision tiene precedencia sobre la seleccion automatica.
- Si un proveedor explicito falla, `ai-gateway` debe responder un error accionable y no hacer fallback silencioso.
- Si `auto` falla con una opcion, `ai-gateway` puede intentar otra opcion compatible segun politica.

## Adaptadores esperados

- chat y streaming de texto;
- generacion de imagenes;
- text-to-speech;
- speech-to-text;
- llamadas realtime.

Para realtime, `ai-gateway` puede crear sesiones, tokens, signed URLs, instrucciones y metadata sin relayar todo el audio. La conexion de baja latencia puede quedar entre `other-gpt` y el proveedor cuando el protocolo lo requiera.

## Riesgos

- empezar dentro de un frontend y quedar acoplado a ese frontend;
- mezclar decisiones de policy con UI conversacional.
- retrasar demos publicas esperando una capa completa de orquestacion multi-provider.
- ignorar una preferencia explicita de proveedor y generar resultados dificiles de explicar.

## Estado esperado en el portafolio

Proyecto propio, visible como la pieza principal del punto `6`. Puede iniciar con integraciones parciales o posteriores, sin cambiar su importancia dentro del learning path. A futuro debe conectar de forma uniforme las capacidades multimodales de `other-gpt` con proveedores como OpenAI, ElevenLabs u otros.
