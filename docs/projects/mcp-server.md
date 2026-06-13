# mcp-server

## Proposito

Exponer tools reutilizables para apps, agentes y OpenClaw a traves de una superficie estandar.

Puede ser necesario temprano como mecanismo para que `openclaw-ops` ejecute operaciones controladas sobre `auth-service`, especialmente crear usuarios, promover roles y denegar accesos por proyecto. Para auth admin, la familia de tools debe modelarse como fachada operativa con contrato comun, idempotencia y aprobacion por riesgo.

## Punto(s) del learning path

- principal: `3`;
- aplica partes de `5`.

## Tipo de proyecto

- backend de tools.

## Arquetipo frontend si aplica

- no aplica.

## Stack recomendado

- `TypeScript`
- `Cloudflare Workers`
- `Cloudflare Agents SDK / MCP SDK`

## Que guarda

- schemas y definiciones de tools;
- configuracion de routing hacia servicios internos;
- logs y metricas de tool calls, si se desea.

## Que no guarda

- corpus RAG como source of truth;
- archivos originales;
- usuarios maestros;
- pricing can?nico.
- reglas canonicas de permisos de `auth-service`.

## Tools de auth esperadas

Como evolucion inmediata despues de levantar la API de `auth-service`, puede exponer tools para que `openclaw-ops` opere auth:

- `auth.createUser`;
- `auth.assignProjectRole`;
- `auth.revokeProjectAccess`;
- `auth.revokeSession`;
- `auth.banUser`;
- `auth.unbanUser`;
- `auth.readmitProjectMembership`;
- `auth.listProjectUsers`;
- `auth.getUserAccessStatus`;
- `auth.listPendingApprovals`;
- `auth.decideApproval`.

Estas tools deben consumir el API de `auth-service`; no deben escribir directo en la base de datos.

## Contrato operativo minimo

- Las mutaciones usan un sobre comun con `targetProjectId`, `reason`, `idempotencyKey`, `ticketRef?`, `channel` y `payload`.
- En v1, `mcp-server` autentica agentes controlados con bearer token propio y llama a `auth-service` con credencial de servicio. La atribucion de operador humano solo se reenvia cuando el contrato admin la recibe; una sesion real de OpenClaw puede reemplazar o enriquecer este modelo en una fase posterior.
- Las respuestas mutantes devuelven `status`, `operationId`, `approvalId?`, `auditEventId`, `message` y `result`.
- Los valores minimos de `status` en v1 son `completed`, `pending_approval`, `denied` y `failed`.
- `mcp-server` valida schema MCP y propaga `correlationId`, pero la decision canonica de permisos, aprobacion y auditoria vive en `auth-service`.
- La v1 se expone solo a agentes controlados por el operador del portafolio, protegida por bearer token en el borde MCP. OAuth o Cloudflare Access quedan para una fase posterior con agentes externos.

## Politica minima de ejecucion

- Las lecturas ejecutan directo.
- Las mutaciones de bajo riesgo pueden ejecutar directo si `auth-service` lo permite.
- Las mutaciones de alto riesgo deben devolver `pending_approval` y luego resolverse con `auth.decideApproval`.
- Mientras exista una sola identidad operativa, `decideApproval` funciona como confirmacion deliberada de segundo paso. Cuando exista una segunda identidad operativa, debe reintroducirse la separacion solicitante/aprobador.
- `mcp-server` no debe exponer una tool generica tipo `runAdminAction`; la v1 usa tools discretas y schemas explicitos.

## Con quien habla

- `ai-gateway`
- `auth-service`
- `cost-console`
- `other-gpt`
- `openclaw-ops`

## Riesgos

- usar MCP como sustituto de APIs internas normales;
- exponer tools sin scopes ni politicas claras.
- intentar mover la decision de riesgo o aprobacion fuera de `auth-service`.
- implementar demasiadas tools antes de tener claro que operaciones necesita la exposicion publica inicial.

## Estado esperado en el portafolio

Proyecto principal del punto `3`, consumido por chat y operaciones. Su primera utilidad practica puede ser exponer tools operativas seguras para OpenClaw, sin reemplazar APIs internas.
