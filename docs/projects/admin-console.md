# admin-console

## Proposito

Panel administrativo del portafolio para usuarios, sesiones, permisos, apps y presupuestos.

Es la ultima prioridad practica del portafolio. No debe bloquear la exposicion temprana de `other-gpt`, `cost-console` u `openclaw-ops`.

## Punto(s) del learning path

- soporte transversal a `5` y `7`.

## Tipo de proyecto

- frontend administrativo.

## Arquetipo frontend

- dashboard CRUD con formularios, tablas, auditoria y permisos.

## Secuencia esperada

- primero deben existir las capacidades administrativas reales en `auth-service`;
- luego deben poder operarse desde `openclaw-ops` mediante tools o MCP;
- finalmente `admin-console` puede ofrecer una interfaz visual completa sobre esas mismas capacidades.

## Stack recomendado

- `Next.js`
- `TypeScript`

## Que guarda

- idealmente no es source of truth; consume datos desde `auth-service` y otros servicios.

## Que no guarda

- identidad maestra por si mismo;
- corpus RAG;
- assets can?nicos.

## Con quien habla

- `auth-service`
- `cost-console`
- `openclaw-ops` opcionalmente

## Riesgos

- duplicar logica que deberia vivir en auth-service;
- crecer sin necesidad si Telegram/OpenClaw cubren operacion inicial.
- consumir tiempo antes de demostrar recursos publicos del portafolio.

## Estado esperado en el portafolio

Proyecto opcional posterior para administracion visual del ecosistema. Debe llegar despues de validar `auth-service` y la operacion inicial desde `openclaw-ops`.
