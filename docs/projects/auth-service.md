# auth-service

## Proposito

Producto base de autenticacion del portafolio. Centraliza identidad, sesiones, alta de usuarios, roles y permisos para apps actuales y futuras.

## Punto(s) del learning path

- transversal, con enfasis fuerte en `5`.

## Tipo de proyecto

- backend producto base.

## Arquetipo frontend si aplica

- portal de acceso y formularios de alta/login;
- consola administrativa basica en una fase posterior.

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
- `ai-gateway`
- `admin-console`
- `openclaw-ops`
- apps futuras del portafolio

## Riesgos

- sobrecomplicar el modelo de identidad demasiado pronto;
- mezclar accidentalmente credenciales entre proyectos.

## Estado esperado en el portafolio

Producto base central y reutilizable, no simple infraestructura interna invisible.
