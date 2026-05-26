# auth-service

## Proposito

Producto base de autenticacion del portafolio. Centraliza identidad, sesiones, alta de usuarios, roles y permisos para apps actuales y futuras.

Su adopcion inicial se enfoca en proyectos publicos o demostrables del portafolio, especialmente `other-gpt` y `cost-console`. La meta temprana no es conectar todo el ecosistema, sino permitir exponer esos proyectos con usuarios y roles separados por proyecto.

`openclaw-ops` no se modela inicialmente como una app con roles normales de usuario final. Opera como superficie administrativa global del portafolio para crear usuarios, cambiar roles, revocar sesiones, banear usuarios y consultar accesos. Esas acciones siguen viviendo en `auth-service`; OpenClaw solo las invoca mediante API, tools o MCP.

## Punto(s) del learning path

- transversal, con enfasis fuerte en `5`.

## Tipo de proyecto

- backend producto base.

## Arquetipo frontend si aplica

- portal de acceso y formularios de alta/login;
- consola administrativa basica en una fase posterior.

## Capacidades operativas iniciales

- levantar una API `Fastify` con `TypeScript`;
- conectar la API a `PostgreSQL`;
- registrar usuarios con email y password;
- iniciar y cerrar sesiones;
- persistir y revocar sesiones;
- crear usuarios para un proyecto especifico;
- promover usuarios a roles superiores por proyecto;
- denegar o revocar acceso por proyecto;
- consultar usuarios, roles y estado de acceso por proyecto;
- auditar acciones administrativas;
- exponer esas acciones para `openclaw-ops` como evolucion inmediata mediante tools o MCP.

## Modelo de seguridad inicial

- Los roles de usuario final son por proyecto.
- Un rol de `other-gpt` no otorga permisos en `cost-console`.
- Un admin de `cost-console` no administra `other-gpt` salvo que tenga una membresia explicita ahi.
- Un usuario puede existir en varios proyectos, pero cada acceso depende de una membresia separada.
- OpenClaw usa una identidad operativa separada, no una cuenta de usuario final comun.
- OpenClaw se autoriza mediante credencial de servicio, token o mecanismo equivalente.
- Cada operacion administrativa de OpenClaw debe indicar `targetProjectId` cuando afecte usuarios, roles o sesiones de un proyecto.
- Toda operacion administrativa debe generar auditoria con actor, operacion, proyecto objetivo, usuario afectado, timestamp y resultado.

## Evolucion MCP/tools

La primera fase termina cuando la API esta levantada, conectada a la base de datos y puede administrar usuarios, sesiones y roles por proyecto.

La segunda fase inmediata expone operaciones administrativas para `openclaw-ops` mediante `mcp-server` o integracion equivalente. Las tools consumen el API de `auth-service`; no escriben directo en la base de datos.

Tools iniciales esperadas:

- `auth.createUser`;
- `auth.assignProjectRole`;
- `auth.revokeProjectAccess`;
- `auth.revokeSession`;
- `auth.banUser`;
- `auth.listProjectUsers`;
- `auth.getUserAccessStatus`.

## Stack recomendado

- `Fastify`
- `TypeScript`
- `Zod`
- `PostgreSQL`
- `Prisma`
- `Pino`
- `Vitest`

## Calidad y tooling

- `TypeScript` en modo estricto.
- `ESLint` con flat config y reglas para TypeScript.
- `Prettier` para formato.
- `Husky` y `lint-staged` para checks antes de commit.
- Scripts minimos: `typecheck`, `lint`, `test`, `build`.

## Hosting inicial

- Base de datos recomendada inicial: `Neon Free`.
- API gratis inicial: `Render Free`, `Koyeb Free` o `Vercel Hobby` para pruebas.
- API pagada recomendada simple: `Render Starter`.
- API pagada barata alternativa: `Fly.io shared-cpu` o `Koyeb micro`.
- `Vercel` queda como opcion serverless si se prioriza integracion con frontends alojados ahi, aceptando que no es el default para una API Fastify tradicional.

## Que guarda

- usuarios;
- credenciales por proyecto o tenant;
- sesiones;
- roles;
- permisos;
- revocaciones;
- auditoria de acceso.

## Que no guarda

- adjuntos del chat;
- corpus RAG;
- pricing de modelos;
- tools MCP;
- mensajes de conversacion.

## Con quien habla

- `other-gpt`
- `cost-console`
- `openclaw-ops`
- `mcp-server` cuando se necesiten tools operativas
- `admin-console` en una fase posterior
- `ai-gateway` de forma posterior o segun necesidad
- apps futuras del portafolio

## Riesgos

- sobrecomplicar el modelo de identidad demasiado pronto;
- mezclar accidentalmente credenciales o roles entre proyectos;
- tratar OpenClaw como backend maestro en vez de superficie operativa;
- permitir operaciones administrativas sin auditoria suficiente;
- bloquear la exposicion de proyectos publicos esperando una consola administrativa completa.

## Estado esperado en el portafolio

Producto base central y reutilizable, no simple infraestructura interna invisible. Debe habilitar primero el acceso controlado a proyectos publicos del portafolio y luego crecer hacia integraciones mas amplias.
