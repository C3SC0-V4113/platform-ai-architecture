# admin-approval

Mecanismo de control operativo para acciones administrativas de alto riesgo.

En este portafolio aparece sobre todo cuando `openclaw-ops` invoca tools de `mcp-server` para operar `auth-service`. Si la policy considera que una accion no debe ejecutarse directo, `auth-service` crea una aprobacion pendiente en vez de aplicar el cambio.

La aprobacion no vive en `mcp-server` ni en OpenClaw. Vive en `auth-service`, expira por defecto en `24h`, exige trazabilidad por `operationId` y no permite que el mismo solicitante se autoapruebe.
