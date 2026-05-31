# other-gpt

## Proposito

`other-gpt` es el frontend satelite principal del portafolio. Su trabajo es ofrecer una experiencia conversacional multimodal usable por personas, no convertirse en el backend central de auth, RAG, costos o operaciones.

Es uno de los proyectos publicos/demostrables principales del portafolio. Para exponerse fuera del entorno local necesita delegar acceso, usuarios y roles a `auth-service`.

## Punto(s) del learning path

- Principal: `4`.
- Futuro: `8`.
- Consumo parcial potencial: `7`.

## Tipo de proyecto

- `Frontend de chat`.

## Arquetipo frontend

- chat conversacional multimodal;
- composer persistente;
- historial visible;
- streaming;
- adjuntos;
- futura experiencia de voz;
- panel de configuracion de proveedores por usuario o proyecto.

## Stack actual

- `Next.js 16`
- `React 19`
- `TypeScript`
- `next-themes`
- `OpenAI SDK`
- componentes UI compuestos sobre base shadcn/Tailwind

## Estado real actual

### Arquitectura

- server-first por defecto;
- `app/page.tsx` y shell principal del chat en Server Components;
- islas cliente minimas para controlador del chat y componentes interactivos.

### Capacidades actuales

- texto con streaming;
- generacion de imagenes con previews parciales;
- adjuntos de archivos en contexto;
- speech-to-text;
- text-to-speech;
- limpieza manual de sesion de chat.

### Adjuntos y sesion hoy

- la sesion actual depende de cookie y store en memoria del proceso;
- los adjuntos son reutilizables dentro de la sesion actual del chat;
- ese modelo no es suficiente para persistencia real multi-login ni para portafolio multi-proyecto.

## Rol futuro dentro del portafolio

- seguir siendo el satelite principal del punto `4`;
- convertirse en la interfaz mas visible del futuro punto `8`;
- delegar autenticacion, usuarios y roles por proyecto a `auth-service`;
- delegar orquestacion de modelos a `ai-gateway`;
- delegar persistencia robusta de assets a `asset/document registry`;
- consumir tools via `mcp-server` cuando corresponda;
- considerar integraciones futuras con servicios externos de RAG solo si aportan valor claro a la experiencia.

## Configuracion futura de proveedores

`other-gpt` tendra un panel de configuracion para definir proveedores por capacidad:

- `chatProvider`: creador de chats;
- `imageProvider`: creador de imagenes;
- `ttsProvider`: text-to-speech;
- `sttProvider`: speech-to-text;
- `realtimeCallProvider`: llamadas realtime.

Cada opcion inicia en `auto`. En `auto`, `ai-gateway` decide proveedor y modelo segun politica, costo, capacidad, latencia o disponibilidad.

Si el usuario o proyecto define un proveedor explicito, `other-gpt` debe enviarlo como preferencia fijada. `ai-gateway` no debe reemplazarlo por otro proveedor de forma silenciosa; si falla, la UI debe mostrar un error accionable.

## Que guarda

- en su forma ideal futura: solo estado UI local y caches de experiencia de cliente.
- en su forma actual: parte del estado de sesion y mensajes vive localmente en el backend Next del proyecto.
- preferencias de experiencia o proveedor cuando correspondan, pero no secretos ni llaves de proveedor.

## Que no debe guardar como source of truth a futuro

- identidad central;
- costos globales;
- pricing de proveedores;
- secretos de proveedor;
- corpus RAG;
- tools MCP;
- storage persistente compartido del portafolio.

## Con quien habla

### Hoy

- OpenAI directamente desde su backend Next.

### Futuro

- `auth-service`
- `ai-gateway`
- `asset/document registry`
- `mcp-server` para tools

## Riesgos

- si absorbe auth, costos, RAG y orquestacion, deja de ser satelite y vuelve a ser centro accidental del portafolio;
- su persistencia actual no cubre el caso objetivo de logout/login con continuidad real.
- exponerlo publicamente sin roles ni aislamiento por proyecto.

## Estado esperado en el portafolio

Satelite de experiencia. Debe ser excelente como frontend conversacional, pero no debe cargar responsabilidades troncales de la plataforma.
