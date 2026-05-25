# ADR 0005: Use AI Gateway as Provider Control Plane

- Date: 2026-05-25
- Status: Accepted

## Context

`other-gpt` necesita crecer hacia una experiencia multimodal configurable: chat, generacion de imagenes, text-to-speech, speech-to-text y llamadas realtime. Al mismo tiempo, el portafolio busca que esas capacidades puedan usar OpenAI, ElevenLabs u otros proveedores futuros sin acoplar la UI de Next.js a cada proveedor.

Tambien se quiere que la experiencia visual de `other-gpt` sea limpia y que la llamada realtime se sienta integrada, aunque por debajo use WebRTC, WebSocket, tokens efimeros, signed URLs o APIs especificas de cada proveedor.

## Decision Drivers

- Mantener `other-gpt` como satelite de experiencia y no como orquestador central.
- Permitir seleccion automatica de proveedor cuando el usuario o proyecto elige `auto`.
- Respetar una configuracion explicita de proveedor sin fallback silencioso.
- Preparar el portafolio para OpenAI, ElevenLabs y proveedores futuros.
- Centralizar politica, costo, logs, credenciales y adaptadores en `ai-gateway`.

## Decision

Se adopta `ai-gateway` como control-plane de proveedores y broker de sesiones para capacidades multimodales.

`other-gpt` tendra un panel futuro de configuracion donde podra definir:

- `chatProvider`;
- `imageProvider`;
- `ttsProvider`;
- `sttProvider`;
- `realtimeCallProvider`.

Cada opcion podra estar en `auto` o en un proveedor explicito.

Cuando una opcion este en `auto`, `ai-gateway` podra decidir proveedor y modelo segun politica, costo, capacidad, latencia, disponibilidad o configuracion global.

Cuando una opcion tenga un proveedor explicito, esa configuracion funcionara como hard pin. `ai-gateway` podra validar credenciales, disponibilidad y compatibilidad, pero no debera sustituir automaticamente el proveedor por otro. Si el proveedor explicito falla, `ai-gateway` debe devolver un error accionable para la UI en vez de hacer fallback silencioso.

Para llamadas realtime, `ai-gateway` puede actuar como control-plane sin relayar todo el audio. Puede crear sesiones, tokens, signed URLs, instrucciones, metadata, politicas y adaptadores por proveedor, mientras `other-gpt` mantiene la UI de llamada y abre la conexion de baja latencia que corresponda.

## Consequences

### Positive

- `other-gpt` mantiene una UI limpia y delega la complejidad multi-provider.
- `ai-gateway` se vuelve el punto natural para politica, costo, auditoria y seleccion de proveedor.
- La configuracion explicita del usuario o proyecto es respetada de forma predecible.
- OpenAI, ElevenLabs y proveedores futuros pueden agregarse mediante adaptadores.

### Negative

- `ai-gateway` debe modelar capacidades por proveedor, no solo modelos de texto.
- La UI necesita manejar errores cuando un proveedor fijado no esta disponible.
- No todos los proveedores tendran paridad exacta de capacidades o protocolo.

## Related Decisions

- ADR 0003 define el aislamiento por proyecto que debe respetar la configuracion.
- ADR 0004 mantiene a `other-gpt` como satelite de experiencia y separa responsabilidades de datos, RAG y tools.
