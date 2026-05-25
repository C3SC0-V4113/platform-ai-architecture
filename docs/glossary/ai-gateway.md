# ai-gateway

Servicio de orquestacion multi-provider. Su trabajo es recibir una intencion de inferencia, decidir proveedor y modelo segun politica, costo y capacidad, y adaptar entradas o salidas al proveedor concreto.

No es el frontend del chat y no deberia ser el source of truth de archivos ni de usuarios.

Su importancia dentro del learning path no obliga a implementarlo completo antes de exponer los primeros proyectos publicos.

Tambien funciona como control-plane de proveedores. Cuando una capacidad esta en `auto`, puede seleccionar proveedor; cuando una capacidad tiene proveedor explicito, debe respetarlo y no hacer fallback silencioso.
