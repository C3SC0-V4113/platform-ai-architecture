# mcp-server

## Proposito

Exponer tools reutilizables para apps, agentes y OpenClaw a traves de una superficie estandar.

Puede ser necesario temprano como mecanismo para que `openclaw-ops` ejecute operaciones controladas sobre `auth-service`, especialmente crear usuarios, promover roles y denegar accesos por proyecto.

## Punto(s) del learning path

- principal: `3`;
- aplica partes de `5`.

## Tipo de proyecto

- backend de tools.

## Arquetipo frontend si aplica

- no aplica.

## Stack recomendado

- `TypeScript`
- `MCP SDK`

## Que guarda

- schemas y definiciones de tools;
- configuracion de routing hacia servicios internos;
- logs y metricas de tool calls, si se desea.

## Que no guarda

- corpus RAG como source of truth;
- archivos originales;
- usuarios maestros;
- pricing can?nico.
- reglas canonicas de permisos de `auth-service`.

## Tools de auth esperadas

Como evolucion inmediata despues de levantar la API de `auth-service`, puede exponer tools para que `openclaw-ops` opere auth:

- `auth.createUser`;
- `auth.assignProjectRole`;
- `auth.revokeProjectAccess`;
- `auth.revokeSession`;
- `auth.banUser`;
- `auth.listProjectUsers`;
- `auth.getUserAccessStatus`.

Estas tools deben consumir el API de `auth-service`; no deben escribir directo en la base de datos.

## Con quien habla

- `knowledge-rag`
- `ai-gateway`
- `auth-service`
- `cost-console`
- `other-gpt`
- `openclaw-ops`

## Riesgos

- usar MCP como sustituto de APIs internas normales;
- exponer tools sin scopes ni politicas claras.
- implementar demasiadas tools antes de tener claro que operaciones necesita la exposicion publica inicial.

## Estado esperado en el portafolio

Proyecto principal del punto `3`, consumido por chat y operaciones. Su primera utilidad practica puede ser exponer tools operativas seguras para OpenClaw, sin reemplazar APIs internas.
