# Principios de Diseño

## 5.1 Loose Coupling

Los componentes interactúan a través de interfaces bien definidas,
minimizando dependencias directas.

## 5.2 High Cohesion

Cada servicio tiene una responsabilidad clara y bien definida dentro de
su dominio.

## 5.3 Single Responsibility

Cada componente se enfoca en una funcionalidad específica, facilitando
mantenimiento y evolución.

## 5.4 Open/Closed Principle

La arquitectura está abierta para extensión pero cerrada para
modificación, permitiendo añadir nuevas funcionalidades sin afectar
componentes existentes.

## 5.5 Dependency Inversion

Los módulos de alto nivel no dependen de módulos de bajo nivel, ambos
dependen de abstracciones.




# Principios de Arquitectura

Los principios de arquitectura son las reglas fundamentales que guían todas las decisiones de diseño en la organización. Son la base sobre la que se evalúan los patrones y las decisiones técnicas.

---

## P01 — Diseño orientado al negocio

Los sistemas deben diseñarse para resolver problemas de negocio concretos, no para adoptar tecnología por su novedad. Cada componente técnico debe poder justificarse en términos de valor entregado.

**Implicaciones:** antes de proponer una solución técnica, define el problema de negocio que resuelve y el resultado esperado.

---

## P02 — Preferir simplicidad

La solución más simple que resuelva el problema correctamente es la solución correcta. La complejidad tiene un costo de mantenimiento que se paga indefinidamente.

**Implicaciones:** evitar abstracciones prematuras, microservicios innecesarios o frameworks sin justificación clara.

---

## P03 — Sistemas desacoplados

Los componentes deben comunicarse a través de interfaces bien definidas y minimizar el conocimiento mutuo. El acoplamiento fuerte es deuda técnica.

**Implicaciones:** preferir comunicación asíncrona vía eventos, definir contratos de API explícitos, evitar dependencias circulares.

---

## P04 — Seguridad desde el diseño

La seguridad no es una capa que se agrega al final; es un atributo de calidad que se diseña desde el inicio. Todo sistema que maneje datos sensibles debe pasar una revisión de seguridad antes de ir a producción.

**Implicaciones:** modelado de amenazas en fase de diseño, revisión de código con foco en seguridad, pruebas de penetración para sistemas críticos.

---

## P05 — Observabilidad como requisito

Todo sistema en producción debe ser observable: logs estructurados, métricas expuestas y trazas distribuidas. Un sistema que no se puede monitorear no se puede operar.

**Implicaciones:** implementar los tres pilares de observabilidad (logs, métricas, trazas) antes del primer despliegue a producción.

---

## P06 — Automatización sobre proceso manual

Los procesos repetibles deben automatizarse. Los pasos manuales son fuentes de error y cuellos de botella. Esto aplica a despliegues, pruebas, generación de documentación e infraestructura.

**Implicaciones:** infraestructura como código (IaC), pipelines CI/CD obligatorios, sin despliegues manuales a producción.

---

## P07 — Datos como activo compartido

Los datos pertenecen a la organización, no a los sistemas que los generan. Los sistemas deben exponer sus datos de manera controlada y documentada para facilitar su reutilización.

**Implicaciones:** contratos de datos explícitos, catálogo de datos actualizado, evitar silos de información.

---

## P08 — Falla con gracia

Los sistemas deben diseñarse asumiendo que los componentes dependientes fallarán. Implementar patrones de resiliencia (circuit breaker, retry con backoff, timeouts) es obligatorio en integraciones externas.

**Implicaciones:** probar fallos en staging, definir comportamiento degradado para cada integración crítica.

---

## Actualización de principios

Los principios se revisan anualmente por el Comité de Arquitectura. Propuestas de cambio deben presentarse con al menos 30 días de anticipación a la revisión anual.
