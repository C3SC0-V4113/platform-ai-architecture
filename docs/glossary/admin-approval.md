# admin-approval

Mecanismo de control operativo para acciones administrativas de alto riesgo.

En este portafolio aparece sobre todo cuando `openclaw-ops` invoca tools de `mcp-server` para operar `auth-service`. Si la policy considera que una accion no debe ejecutarse directo, `auth-service` crea una aprobacion pendiente en vez de aplicar el cambio.

La aprobacion no vive en `mcp-server` ni en OpenClaw. Vive en `auth-service`, expira por defecto en `24h` y exige trazabilidad por `operationId`.

En la fase inicial de una sola identidad operativa, la aprobacion funciona como confirmacion deliberada de segundo paso. Cuando exista una segunda identidad operativa, el modelo debe volver a separar solicitante y aprobador para que el mismo operador no autoapruebe su accion sensible.
