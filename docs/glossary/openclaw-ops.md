# openclaw-ops

Uso de OpenClaw como interfaz operativa del portafolio.

Sirve para consumir skills, tools y canales como Telegram, especialmente para operaciones y consulta de capacidades ya construidas. No debe convertirse en el backend central del ecosistema.

Puede ser la primera superficie para operar acciones administrativas delegadas, como crear usuarios, cambiar roles, revocar sesiones, banear usuarios o denegar accesos mediante `auth-service`.

Para la fase inicial de auth, se considera una superficie admin global del portafolio. No guarda identidad maestra, no define permisos canonicos y sus operaciones deben quedar auditadas por `auth-service`.
