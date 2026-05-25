# ADR 0003: Centralize Authentication with Project Isolation

- Date: 2026-05-24
- Status: Accepted

## Context

Se quiere un servicio central de autenticacion para varias aplicaciones del portafolio y potencialmente para apps futuras fuera de los satelites actuales. Al mismo tiempo, no se desea mezclar credenciales ni perfiles entre proyectos de forma indiscriminada.

## Decision Drivers

- Tener un centro de mando claro de autenticacion.
- Permitir alta de usuarios, sesiones y revocacion desde un solo servicio.
- Mantener aislamiento de identidades y perfiles por proyecto.
- Evitar que cada satelite implemente auth propia.

## Decision

Se adopta `auth-service` como producto base central del portafolio.

`auth-service` controlara:

- alta de usuarios,
- login y logout por sesion,
- revocacion de sesiones,
- roles y permisos,
- aislamiento por proyecto o tenant,
- administracion de accesos para apps actuales y futuras.

El servicio sera centralizado, pero las credenciales y perfiles se mantendran aislados por proyecto. Esto implica un modelo tipo tenant, realm o app-scope, no una identidad global plana compartida por todas las apps.

## Consequences

### Positive

- Una sola superficie de identidad para el portafolio.
- Mejor gobierno de accesos y sesiones.
- Facilita crecimiento hacia mas apps sin reimplementar auth.

### Negative

- Mayor complejidad del modelo de auth desde el inicio.
- Requiere claridad al modelar proyectos, tenants y permisos.

## Related Decisions

- ADR 0002 define el contexto multi-repo donde auth-service se vuelve producto base.
- ADR 0004 depende de auth-service para persistencia y aislamiento por proyecto.
