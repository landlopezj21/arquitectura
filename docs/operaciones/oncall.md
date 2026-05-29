# On-call

El protocolo de guardia define cómo los equipos se turnan la responsabilidad de responder a incidentes fuera del horario hábil.

---

## Principios

- Quien construye el sistema, lo opera. El equipo de desarrollo lleva guardia de sus propios servicios.
- La guardia debe ser sostenible. Nadie puede estar de guardia efectiva si está agotado.
- El objetivo no es estar disponible 24/7 — es que alguien pueda responder cuando realmente importa.

---

## Estructura de la guardia

| Rol | Responsabilidad | Tiempo de respuesta |
|---|---|---|
| **Guardia primaria** | Primera línea de respuesta. Recibe todas las alertas. | < 5 min en horario, < 15 min fuera de horario |
| **Guardia secundaria** | Escalamiento si el primario no responde o necesita apoyo en SEV1. | < 15 min |
| **Escalamiento gerencial** | Solo para SEV1 con impacto en clientes o riesgo regulatorio. | < 30 min |

---

## Rotación

- Turnos de **1 semana** por persona.
- Cada persona no puede tener más de **1 semana de guardia por mes**.
- El calendario se publica con al menos **4 semanas de anticipación**.
- Se puede intercambiar turno con acuerdo entre los involucrados y notificación al equipo.
- Nadie entra a guardia primaria sin haber hecho al menos un turno de guardia secundaria previamente.

---

## Antes de entrar a guardia

El técnico que inicia turno debe verificar:

- [ ] Notificaciones de PagerDuty activadas en el teléfono.
- [ ] Acceso a los sistemas de producción funcionando (VPN, credenciales).
- [ ] Runbooks actualizados y accesibles.
- [ ] Dashboard de salud de los servicios revisado — no entrar con alertas activas sin resolver.
- [ ] Handoff con el técnico saliente: ¿hay algo que debo saber?

---

## Runbooks

Cada servicio debe tener un runbook documentado en su repositorio en `docs/runbook.md`. El runbook contiene los pasos para diagnosticar y resolver los problemas más frecuentes.

**Estructura mínima de un runbook:**

```markdown
# Runbook — [Nombre del servicio]

## Accesos requeridos
- Consola AWS: https://...
- Dashboard Grafana: https://...
- Logs en Datadog: https://...

## Alertas comunes y cómo resolverlas

### Alta latencia en endpoint /checkout
1. Verificar en Grafana si hay spike de tráfico.
2. Revisar logs de errores en Datadog: `service:pagos level:error`.
3. Verificar conexiones a la BD: [link al dashboard].
4. Si la BD está saturada: escalar instancia RDS desde la consola AWS.
5. Si el problema persiste: escalar a guardia secundaria.

### Pod en CrashLoopBackOff
1. Revisar logs del pod: `kubectl logs <pod> -n produccion --previous`
2. Identificar el error en los últimos 50 líneas.
3. Si es error de configuración: verificar variables de entorno en el secret.
4. Si es error de código: iniciar proceso de rollback.
```

---

## Compensación y bienestar

- Una noche con incidente SEV1 o SEV2 (más de 1 hora de trabajo) se compensa con **medio día libre** al día siguiente.
- Si el técnico de guardia fue despertado más de 3 veces en una semana, tiene derecho a solicitar reasignación del turno.
- La carga de guardia (cantidad de alertas, incidentes por turno) se revisa mensualmente en la retrospectiva del equipo.

---

## Métricas de on-call

El equipo debe rastrear mensualmente:

| Métrica | Objetivo |
|---|---|
| Alertas fuera de horario por semana | < 5 por técnico |
| Tiempo promedio de reconocimiento (MTTA) | < 10 min |
| Tiempo promedio de resolución (MTTR) | < 2 horas para SEV1 |
| % de alertas que requieren acción humana | > 80% (si es menor, hay demasiado ruido) |

Las métricas de on-call son indicadores de salud del sistema, no de rendimiento individual.
