# cost-console

## Proposito

Dashboard y capa de inteligencia de costos del portafolio.

Es uno de los proyectos publicos/demostrables que se quiere exponer temprano, por lo que necesita integrarse con `auth-service` para acceso controlado y roles por proyecto.

## Punto(s) del learning path

- principal: `7`;
- aplica partes de `6`.

## Tipo de proyecto

- frontend dashboard.

## Arquetipo frontend

- dashboard analitico con tablas, filtros, comparativas, forecast y recomendacion de modelos.

## Stack recomendado

- `Next.js`
- `TypeScript`

## Que guarda

- pricing conocido por proveedor o modelo;
- consumo agregado;
- presupuestos;
- recomendaciones o reglas de comparacion.

## Que no guarda

- identidad maestra;
- assets can?nicos;
- corpus RAG.

## Con quien habla

- `ai-gateway`
- `auth-service`
- `openclaw-ops`
- `other-gpt` de forma parcial

## Riesgos

- intentar convertirlo en gateway;
- mezclar visualizacion de costos con routing de inferencia.
- exigir integracion completa con todos los servicios antes de que exista una demo util.

## Estado esperado en el portafolio

Proyecto visible del punto `7`, util tanto como dashboard como capability de recomendacion economica. Debe poder exponerse como recurso publico del portafolio aunque algunas integraciones, como `ai-gateway`, maduren despues.
