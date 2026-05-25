# auth-service

## Proposito

Producto base de autenticacion del portafolio. Centraliza identidad, sesiones, alta de usuarios, roles y permisos para apps actuales y futuras.

Su adopcion inicial se enfoca en proyectos publicos o demostrables del portafolio, especialmente `other-gpt`, `cost-console` y `openclaw-ops`. La meta temprana no es conectar todo el ecosistema, sino permitir exponer esos proyectos con usuarios y roles separados por proyecto.

## Punto(s) del learning path

- transversal, con enfasis fuerte en `5`.

## Tipo de proyecto

- backend producto base.

## Arquetipo frontend si aplica

- portal de acceso y formularios de alta/login;
- consola administrativa basica en una fase posterior.

## Capacidades operativas iniciales

- crear usuarios para un proyecto especifico;
- promover usuarios a roles superiores por proyecto;
- denegar o revocar acceso por proyecto;
- consultar usuarios, roles y estado de acceso por proyecto;
- exponer esas acciones primero para `openclaw-ops`, idealmente mediante tools o MCP.

## Stack recomendado

- `Fastify`
- `TypeScript`
- `Zod`
- `PostgreSQL`

## Que guarda

- usuarios;
- credenciales por proyecto o tenant;
- sesiones;
- roles;
- permisos;
- revocaciones;
- auditoria de acceso.

## Que no guarda

- adjuntos del chat;
- corpus RAG;
- pricing de modelos;
- tools MCP;
- mensajes de conversacion.

## Con quien habla

- `other-gpt`
- `cost-console`
- `openclaw-ops`
- `mcp-server` cuando se necesiten tools operativas
- `admin-console` en una fase posterior
- `ai-gateway` de forma posterior o segun necesidad
- apps futuras del portafolio

## Riesgos

- sobrecomplicar el modelo de identidad demasiado pronto;
- mezclar accidentalmente credenciales entre proyectos.
- bloquear la exposicion de proyectos publicos esperando una consola administrativa completa.

## Estado esperado en el portafolio

Producto base central y reutilizable, no simple infraestructura interna invisible. Debe habilitar primero el acceso controlado a proyectos publicos del portafolio y luego crecer hacia integraciones mas amplias.
