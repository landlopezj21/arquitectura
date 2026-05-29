# Estándares de Ingeniería

Bienvenido al repositorio central de estándares de ingeniería. Este sitio define las guías, convenciones y principios que todos los equipos deben seguir al diseñar, construir y operar sistemas.

---

## ¿Qué encontrarás aquí?

| Sección | Descripción |
|---|---|
| [Arquitectura](arquitectura/index.md) | Principios, patrones aprobados y registro de decisiones arquitectónicas (ADRs) |
| [Desarrollo](desarrollo/index.md) | Convenciones de código, estrategia de ramas, revisión de código y pipelines CI/CD |
| [Seguridad y Cumplimiento](seguridad/index.md) | Controles de seguridad obligatorios, privacidad de datos y normativas aplicables |
| [Operaciones y SRE](operaciones/index.md) | Observabilidad, SLOs, gestión de incidentes y protocolo on-call |

---

## Principio rector

> "Los estándares no existen para limitar la creatividad, sino para liberar a los equipos de tener que resolver los mismos problemas una y otra vez."

Estos estándares son documentos vivos. Cualquier equipo puede proponer cambios mediante Pull Request al repositorio. Los cambios de alto impacto deben respaldarse con un ADR.

## Cómo contribuir

1. Abre un issue describiendo el cambio o adición propuesta.
2. Crea una rama `docs/descripcion-del-cambio`.
3. Envía un Pull Request con los cambios.
4. El equipo de Arquitectura revisa y aprueba en un plazo máximo de 5 días hábiles.

## Versionado

Los estándares siguen versionado semántico. Cambios que rompen compatibilidad incrementan el número mayor (v1 → v2). Consulta el historial de cambios en el repositorio.
