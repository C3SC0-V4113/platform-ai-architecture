# admin-console

## Proposito

Panel administrativo del portafolio para usuarios, sesiones, permisos, apps y presupuestos.

## Punto(s) del learning path

- soporte transversal a `5` y `7`.

## Tipo de proyecto

- frontend administrativo.

## Arquetipo frontend

- dashboard CRUD con formularios, tablas, auditoria y permisos.

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

## Estado esperado en el portafolio

Proyecto opcional posterior para administracion visual del ecosistema.
