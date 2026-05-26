# openclaw-ops

## Proposito

Usar OpenClaw como interfaz operativa para consumir skills, tools y canales como Telegram dentro del portafolio.

Tambien puede servir como primera superficie practica para administrar accesos del portafolio, consumiendo `auth-service` mediante API, tools o MCP.

Para `auth-service`, OpenClaw se trata inicialmente como una superficie administrativa global, no como una app con roles normales de usuario final. Puede operar usuarios, roles y sesiones del portafolio, pero no guarda identidad maestra ni duplica reglas de permisos.

## Punto(s) del learning path

- principal: `1`;
- consume partes de `3` y `7`.

## Tipo de proyecto

- interfaz operativa.

## Arquetipo frontend

- control surface operativo;
- canal de Telegram;
- consumo de skills y MCPs;
- operaciones administrativas de usuarios, roles y sesiones delegadas a `auth-service`.

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

- corpus RAG maestro;
- identidad maestra del portafolio;
- reglas canonicas de permisos;
- mensajes historicos del chat como source of truth.

## Operaciones de auth esperadas

- crear usuarios para proyectos publicos;
- cambiar roles de usuarios por proyecto;
- denegar o revocar acceso por proyecto;
- revocar sesiones;
- banear usuarios;
- consultar estado de usuarios y permisos;
- ejecutar estas acciones mediante tools o MCP cuando exista la segunda fase de integracion.

Cada operacion administrativa debe ser autorizada por `auth-service` y quedar auditada ahi con actor, operacion, proyecto objetivo, usuario afectado, timestamp y resultado.

## Con quien habla

- `mcp-server`
- `auth-service`
- `cost-console`
- eventualmente `ai-gateway`

## Riesgos

- intentar convertir OpenClaw en backend maestro del portafolio;
- mezclar operacion con identidad central;
- duplicar logica de permisos que debe vivir en `auth-service`.
- operar acciones admin sin trazabilidad suficiente en `auth-service`.

## Estado esperado en el portafolio

Proyecto visible del punto `1`, orientado a operaciones y consumo de capabilities existentes. En la primera exposicion publica, debe poder operar auth sin reemplazar a `auth-service`.
