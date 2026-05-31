# ADR 0008: Operate Auth Admin via MCP with Risk-Based Approval

- Date: 2026-05-29
- Status: Accepted

## Context

ADR 0006 ya fijo que `openclaw-ops` seria la primera superficie administrativa para `auth-service`, y que la fase inmediata posterior expondria tools via `mcp-server`. Faltaba cerrar tres cosas para esa segunda fase:

- el contrato minimo comun de las tools mutantes;
- que acciones ejecutan directo y cuales deben pasar por aprobacion;
- donde vive la auditoria y el estado de aprobacion.

Sin esa definicion, `mcp-server` podria crecer como un tunel privilegiado ambiguo o `openclaw-ops` podria terminar asumiendo reglas que pertenecen a `auth-service`.

## Decision Drivers

- Mantener a `mcp-server` como fachada operativa y no como backend maestro de auth.
- Mantener a `auth-service` como punto unico de autorizacion, aprobacion y auditoria.
- Permitir operacion temprana desde OpenClaw sin abrir acciones sensibles sin friccion.
- Evitar doble ejecucion por reintentos de chat, canal o tool.
- Dejar una familia de tools discreta y entendible para la v1.

## Decision

La operacion administrativa de auth via MCP se implementa asi:

1. `mcp-server` expone una familia de tools discretas:
   - `auth.createUser`
   - `auth.assignProjectRole`
   - `auth.revokeProjectAccess`
   - `auth.revokeSession`
   - `auth.banUser`
   - `auth.listProjectUsers`
   - `auth.getUserAccessStatus`
   - `auth.listPendingApprovals`
   - `auth.decideApproval`

2. Las tools mutantes usan un contrato comun con:
   - `targetProjectId`
   - `reason`
   - `idempotencyKey`
   - `ticketRef?`
   - `channel`
   - `payload`

3. `mcp-server` valida schema MCP, propaga identidad operativa, `correlationId` e `idempotencyKey`, y delega la decision canonica a `auth-service`.

4. `auth-service` decide riesgo, autorizacion y resultado final:
   - lecturas ejecutan directo;
   - mutaciones de bajo riesgo pueden ejecutar directo;
   - mutaciones de alto riesgo crean una aprobacion pendiente y no aplican side effects hasta decision posterior.

5. `auth-service` mantiene dos registros de negocio:
   - auditoria append-only de eventos admin;
   - estado vivo de aprobaciones pendientes o resueltas.

6. El solicitante no puede autoaprobar. La aprobacion expira por defecto en `24h` y la accion se revalida antes de ejecutarse.

## Consequences

### Positive

- OpenClaw puede operar auth temprano sin absorber reglas canonicas de permisos.
- `mcp-server` queda con responsabilidades acotadas: contrato, routing, identidad operativa y resultado.
- Las acciones sensibles ganan separacion entre solicitud y aprobacion.
- La idempotencia reduce efectos duplicados por reintentos de canal o chat.
- La auditoria permite reconstruir cada accion por `operationId` y `correlationId`.

### Negative

- La experiencia operativa tiene mas friccion para acciones sensibles.
- `auth-service` debe modelar politicas de riesgo ademas de permisos.
- Aparece una pieza extra de estado: aprobaciones pendientes con expiracion.

## Implementation Notes

- Las tools de auth no escriben directo en PostgreSQL ni exponen queries arbitrarias.
- La respuesta comun de mutaciones devuelve `status`, `operationId`, `approvalId?`, `auditEventId`, `message` y `result`.
- Los valores minimos de `status` son `completed`, `pending_approval`, `denied` y `failed`.
- Riesgo alto minimo en v1:
  - `assignProjectRole` a rol administrativo;
  - `revokeSession` con alcance masivo;
  - `banUser` en cualquier scope;
  - cualquier accion marcada por policy como `high_risk`.
- La auditoria admin registra hitos como `requested`, `pending_approval`, `approved`, `rejected`, `completed`, `denied` y `failed`.
- `admin_action_audit` debe guardar al menos actor operativo, operador humano, proyecto objetivo, usuario objetivo, `reason`, `idempotencyKey`, `correlationId`, snapshots redactados, version de policy y resultado.
- `admin_approval` debe guardar al menos estado, solicitante, aprobador, nivel requerido y expiracion.
- Los snapshots de request y result deben redactar secretos, tokens y credenciales.

## Related Decisions

- ADR 0003 define `auth-service` como centro de autenticacion con aislamiento por proyecto.
- ADR 0004 mantiene a `mcp-server` fuera del rol de source of truth.
- ADR 0006 define OpenClaw como superficie admin inicial y abre la fase MCP posterior.
