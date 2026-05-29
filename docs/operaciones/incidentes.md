# Gestión de Incidentes

Un incidente es cualquier evento que afecta la disponibilidad, rendimiento o seguridad de un sistema en producción de forma no planificada.

---

## Clasificación de incidentes

| Severidad | Impacto | Ejemplo | Tiempo de respuesta |
|---|---|---|---|
| **SEV1 — Crítico** | Sistema completamente caído o brecha de seguridad activa | Servicio de pagos no procesa transacciones | Inmediato (< 5 min) |
| **SEV2 — Alto** | Funcionalidad principal degradada, afecta a mayoría de usuarios | Latencia 10x superior al normal | < 15 min |
| **SEV3 — Medio** | Funcionalidad secundaria afectada o degradación parcial | Un flujo alternativo no funciona | < 1 hora |
| **SEV4 — Bajo** | Impacto mínimo, puede esperar horario hábil | Error en un reporte no crítico | Siguiente día hábil |

---

## Roles durante un incidente

| Rol | Responsabilidad |
|---|---|
| **Incident Commander (IC)** | Coordina la respuesta, toma decisiones, comunica el estado |
| **Técnico de respuesta** | Investiga la causa, implementa la solución |
| **Comunicaciones** | Actualiza la página de status y notifica a stakeholders |

En incidentes SEV3/SEV4 el técnico de guardia puede asumir todos los roles. En SEV1/SEV2 se recomienda separar roles.

---

## Proceso de respuesta

### 1. Detección y declaración (0–5 min)

- La alerta llega al técnico de guardia (PagerDuty / alerta Slack).
- Si es SEV1 o SEV2: declarar el incidente en el canal `#incidentes` con el formato:

```
🚨 INCIDENTE SEV[N] — [Descripción breve]
Sistemas afectados: [nombre del sistema]
Impacto: [quién y qué está afectado]
IC: @nombre
```

- Crear un hilo de seguimiento y mantenerlo actualizado.

### 2. Mitigación (primeros 30 min)

**Objetivo: restaurar el servicio, no encontrar la causa raíz.**

Acciones típicas de mitigación:
- Rollback al último despliegue estable.
- Escalar instancias / reiniciar pods.
- Activar feature flag para desactivar la funcionalidad afectada.
- Redirigir tráfico a región alternativa.

Publicar actualizaciones en el hilo cada **10 minutos** mientras el incidente esté activo.

### 3. Resolución

- Confirmar que los SLIs volvieron a niveles normales.
- Anunciar resolución en `#incidentes`:

```
✅ RESUELTO — [Descripción breve]
Duración: [X horas Y minutos]
Causa: [descripción breve]
Post-mortem: [fecha programada]
```

### 4. Post-mortem

Obligatorio para incidentes SEV1 y SEV2. Opcional pero recomendado para SEV3.

**Plazo:** completar el post-mortem en los **5 días hábiles** siguientes al incidente.

---

## Plantilla de post-mortem

```markdown
## Post-mortem — [Título del incidente]

**Fecha:** YYYY-MM-DD
**Duración:** X horas Y minutos
**Severidad:** SEV[N]
**Sistemas afectados:** [lista]
**Autores:** [nombres]

### Resumen ejecutivo
Una o dos oraciones describiendo qué pasó y el impacto.

### Línea de tiempo
| Hora | Evento |
|---|---|
| 14:32 | Primera alerta disparada |
| 14:35 | Técnico de guardia confirma el incidente |
| 14:50 | Se identifica causa raíz |
| 15:10 | Rollback ejecutado |
| 15:18 | Servicio restaurado |

### Causa raíz
Descripción técnica de qué causó el incidente.

### Factores contribuyentes
¿Qué condiciones permitieron que esto ocurriera?

### Qué salió bien
- Detección rápida gracias a alerta de burn rate
- Rollback funcionó sin problemas

### Qué mejorar
- La alerta tardó 8 minutos en dispararse — umbral demasiado alto

### Acciones de mejora
| Acción | Responsable | Fecha límite |
|---|---|---|
| Bajar umbral de alerta de latencia | Equipo Pagos | 2024-04-01 |
| Agregar prueba de integración para este escenario | Equipo Pagos | 2024-04-15 |
```

---

## Cultura de blameless post-mortem

El objetivo del post-mortem es aprender, no asignar culpa. Los sistemas fallan por condiciones sistémicas, no porque alguien "hizo algo mal". Las acciones de mejora deben apuntar a los sistemas, procesos y herramientas, no a las personas.
