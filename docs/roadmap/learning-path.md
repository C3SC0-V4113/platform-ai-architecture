# Learning Path Base

Este documento limpia y fija los puntos base del learning path que gobierna el portafolio. Todo el plan del portafolio existe para cubrir estos puntos de forma explicita, visible y trazable.

El orden de los puntos no representa prioridad de entrega ni dificultad. Todas las habilidades son importantes para el portafolio; la secuencia practica de implementacion puede variar segun la necesidad de demostrar resultados, exponer proyectos publicos o aclarar casos de uso.

## Punto 1: OpenClaw

- Nombre corto: `OpenClaw Ops`
- Objetivo: usar OpenClaw como interfaz operativa para skills, tools y canales como Telegram.
- Capability: interfaz de operacion y consumo de herramientas.
- Proyecto principal: `openclaw-ops`.
- Consumidores o soporte: `mcp-server`, `cost-console`, `auth-service`.

## Punto 2: RAG

- Nombre corto: `Knowledge RAG`
- Objetivo: indexar y recuperar conocimiento con citas sobre un corpus externo curado.
- Capability: ingestion, embeddings, chunking, retrieval, grounding.
- Proyecto principal: `knowledge-rag`.
- Consumidores o soporte: `Google Drive` como fuente externa; integracion futura opcional con el ecosistema central.

## Punto 3: MCP

- Nombre corto: `MCP Tools`
- Objetivo: exponer tools reutilizables para apps y agentes.
- Capability: capa estandar de herramientas y contratos de tool use.
- Proyecto principal: `mcp-server`.
- Consumidores o soporte: `other-gpt`, `openclaw-ops`, `auth-service`.

## Punto 4: Frontend

- Nombre corto: `Conversational Frontend`
- Objetivo: ofrecer una experiencia conversacional multimodal usable por personas.
- Capability: chat, streaming, adjuntos, UX de interaccion, futuro voice UX.
- Proyecto principal: `other-gpt`.
- Consumidores o soporte: `auth-service`, `ai-gateway`, `asset/document registry`.

## Punto 5: Seguridad

- Nombre corto: `Security and Governance`
- Objetivo: aplicar control de acceso, scopes, aislamiento, aprobacion operativa, auditoria y politicas de uso.
- Capability: seguridad transversal y gobierno operativo.
- Proyecto principal: transversal.
- Consumidores o soporte: todos los proyectos.

## Punto 6: Orquestacion de modelos

- Nombre corto: `AI Gateway`
- Objetivo: elegir proveedor y modelo segun capacidad, costo, politica, latencia y preferencias explicitas.
- Capability: orquestacion multi-provider y control-plane de proveedores.
- Proyecto principal: `ai-gateway`.
- Consumidores o soporte: `other-gpt`, `cost-console`, `mcp-server`.

## Punto 7: Costos

- Nombre corto: `Cost Intelligence`
- Objetivo: estimar cuanto costaria un chat, una carga de embeddings y una consulta a base vectorial, separando tokens, contexto y etapas del pipeline.
- Capability: token accounting, calculadora de embeddings, costo de retrieval y simulacion economica de arquitecturas RAG.
- Proyecto principal: `cost-console`.
- Consumidores o soporte: `ai-gateway`, `other-gpt`, `openclaw-ops`.
- Cobertura minima esperada:
  - calcular tokens de `input`, `output` y contexto enviados en cada llamada de chat;
  - estimar el costo de una sesion o flujo conversacional, no solo de un request aislado;
  - calcular los tokens y el costo de embeddings durante la carga inicial hacia una base vectorial;
  - calcular los tokens y el costo de embeddings y contexto en cada consulta a la base vectorial.

## Punto 8: Voz

- Nombre corto: `Voice UX`
- Objetivo: agregar conversacion por voz como capacidad de experiencia.
- Capability: STT, TTS y luego realtime voice, con proveedor configurable via `ai-gateway`.
- Proyecto principal: `other-gpt`.
- Consumidores o soporte: `ai-gateway`.

## Reglas de cobertura

- Cada punto debe tener al menos un proyecto principal visible en el portafolio.
- Un proyecto puede consumir un punto sin ser el proyecto principal de ese punto.
- Los puntos `5` y `6` se consideran fuertemente transversales aunque tengan proyectos mas obvios.
- `other-gpt` no es el centro del portafolio; es el satelite principal del punto `4` y entrada futura a `8`.
- La cobertura del learning path no obliga a implementar todos los puntos al mismo tiempo ni en el orden numerico del documento.
