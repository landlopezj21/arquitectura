# SLOs y SLAs

Los Service Level Objectives (SLOs) son la herramienta principal para gestionar la confiabilidad. Definen qué significa "funcionar bien" para cada servicio.

---

## Conceptos

| Término | Definición |
|---|---|
| **SLI** (Service Level Indicator) | Métrica que mide el comportamiento del servicio (ej. tasa de errores, latencia) |
| **SLO** (Service Level Objective) | Objetivo interno para el SLI (ej. 99.9% de requests exitosos por mes) |
| **SLA** (Service Level Agreement) | Compromiso contractual con clientes externos basado en los SLOs |
| **Error Budget** | Margen de falla permitido. Si el SLO es 99.9%, el error budget mensual es ~43 minutos |

---

## SLOs mínimos por tipo de sistema

### Sistemas Críticos

| SLI | SLO | Ventana |
|---|---|---|
| Disponibilidad | ≥ 99.9% | Mensual (permite ~43 min de caída) |
| Tasa de error | < 0.1% de requests | Mensual |
| Latencia P99 | < 1000 ms | Por hora |
| Latencia P50 | < 200 ms | Por hora |

### Sistemas Internos

| SLI | SLO | Ventana |
|---|---|---|
| Disponibilidad | ≥ 99.5% | Mensual (permite ~3.6 horas de caída) |
| Tasa de error | < 1% de requests | Mensual |
| Latencia P99 | < 2000 ms | Por hora |

---

## Cómo definir SLOs para un nuevo servicio

1. **Identificar los SLIs relevantes** — ¿qué métricas importan para los usuarios de este servicio?
2. **Consultar el historial** — si el sistema existe, usar los últimos 90 días como base.
3. **Fijar el objetivo por debajo del historial** — el SLO debe ser alcanzable, no aspiracional.
4. **Documentar en el `catalog-info.yaml`** del servicio.
5. **Crear alertas basadas en el error budget**, no en valores absolutos.

---

## Gestión del error budget

El error budget es la cantidad de falla permitida. Cuando se agota, el equipo debe priorizar confiabilidad sobre nuevas funcionalidades.

### Política de error budget

| Estado del budget | Acción |
|---|---|
| > 50% restante | Velocidad normal de desarrollo |
| 25–50% restante | Aumentar cobertura de pruebas, revisar alertas |
| < 25% restante | Congelar features no críticas, priorizar estabilidad |
| Agotado | Solo hotfixes y mejoras de confiabilidad hasta que se recupere |

---

## Alertas basadas en error budget (burn rate)

En lugar de alertar cuando se supera un umbral puntual, alertar cuando el presupuesto se consume demasiado rápido:

```yaml
# Alerta crítica: si continúa este ritmo, el budget se agota en 1 hora
- alert: ErrorBudgetBurnRateCritico
  expr: error_budget_burn_rate > 14.4
  for: 5m
  labels:
    severity: critical

# Alerta de advertencia: budget agotado en 6 horas
- alert: ErrorBudgetBurnRateAlto
  expr: error_budget_burn_rate > 6
  for: 30m
  labels:
    severity: warning
```

---

## Revisión periódica de SLOs

Los SLOs se revisan trimestralmente. En cada revisión:

- Analizar si el SLO fue alcanzado en el trimestre anterior.
- Si se alcanzó con holgura (> 80% del budget siempre disponible), considerar aumentar el objetivo.
- Si se incumplió frecuentemente, investigar causas raíz antes de ajustar el objetivo.
- Documentar los cambios con fecha y justificación.
