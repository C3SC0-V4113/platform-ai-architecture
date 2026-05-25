# auth-service

Producto base de autenticacion del portafolio.

Centraliza identidad, sesiones, alta, revocacion y permisos, pero mantiene aislamiento por proyecto o tenant para no mezclar credenciales ni perfiles entre apps.

Su adopcion puede ser progresiva: primero habilita proyectos publicos/demostrables con roles por proyecto, y despues se conecta a mas piezas del ecosistema.
