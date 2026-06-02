# auth-service

## Proposito

Producto base de autenticacion del portafolio. Centraliza identidad, sesiones, alta de usuarios, roles y permisos para apps actuales y futuras.

Su adopcion inicial se enfoca en proyectos publicos o demostrables del portafolio, especialmente `other-gpt` y `cost-console`. La meta temprana no es conectar todo el ecosistema, sino permitir exponer esos proyectos con usuarios y roles separados por proyecto.

`openclaw-ops` no se modela inicialmente como una app con roles normales de usuario final. Opera como superficie administrativa global del portafolio para crear usuarios, cambiar roles, revocar sesiones, banear usuarios y consultar accesos. Esas acciones siguen viviendo en `auth-service`; OpenClaw solo las invoca mediante API, tools o MCP, y `auth-service` decide si ejecuta directo o si abre una aprobacion previa.

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
- Las acciones de alto riesgo deben requerir aprobacion separada.
- El solicitante no puede autoaprobar su propia accion.
- La accion aprobada debe revalidarse antes de ejecutarse.

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
- `auth.getUserAccessStatus`;
- `auth.listPendingApprovals`;
- `auth.decideApproval`.

Contrato v1 esperado para mutaciones:

- request comun con `targetProjectId`, `reason`, `idempotencyKey`, `ticketRef?`, `channel` y `payload`;
- response comun con `status`, `operationId`, `approvalId?`, `auditEventId`, `message` y `result`;
- valores minimos de `status`: `completed`, `pending_approval`, `denied`, `failed`.

Politica minima por riesgo:

- lecturas y cambios no administrativos por proyecto pueden ejecutar directo;
- `assignProjectRole` a rol administrativo, `revokeSession` masivo y `banUser` deben crear aprobacion previa;
- `auth-service` decide el riesgo final y puede exigir aprobacion adicional aunque la tool sea la misma.

## Aprobacion y auditoria admin

Para esta familia de tools, `auth-service` debe sostener dos piezas de estado:

- auditoria append-only de eventos admin, con hitos como `requested`, `pending_approval`, `approved`, `rejected`, `completed`, `denied` y `failed`;
- estado vivo de aprobacion para acciones pendientes, con expiracion por defecto de `24h`.

La auditoria debe permitir reconstruir una accion por `operationId`, `correlationId` e `idempotencyKey`, sin guardar secretos o credenciales en snapshots.

Tabla `admin_action_audit` minima esperada:

- `id`;
- `operationId`;
- `eventType`;
- `operationName`;
- `occurredAt`;
- `servicePrincipalId`;
- `operatorUserId`;
- `sourceChannel`;
- `targetProjectId?`;
- `targetUserId?`;
- `targetSessionId?`;
- `approvalId?`;
- `reason`;
- `idempotencyKey`;
- `correlationId`;
- `requestSnapshotJson` redacted;
- `resultSnapshotJson` redacted;
- `policyVersion`;
- `errorCode?`.

Tabla `admin_approval` minima esperada:

- `id`;
- `operationId`;
- `status`;
- `requestedAt`;
- `requestedByUserId`;
- `requiredApprovalLevel`;
- `approvedByUserId?`;
- `decidedAt?`;
- `decisionReason?`;
- `expiresAt`.

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
- permitir aprobaciones sin separacion real entre solicitante y aprobador;
- bloquear la exposicion de proyectos publicos esperando una consola administrativa completa.

## Estado esperado en el portafolio

Producto base central y reutilizable, no simple infraestructura interna invisible. Debe habilitar primero el acceso controlado a proyectos publicos del portafolio y luego crecer hacia integraciones mas amplias.

## Estado de implementacion (Identity-Service)

El repo que implementa `auth-service` se llama `Identity-Service`. Esta seccion
registra el progreso real frente a la vision de este documento; no la reescribe.

### Implementado hoy

- Stack: `Fastify` + `TypeScript` + `Prisma` + `PostgreSQL` + `Zod` + `Vitest`.
- Autenticacion por proyecto, basada en cookie de sesion, bajo
  `/projects/:slug/auth/*`: `register/email-check`, `register`, `login`,
  `logout`, `me`, `session`.
- Registro en dos pasos (email-check y luego alta), con login que auto-crea una
  membresia base `ACTIVE` con rol `user` en el primer acceso a un proyecto.
- Validador de sesion liviano `GET /projects/:slug/auth/session` (`204` valida,
  `401` no usable) para middleware de clientes.
- Administracion de membresias por project admin (cookie):
  `GET /projects/:slug/me`, `memberships` (listado paginado), `audit-logs`,
  crear, suspender, reactivar, revocar, y reemplazo de roles.
- Gestion de sesiones por project admin: `GET /projects/:slug/sessions` y
  `POST /projects/:slug/sessions/:sessionId/revoke`.
- Auditoria de mutaciones de membresia (read-only, inmutable) y proteccion del
  ultimo admin activo del proyecto.
- Seeds: `other-gpt` (roles `user`, `pro`, `admin`) y `cost-console` (roles
  `user`, `admin`). Bootstrap del primer admin por script (`npm run db:bootstrap-admin`).
- Fundacion de la superficie admin MCP (ADR 0008): identidad de servicio (machine
  auth) por bearer token con alcance least-privilege (flag `allProjects` para
  `openclaw-ops` o allow-list explicito de proyectos, sin re-login por proyecto),
  esquema `ServicePrincipal`/`AdminOperation`/`admin_action_audit`/`admin_approval`,
  y script de bootstrap/rotacion (`npm run db:bootstrap-service-principal`).
- Superficie `/admin/*` (camino directo) autenticada solo por service principal,
  con el envelope comun de mutacion/response, replay idempotente por
  `(servicePrincipalId, idempotencyKey)`, auditoria append-only, y los estados
  `completed`/`denied`/`failed`. Operaciones: lecturas `listProjectUsers`,
  `getUserAccessStatus`, `listPendingApprovals`, y mutaciones de bajo riesgo
  `auth.createUser` y `auth.unbanUser`.
- Ciclo de aprobacion por riesgo: las operaciones de alto riesgo registran un
  `AdminOperation` en `PENDING_APPROVAL` + `admin_approval` (expiracion 24h) en
  vez de ejecutar; `auth.decideApproval` confirma (approve) o cancela (reject) la
  accion como un segundo paso deliberado, respeta la expiracion y ejecuta el
  efecto diferido al aprobar.
- Familia completa de operaciones (ADR 0008/0009): lecturas; mutaciones directas
  `createUser`, `unbanUser`, `revokeProjectAccess`, `revokeSession` individual; y
  de alto riesgo (gated) `banUser`, `revokeSession` masivo, `assignProjectRole` a
  rol `admin` y `readmitProjectMembership`. `assignProjectRole` es directo para
  roles no-admin. Reusa los invariantes de membresia (proteccion del ultimo admin
  activo).

### Estado

La vision MCP/admin de este documento y de ADR 0008/0009 esta **implementada** en
`Identity-Service`. Ademas, los project admins (via cookie) tienen visibilidad
read-only de las operaciones de maquina que afectan su proyecto mediante
`GET /projects/:slug/admin-operations`. La clasificacion de riesgo esta
centralizada en una policy unica (`classifyOperationRisk`, default-to-safe) y se
persiste un `policyVersion` en cada operacion. La retencion del audit admin es un
script local de mantenimiento (`npm run db:prune-admin-operations`, con
`--dry-run`/`--export`) que poda solo operaciones terminales antiguas, nunca las
`PENDING_APPROVAL`. Unico pendiente (diferido): reintroducir la regla de dos
operadores si se conecta una segunda identidad operativa.

### Guias de integracion

El repo `Identity-Service` publica dos guias que se comparten con los proyectos
consumidores (viven en `docs/` de ese repo, como fuente de verdad):

- guia de apps de usuario (cookie/sesion) para front-ends como `other-gpt` y
  `cost-console`;
- guia admin/MCP para `mcp-server`, operadores y los comandos admin (incluido el
  prune del audit).

### Divergencias de contrato a tener en cuenta para el ecosistema

- La autorizacion es estrictamente por proyecto: no existe rol admin global; un
  admin solo opera dentro de su proyecto.
- La auditoria existente es `ProjectMembershipAuditLog` (alcance membresia, sin
  `reason`/`operationId`/`correlationId`), distinta de la `admin_action_audit`
  planeada para la superficie MCP.
- El contrato de sesion real es cookie httpOnly (`identity_service_session`,
  `sameSite=lax`, TTL `24h`), util para `other-gpt` que delega autenticacion: el
  cliente valida con `GET /projects/:slug/auth/session` y no necesita leer el token.
- Las sesiones son por proyecto: una sesion de un proyecto no autentica a otro.
- Divergencia con ADR 0008 de platform: la aprobacion de alto riesgo se
  implemento como un **guard de confirmacion en dos pasos** (el mismo operador
  puede confirmar su propia solicitud), no como la regla "el solicitante no puede
  autoaprobar". Decision deliberada porque el portafolio corre un solo bot de
  `openclaw-ops`; el valor es exigir un segundo paso deliberado antes de un cambio
  sensible, no separar dos personas. Si mas adelante se conecta una segunda
  identidad operativa, conviene reintroducir la regla de dos operadores.
- Contrato de idempotencia: el `idempotencyKey` es unico por
  `(servicePrincipalId, idempotencyKey)` y esta acotado a una sola operacion
  logica. Reintentar replica el resultado de `completed`/`pending_approval`/
  `denied`, pero un `failed` (sin efecto aplicado) se re-ejecuta al reintentar;
  reutilizarlo para otra operacion devuelve `409 ADMIN_IDEMPOTENCY_KEY_REUSED`.
  Los clientes (`mcp-server`) deben usar una clave unica por request.
