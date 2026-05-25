# openclaw-ops

## Proposito

Usar OpenClaw como interfaz operativa para consumir skills, tools y canales como Telegram dentro del portafolio.

## Punto(s) del learning path

- principal: `1`;
- consume partes de `3` y `7`.

## Tipo de proyecto

- interfaz operativa.

## Arquetipo frontend

- control surface operativo;
- canal de Telegram;
- consumo de skills y MCPs.

## Stack recomendado

- `OpenClaw Gateway`
- skills
- tools
- canales

## Que guarda

- configuracion del gateway;
- conexiones de canales;
- sesiones operativas de OpenClaw;
- configuracion de skills/plugins.

## Que no guarda

- identidad maestra del portafolio;
- corpus RAG maestro;
- mensajes historicos del chat como source of truth.

## Con quien habla

- `mcp-server`
- `auth-service`
- `cost-console`
- eventualmente `ai-gateway`

## Riesgos

- intentar convertir OpenClaw en backend maestro del portafolio;
- mezclar operacion con identidad central.

## Estado esperado en el portafolio

Proyecto visible del punto `1`, orientado a operaciones y consumo de capabilities existentes.
