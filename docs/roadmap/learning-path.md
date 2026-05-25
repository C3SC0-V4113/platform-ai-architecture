# Learning Path Base

Este documento limpia y fija los puntos base del learning path que gobierna el portafolio. Todo el plan del portafolio existe para cubrir estos puntos de forma explicita, visible y trazable.

## Punto 1: OpenClaw

- Nombre corto: `OpenClaw Ops`
- Objetivo: usar OpenClaw como interfaz operativa para skills, tools y canales como Telegram.
- Capability: interfaz de operacion y consumo de herramientas.
- Proyecto principal: `openclaw-ops`.
- Consumidores o soporte: `mcp-server`, `cost-console`, `auth-service`.

## Punto 2: RAG

- Nombre corto: `Knowledge RAG`
- Objetivo: indexar y recuperar conocimiento con citas y permisos.
- Capability: ingestion, embeddings, chunking, retrieval, grounding.
- Proyecto principal: `knowledge-rag`.
- Consumidores o soporte: `other-gpt`, `mcp-server`, `ai-gateway`.

## Punto 3: MCP

- Nombre corto: `MCP Tools`
- Objetivo: exponer tools reutilizables para apps y agentes.
- Capability: capa estandar de herramientas y contratos de tool use.
- Proyecto principal: `mcp-server`.
- Consumidores o soporte: `other-gpt`, `openclaw-ops`.

## Punto 4: Frontend

- Nombre corto: `Conversational Frontend`
- Objetivo: ofrecer una experiencia conversacional multimodal usable por personas.
- Capability: chat, streaming, adjuntos, UX de interaccion, futuro voice UX.
- Proyecto principal: `other-gpt`.
- Consumidores o soporte: `auth-service`, `ai-gateway`, `asset/document registry`.

## Punto 5: Seguridad

- Nombre corto: `Security and Governance`
- Objetivo: aplicar control de acceso, scopes, aislamiento, limpieza de datos y politicas de uso.
- Capability: seguridad transversal.
- Proyecto principal: transversal.
- Consumidores o soporte: todos los proyectos.

## Punto 6: Orquestacion de modelos

- Nombre corto: `AI Gateway`
- Objetivo: elegir proveedor y modelo segun capacidad, costo, politica y latencia.
- Capability: orquestacion multi-provider.
- Proyecto principal: `ai-gateway`.
- Consumidores o soporte: `other-gpt`, `cost-console`, `mcp-server`.

## Punto 7: Costos

- Nombre corto: `Cost Intelligence`
- Objetivo: medir consumo, pricing, presupuestos y recomendacion de modelo.
- Capability: observabilidad y decision economica.
- Proyecto principal: `cost-console`.
- Consumidores o soporte: `ai-gateway`, `other-gpt`, `openclaw-ops`.

## Punto 8: Voz

- Nombre corto: `Voice UX`
- Objetivo: agregar conversacion por voz como capacidad de experiencia.
- Capability: STT, TTS y luego realtime voice.
- Proyecto principal: `other-gpt`.
- Consumidores o soporte: `ai-gateway`.

## Reglas de cobertura

- Cada punto debe tener al menos un proyecto principal visible en el portafolio.
- Un proyecto puede consumir un punto sin ser el proyecto principal de ese punto.
- Los puntos `5` y `6` se consideran fuertemente transversales aunque tengan proyectos mas obvios.
- `other-gpt` no es el centro del portafolio; es el satelite principal del punto `4` y entrada futura a `8`.
