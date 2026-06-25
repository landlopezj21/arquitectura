# Gestión de Incidentes

Un incidente es cualquier evento que afecta la disponibilidad, rendimiento o seguridad de un sistema en producción de forma no planificada.

[WIP]
---

## Clasificación de incidentes

| Severidad | Impacto | Ejemplo | Tiempo de respuesta |
|---|---|---|---|
| **SEV1 — Crítico** | Sistema completamente caído o brecha de seguridad activa | Servicio de pagos no procesa transacciones | Inmediato (< 5 min) |
| **SEV2 — Alto** | Funcionalidad principal degradada, afecta a mayoría de usuarios | Latencia 10x superior al normal | < 15 min |
| **SEV3 — Medio** | Funcionalidad secundaria afectada o degradación parcial | Un flujo alternativo no funciona | < 1 hora |
| **SEV4 — Bajo** | Impacto mínimo, puede esperar horario hábil | Error en un reporte no crítico | Siguiente día hábil |

---
