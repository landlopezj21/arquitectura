# Observabilidad

Todo sistema en producción debe implementar los tres pilares de observabilidad antes de su primer despliegue. Un sistema que no se puede observar no se puede operar.

[WIP]
---

## Los tres pilares

### 1. Logs estructurados

Los logs deben ser estructurados en formato JSON para facilitar su búsqueda y análisis.

**Formato mínimo requerido:**
```json
{
  "timestamp": "2024-03-15T10:30:00.000Z",
  "level": "info",
  "service": "servicio-pagos",
  "version": "2.3.1",
  "traceId": "abc123def456",
  "message": "Transacción procesada",
  "userId": "usr_789",
  "transactionId": "txn_456",
  "durationMs": 234
}
```

**Niveles de log y cuándo usarlos:**

| Nivel | Cuándo usarlo |
|---|---|
| `error` | Errores que requieren atención inmediata; generan alerta |
| `warn` | Situaciones anómalas que no rompen el flujo pero deben monitorearse |
| `info` | Eventos de negocio importantes (transacción completada, usuario creado) |
| `debug` | Información detallada útil para diagnóstico; desactivado en producción |

**Prohibido en logs:**
- Contraseñas, tokens, claves de API
- Números de tarjeta de crédito o datos bancarios completos
- Datos personales sensibles sin enmascarar

---

### 2. Métricas

Cada servicio debe exponer métricas en formato Prometheus en el endpoint `/metrics`.

**Métricas obligatorias para APIs HTTP (RED):**

```
# Rate — solicitudes por segundo
http_requests_total{method, route, status_code}

# Errors — tasa de errores
http_requests_total{status_code="5xx"}

# Duration — latencia de respuesta
http_request_duration_seconds{method, route, quantile}
```

**Métricas de negocio — ejemplos:**
```
# Transacciones procesadas
pagos_transacciones_total{estado="completada|fallida|pendiente"}

# Valor procesado
pagos_monto_procesado_total{moneda}

# Tiempo de procesamiento
pagos_procesamiento_duration_seconds
```

**Dashboards requeridos en Grafana:**
- Dashboard de salud del servicio (RED metrics)
- Dashboard de métricas de negocio
- Dashboard de infraestructura (CPU, memoria, conexiones)

---

### 3. Trazas distribuidas

Usar OpenTelemetry como estándar de instrumentación. Las trazas permiten seguir una solicitud a través de múltiples servicios.

**Configuración mínima:**
```typescript
// Ejemplo Node.js con OpenTelemetry
import { NodeSDK } from '@opentelemetry/sdk-node';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';

const sdk = new NodeSDK({
  serviceName: 'mi-servicio',
  traceExporter: new OTLPTraceExporter({
    url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT,
  }),
});
```

**Reglas de trazas:**
- Propagar el `traceId` en todas las llamadas entre servicios (header `traceparent`).
- Incluir el `traceId` en todos los logs (correlación logs-trazas).
- Instrumentar automáticamente clientes HTTP, llamadas a BD y colas de mensajes.

---

## Herramientas aprobadas

| Pilar | Herramienta | Uso |
|---|---|---|
| Logs | ELK  | Centralización, búsqueda y alertas |
| Métricas | Prometheus + Grafana | Visualización y alertas |
| Trazas | ELK | Análisis de latencia y errores distribuidos |

---

## Health checks

Todo servicio debe exponer los siguientes endpoints:

```
GET /health/live    → 200 si el proceso está vivo (para liveness probe)
GET /health/ready   → 200 si el servicio puede recibir tráfico (para readiness probe)
```


