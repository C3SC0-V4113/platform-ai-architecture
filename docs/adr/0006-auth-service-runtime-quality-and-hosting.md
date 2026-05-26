# ADR 0006: Auth Service Runtime, Quality and Hosting

- Date: 2026-05-26
- Status: Accepted

## Context

`auth-service` sera el centro de autenticacion para demos del portafolio. Debe habilitar proyectos como `other-gpt` y `cost-console` con usuarios, sesiones y roles separados por proyecto, sin convertir cada demo en una implementacion aislada de auth.

El proyecto debe ser implementable por alguien que conoce TypeScript de forma inicial y tiene experiencia conceptual con clean architecture en C#, pero no necesariamente con Fastify, linters o hosting de backends Node.

Tambien se espera que `openclaw-ops` funcione como superficie administrativa inicial para operar usuarios, roles, sesiones y bloqueos. Esa superficie debe delegar en `auth-service` y no convertirse en backend maestro.

## Decision Drivers

- Mantener una arquitectura comprensible y confiable para un backend TypeScript.
- Detectar errores comunes temprano con tooling de calidad.
- Mantener costo inicial lo mas bajo posible.
- Separar roles de usuario final por proyecto.
- Permitir operacion administrativa desde OpenClaw sin duplicar logica de permisos.
- Dejar una ruta clara hacia MCP/tools despues del primer objetivo: API levantada y conectada a base de datos.

## Decision

Se implementara `auth-service` con:

- `Fastify` como framework HTTP.
- `TypeScript` en modo estricto.
- `Zod` para validacion de entrada y contratos.
- `PostgreSQL` como base de datos.
- `Prisma` como ORM y herramienta de migraciones.
- `Pino` para logs estructurados.
- `Vitest` para pruebas unitarias e integracion HTTP.

El ecosistema de calidad minimo sera:

- `ESLint` con flat config y reglas para TypeScript.
- `Prettier` para formato.
- `Husky` y `lint-staged` para checks antes de commit.
- scripts `typecheck`, `lint`, `test` y `build`.

El hosting inicial sera:

- base de datos en `Neon Free`;
- API en un hosting gratuito para aprendizaje y pruebas, como `Render Free`, `Koyeb Free` o `Vercel Hobby`;
- primera opcion pagada simple: `Render Starter`;
- opciones pagadas baratas alternativas: `Fly.io shared-cpu` o `Koyeb micro`;
- `Vercel` se mantiene como opcion serverless cuando se priorice integracion con frontends alojados ahi, pero no es el default para una API Fastify tradicional.

OpenClaw se modela como cliente operativo/admin global inicial:

- no es una app con roles normales de usuario final;
- usa una identidad operativa separada;
- se autoriza mediante credencial de servicio, token o mecanismo equivalente;
- sus acciones administrativas se ejecutan mediante API, tools o MCP;
- sus acciones deben quedar auditadas por `auth-service`.

Los roles de usuarios finales son por proyecto:

- un rol de `other-gpt` no otorga permisos en `cost-console`;
- un admin de `cost-console` no administra `other-gpt` salvo que tenga membresia explicita ahi;
- cada operacion que afecte usuarios o roles de proyecto debe indicar el proyecto objetivo.

## Consequences

### Positive

- El backend queda alineado con una arquitectura TypeScript comun y mantenible.
- El tooling ayuda a detectar errores de tipos, async, formato y tests antes de desplegar.
- El costo inicial puede mantenerse en cero o muy bajo.
- Las demos pueden compartir centro de autenticacion sin compartir roles accidentalmente.
- OpenClaw puede operar auth temprano sin reemplazar a `auth-service`.

### Negative

- Fastify, ESLint, Prisma y hosting Node agregan curva de aprendizaje.
- Los planes gratuitos tienen limites, sleeps o condiciones que no son ideales para demos publicas estables.
- OpenClaw como admin global simplifica la fase inicial, pero concentra poder operativo.
- Vercel requiere aceptar modelo serverless si se usa para este backend.

## Implementation Notes

- La primera fase termina cuando la API esta levantada, conectada a PostgreSQL y cubre login, logout, sesiones, roles por proyecto y revocacion.
- La fase inmediata posterior expone tools para OpenClaw via `mcp-server` o integracion equivalente.
- Las tools de auth deben llamar al API de `auth-service`; no deben escribir directo a PostgreSQL.
- Las tools iniciales esperadas son `auth.createUser`, `auth.assignProjectRole`, `auth.revokeProjectAccess`, `auth.revokeSession`, `auth.banUser`, `auth.listProjectUsers` y `auth.getUserAccessStatus`.
- Cada accion administrativa debe registrar actor, operacion, proyecto objetivo, usuario afectado, timestamp y resultado.

## Related Decisions

- ADR 0002 define el enfoque multi-repo donde `auth-service` vive como producto separado.
- ADR 0003 define `auth-service` como centro de autenticacion con aislamiento por proyecto.
- ADR 0004 depende de `auth-service` para aislamiento de persistencia por proyecto.
- ADR 0005 mantiene `ai-gateway` separado del control de identidad.

## References

- [Fastify TypeScript](https://fastify.dev/docs/latest/Reference/TypeScript/)
- [typescript-eslint Getting Started](https://typescript-eslint.io/getting-started/)
- [Neon Pricing](https://neon.com/pricing)
- [Render Pricing](https://render.com/pricing)
- [Render Free Plan](https://render.com/docs/free)
- [Vercel Pricing](https://vercel.com/pricing)
- [Vercel Functions Limits](https://vercel.com/docs/functions/limitations)
- [Fly.io Pricing](https://flyio-landing.fly.dev/docs/about/pricing/)
- [Koyeb Instances](https://www.koyeb.com/docs/reference/instances)
