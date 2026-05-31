# mcp-server

Servicio que expone tools estandarizadas para apps y agentes.

Su responsabilidad es ofrecer una superficie de herramientas reutilizable y consistente. No reemplaza las APIs internas de negocio ni es source of truth de datos.

Una utilidad temprana puede ser exponer tools seguras para que OpenClaw opere capacidades de otros servicios, por ejemplo acciones administrativas de `auth-service`.

Cuando exponga tools de auth, debe tratarlas como fachada operativa: las tools llaman al API de `auth-service` y no se convierten en source of truth de usuarios, permisos o sesiones.

Para auth admin, tambien carga el contrato operativo comun de las tools, propaga identidad operativa, `correlationId` e `idempotencyKey`, y devuelve si una accion quedo `completed` o `pending_approval`. No decide permisos canonicos ni reemplaza la auditoria de negocio.
