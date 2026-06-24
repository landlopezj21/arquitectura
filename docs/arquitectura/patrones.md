# Patrones Arquitectónicos Implementados

## 4.1 API Gateway Pattern

Implementado através de Kong, proporcionando un punto de entrada único
que simplifica la gestión de APIs y mejora la seguridad.

## 4.2 Microservices Pattern

Servicios independientes que pueden desarrollarse, desplegarse y
escalarse de forma autónoma.

## 4.3 Event-Driven Architecture

Comunicación asíncrona a través de eventos que mejora el desacoplamiento
y la escalabilidad.

## 4.4 CQRS (Command Query Responsibility Segregation)

Separación de operaciones de lectura y escritura para optimizar
performance y escalabilidad.

## 4.5 Saga Pattern

Gestión de transacciones distribuidas a través de múltiples servicios.

## 4.6 Circuit Breaker Pattern

Protección contra fallos en cascada mediante aislamiento de servicios
defectuosos.


# Patrones de Arquitectura

Catálogo de patrones aprobados, patrones condicionales y patrones a evitar. Cada equipo debe justificar el uso de un patrón condicional mediante un ADR.

---

## Patrones aprobados ✅

### API Gateway

Punto de entrada único para clientes externos. Centraliza autenticación, rate limiting, logging y enrutamiento.

**Cuándo usarlo:** siempre que se expongan servicios hacia el exterior o hacia otras áreas de negocio.
**Implementación de referencia:** Kong, AWS API Gateway.

---

### Event-driven con broker de mensajes

Comunicación asíncrona entre servicios a través de un broker central. Desacopla productores de consumidores.

**Cuándo usarlo:** flujos de negocio que no requieren respuesta inmediata, integración entre dominios de negocio distintos.
**Implementación de referencia:** Kafka, RabbitMQ.

---

### Repository Pattern

Abstracción de la capa de acceso a datos. El dominio no conoce el mecanismo de persistencia.

**Cuándo usarlo:** en cualquier componente con lógica de negocio que acceda a base de datos.

---

### Circuit Breaker

Protege al sistema ante fallos en cascada. Corta el circuito cuando un servicio dependiente supera el umbral de errores.

**Cuándo usarlo:** todas las integraciones con servicios externos o servicios internos críticos.
**Implementación de referencia:** Resilience4j, Polly (.NET).

---

### Infrastructure as Code (IaC)

Toda infraestructura se define como código versionado. Sin aprovisionamiento manual.

**Cuándo usarlo:** siempre. Sin excepciones para entornos de producción o staging.
**Implementación de referencia:** Terraform.

---

## Patrones condicionales ⚠️

Requieren ADR aprobado antes de implementarse.

### Microservicios

Descomposición del sistema en servicios independientes desplegables.

**Cuándo considerarlo:** cuando un monolito tiene equipos claramente separados con ritmos de despliegue distintos y dominios bien delimitados.
**Riesgo:** complejidad operacional significativa. No usar para equipos pequeños o sistemas en etapa temprana.

---

### CQRS (Command Query Responsibility Segregation)

Separación de modelos de lectura y escritura.

**Cuándo considerarlo:** sistemas con patrones de lectura y escritura muy diferentes, alta carga de lectura, reportería compleja.
**Riesgo:** complejidad de sincronización entre modelos. Evaluar si la complejidad se justifica.

---

### Sagas (transacciones distribuidas)

Coordinación de transacciones que abarcan múltiples servicios mediante coreografía u orquestación.

**Cuándo considerarlo:** solo cuando se tienen microservicios con bases de datos separadas y se necesita consistencia eventual.
**Riesgo:** muy complejo de implementar y depurar. Preferir transacciones locales siempre que sea posible.

---

## Patrones a evitar ❌

| Patrón | Motivo |
|---|---|
| Big Ball of Mud | Sin estructura, imposible de mantener a escala |
| Shared Database entre servicios | Acoplamiento fuerte, rompe la autonomía de los servicios |
| Llamadas síncronas en cadena (>3 saltos) | Fragilidad ante fallos, latencia acumulada |
| God Object / God Service | Viola el principio de responsabilidad única |
| Polling activo como alternativa a eventos | Ineficiente; usar webhooks o mensajería |
