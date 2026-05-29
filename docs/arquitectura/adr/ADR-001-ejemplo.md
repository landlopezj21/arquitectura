# ADR-001 — Adopción de Kafka como broker de mensajería

**Estado:** Aprobado
**Fecha:** 2024-03-15
**Autores:** Ana Torres, Carlos Mendoza
**Revisores:** Comité de Arquitectura
**ADRs relacionados:** —

---

## Contexto

Los equipos de Pedidos, Pagos y Notificaciones necesitan comunicarse entre sí de forma asíncrona. Actualmente cada integración usa su propio mecanismo (llamadas HTTP síncronas, cron jobs, polling a base de datos compartida), lo que genera acoplamiento fuerte, pérdida de mensajes ante fallos y dificultad para auditar flujos.

Se requiere una solución de mensajería centralizada que soporte:
- Al menos 10.000 mensajes/segundo en pico
- Retención de mensajes para reprocesamiento
- Múltiples consumidores por topic
- Trazabilidad de eventos de negocio

---

## Opciones consideradas

### Opción 1 — Apache Kafka

Plataforma de streaming distribuida con almacenamiento persistente de eventos.

**Ventajas:**
- Alta throughput (millones de mensajes/segundo)
- Retención configurable de mensajes (replay)
- Múltiples consumer groups independientes
- Ecosistema maduro (Kafka Connect, Kafka Streams)
- Soporte nativo en los tres principales clouds

**Desventajas:**
- Curva de aprendizaje mayor que RabbitMQ
- Operacionalmente más complejo de gestionar (Zookeeper / KRaft)
- Overhead para casos de uso simples de cola

---

### Opción 2 — RabbitMQ

Broker de mensajería tradicional basado en colas.

**Ventajas:**
- Más simple de operar
- Flexible en patrones de enrutamiento
- Buena documentación

**Desventajas:**
- No retiene mensajes por defecto tras el consumo
- Escalabilidad más limitada en escenarios de alta carga
- Sin soporte nativo para replay de eventos

---

### Opción 3 — AWS SQS + SNS

Servicios gestionados de AWS para colas y pub/sub.

**Ventajas:**
- Sin gestión de infraestructura
- Integración nativa con el ecosistema AWS

**Desventajas:**
- Acoplamiento a un proveedor cloud específico
- Retención máxima de 14 días en SQS
- Costo variable difícil de predecir en alto volumen

---

## Decisión

**Se elige la Opción 1 — Apache Kafka**, usando la distribución gestionada Confluent Cloud para entornos de producción y staging, y Kafka en Docker para desarrollo local.

El requerimiento de replay de eventos y la proyección de crecimiento a 12 meses descartaron RabbitMQ. La dependencia de proveedor y las limitaciones de retención descartaron SQS+SNS.

---

## Consecuencias

### Positivas
- Desacoplamiento real entre servicios de Pedidos, Pagos y Notificaciones
- Capacidad de auditoría y reprocesamiento de eventos históricos
- Base para implementar event sourcing en el futuro si se requiere

### Negativas / Compromisos asumidos
- Los equipos deben capacitarse en conceptos de Kafka (topics, partitions, consumer groups, offsets)
- Costo adicional por Confluent Cloud (~$400/mes en configuración inicial)
- Se necesita definir convenciones de naming para topics antes de comenzar

### Acciones de seguimiento
- [ ] Definir convención de naming para topics (propietario: equipo de Arquitectura, fecha límite: 2024-04-01)
- [ ] Crear módulo de librería compartida para producción/consumo con configuración estándar
- [ ] Documentar patrones de uso en la wiki técnica
- [ ] Configurar monitoreo de consumer lag en Datadog

---

## Referencias

- [Documentación oficial de Apache Kafka](https://kafka.apache.org/documentation/)
- [Ticket de evaluación PROJ-847](https://jira.ejemplo.com/PROJ-847)
- [Benchmarks internos de carga](https://confluence.ejemplo.com/paginas/kafka-benchmark-2024)
