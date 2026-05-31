# auth-service

Producto base de autenticacion del portafolio.

Centraliza identidad, sesiones, alta, revocacion y permisos, pero mantiene aislamiento por proyecto o tenant para no mezclar credenciales ni perfiles entre apps.

Los roles de usuarios finales son por proyecto: un rol en `other-gpt` no implica permisos en `cost-console`. La relacion entre usuario, proyecto y roles debe modelarse como una membresia o scope equivalente.

`openclaw-ops` opera como superficie administrativa global inicial. No es un tenant regular ni reemplaza la logica de permisos; invoca acciones administrativas de `auth-service` y esas acciones deben quedar auditadas.

Cuando esas acciones viajan por MCP, `auth-service` sigue siendo el punto unico de autorizacion, aprobacion y auditoria. Si una operacion es de alto riesgo, puede abrir una aprobacion previa y exigir que un segundo operador la decida.

Su adopcion puede ser progresiva: primero habilita proyectos publicos/demostrables con roles por proyecto, y despues se conecta a mas piezas del ecosistema.
