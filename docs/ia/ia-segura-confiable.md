# IA Segura, Confiable y Auditada — Sin Shadow AI

**Versión:** 1.0  
**Fecha:** Junio 2026  
**Audiencia:** Dirección ejecutiva, arquitectura, seguridad, cumplimiento y equipos de tecnología

---

> **Premisa:** La IA no gobernada no es más rápida — es más peligrosa.  
> Shadow AI es el equivalente moderno de Shadow IT, pero con consecuencias potencialmente más graves: fuga de datos confidenciales, decisiones automatizadas sin trazabilidad y exposición regulatoria sin visibilidad.

---

## Tabla de Contenidos

1. [¿Qué es Shadow AI?](#qué-es-shadow-ai)
2. [Riesgos Reales del Shadow AI](#riesgos-reales-del-shadow-ai)
3. [Los Tres Pilares: Segura, Confiable y Auditada](#los-tres-pilares)
4. [Marco de Control Anti-Shadow AI](#marco-de-control-anti-shadow-ai)
5. [Arquitectura de IA Corporativa Aprobada](#arquitectura-de-ia-corporativa-aprobada)
6. [Proceso de Incorporación de Herramientas de IA](#proceso-de-incorporación-de-herramientas-de-ia)
7. [Detección y Respuesta ante Shadow AI](#detección-y-respuesta-ante-shadow-ai)
8. [Cultura: La Raíz del Problema](#cultura-la-raíz-del-problema)
9. [Indicadores de Madurez](#indicadores-de-madurez)
10. [Hoja de Ruta de Implementación](#hoja-de-ruta-de-implementación)

---

## ¿Qué es Shadow AI?

Shadow AI es el uso de **herramientas, modelos o sistemas de inteligencia artificial fuera del conocimiento, aprobación o control de la organización**. Ocurre cuando empleados, equipos o contratistas adoptan soluciones de IA de forma autónoma, sin pasar por los procesos de evaluación, seguridad y gobierno establecidos.

### Formas comunes de Shadow AI

| Forma | Descripción | Ejemplo |
|-------|-------------|---------|
| **Herramientas de productividad no aprobadas** | Uso de asistentes de IA de terceros para tareas de trabajo | Pegar código propietario en ChatGPT, Gemini o Copilot sin licencia corporativa |
| **APIs de IA sin contrato** | Llamadas directas a APIs de modelos sin revisión legal o de seguridad | Integración casera a OpenAI API con tarjeta personal |
| **Modelos locales no registrados** | Ejecución de modelos open-source sin revisión | LLaMA o Mistral instalados en laptops corporativas sin aprobación |
| **Automatizaciones con IA embebida** | Flujos de trabajo que incorporan IA sin documentarla | Zapier / Make con nodos de IA que procesan datos de clientes |
| **IA en aplicaciones SaaS** | Funciones de IA activadas en herramientas SaaS ya contratadas | Activar Copilot en Microsoft 365 sin política de uso definida |
| **Modelos desarrollados ad hoc** | Modelos entrenados por equipos sin proceso formal | Script de ML en Python entrenado con datos de producción |

### ¿Por qué ocurre?

Shadow AI no nace de mala fe. Nace de:

- **Fricción excesiva** en los procesos de aprobación de herramientas.
- **Ventaja de productividad percibida** que los empleados quieren aprovechar.
- **Falta de alternativas corporativas** equivalentes a las herramientas externas.
- **Desconocimiento del riesgo**: los empleados no saben qué datos son sensibles.
- **Cultura de resultados sin marco**: se premia el output, no el proceso.

La solución no es solo prohibir — es **ofrecer una alternativa mejor y más fácil**.

---

## Riesgos Reales del Shadow AI

### Riesgo 1: Fuga de Datos Confidenciales

Cuando un empleado pega información en un modelo de IA de tercero sin contrato de procesamiento de datos (DPA), esa información puede:

- Quedar almacenada en los servidores del proveedor.
- Usarse para reentrenamiento del modelo.
- Quedar expuesta ante brechas de seguridad del proveedor.
- Violar GDPR, LGPD u otras normativas de privacidad.

**Datos de alto riesgo frecuentemente expuestos:** código fuente propietario, datos de clientes, información financiera, estrategia corporativa, datos de empleados.

### Riesgo 2: Decisiones sin Trazabilidad

Un modelo de IA que automatiza una decisión de negocio sin registro de auditoría:

- No puede ser auditado ante reclamaciones de clientes o reguladores.
- No tiene un responsable identificable cuando falla.
- Puede operar con sesgos no detectados indefinidamente.
- No puede ser revertido si produce resultados incorrectos.

### Riesgo 3: Exposición Regulatoria

El EU AI Act y otras normativas exigen que las organizaciones **conozcan y controlen** los sistemas de IA que usan. La ignorancia del Shadow AI no es una defensa legal válida: la responsabilidad recae en la organización, no en el empleado que adoptó la herramienta.

### Riesgo 4: Deuda Técnica y Operacional

Los modelos no gobernados eventualmente:

- Se convierten en dependencias críticas sin soporte formal.
- No se actualizan cuando el proveedor lanza nuevas versiones con cambios de comportamiento.
- Son imposibles de auditar cuando surgen problemas en producción.
- Generan inconsistencias cuando distintos equipos usan versiones diferentes.

---

## Los Tres Pilares

### Pilar 1: IA Segura

**Definición:** los sistemas de IA no introducen vulnerabilidades de seguridad, no exponen datos sensibles y operan dentro de los controles de seguridad corporativos.

**Controles clave:**

- Todos los modelos y herramientas de IA operan dentro del perímetro de seguridad de la organización o bajo contrato con DPA firmado.
- Los datos de entrenamiento y los prompts nunca salen hacia proveedores externos sin clasificación y aprobación previa.
- Las APIs de IA están protegidas con autenticación, autorización y rate limiting.
- Los modelos de lenguaje están configurados para no retener datos entre sesiones.
- Existe un proceso de evaluación de seguridad (penetration testing, prompt injection testing) para modelos de IA expuestos a usuarios externos.

**Clasificación de datos en interacciones con IA:**

| Nivel | Puede enviarse a IA externa | Puede usarse en IA interna | Ejemplos |
|-------|----------------------------|---------------------------|----------|
| **Público** | ✅ Sí | ✅ Sí | Documentación pública, noticias |
| **Interno** | ⚠️ Solo con DPA aprobado | ✅ Sí | Políticas internas, comunicaciones generales |
| **Confidencial** | ❌ No | ✅ Solo en entornos aprobados | Estrategia, datos financieros, código propietario |
| **Restringido** | ❌ No | ❌ Solo con cifrado y control de acceso | Datos de clientes, datos personales, secretos comerciales |

### Pilar 2: IA Confiable

**Definición:** los sistemas de IA producen resultados consistentes, predecibles y alineados con los objetivos de negocio; su comportamiento puede ser explicado y validado.

**Controles clave:**

- Cada modelo en producción tiene una **línea base de rendimiento** documentada.
- Existen pruebas de regresión automatizadas que se ejecutan antes de cada actualización.
- Los modelos de alto impacto tienen evaluaciones periódicas de sesgo y equidad.
- Los resultados de IA que afectan decisiones relevantes tienen un mecanismo de revisión humana.
- Se mantiene un historial versionado de modelos para permitir rollback.

**Dimensiones de confiabilidad:**

```
Confiabilidad = Precisión × Consistencia × Explicabilidad × Equidad × Resiliencia
```

- **Precisión**: el modelo produce resultados correctos en los casos esperados.
- **Consistencia**: el mismo input produce outputs equivalentes en el tiempo.
- **Explicabilidad**: se puede justificar por qué el modelo produjo un resultado.
- **Equidad**: el modelo no discrimina injustificadamente entre grupos.
- **Resiliencia**: el modelo mantiene su comportamiento ante inputs adversariales o inesperados.

### Pilar 3: IA Auditada

**Definición:** existe un registro completo y verificable de qué modelos existen, quién los usa, qué datos procesan, qué decisiones toman y cómo evolucionan en el tiempo.

**Controles clave:**

- **Inventario de modelos**: registro centralizado de todos los sistemas de IA activos.
- **Logs de uso**: registro de interacciones con modelos de alto riesgo (quién, cuándo, qué prompt, qué resultado).
- **Trazabilidad de datos**: linaje desde los datos de entrenamiento hasta las decisiones en producción.
- **Registro de cambios**: histórico de versiones, actualizaciones y cambios de comportamiento.
- **Reportes periódicos**: informes de rendimiento, deriva e incidentes para el Comité de IA.
- **Auditorías externas**: para modelos críticos, revisión por terceros independientes.

**Log mínimo de auditoría por interacción:**

```json
{
  "timestamp": "2026-06-18T10:34:21Z",
  "model_id": "modelo-clasificacion-credito-v2.1",
  "user_id": "hash_anonimizado",
  "input_hash": "sha256_del_prompt_o_features",
  "output": "APROBADO",
  "confidence": 0.87,
  "data_classification": "restringido",
  "human_review_required": false,
  "session_id": "uuid"
}
```

---

## Marco de Control Anti-Shadow AI

### Capa 1: Prevención (dificultar el Shadow AI)

- **Política de uso de IA** publicada y firmada por todos los empleados.
- **Bloqueo de red** a herramientas de IA no aprobadas en dispositivos corporativos.
- **DLP (Data Loss Prevention)** configurado para detectar patrones de datos sensibles enviados a dominios de IA externos.
- **Gestión de identidades**: las herramientas de IA aprobadas están integradas con SSO corporativo.
- **Cláusulas contractuales**: los contratos con proveedores y contratistas incluyen prohibición de Shadow AI.

### Capa 2: Habilitación (hacer el Shadow AI innecesario)

La mejor prevención es ofrecer herramientas mejores y más accesibles que las externas.

- **Catálogo de herramientas de IA aprobadas**: listado actualizado de soluciones disponibles para distintos casos de uso.
- **Entorno de experimentación**: sandbox aprobado donde los equipos pueden probar modelos de IA sin riesgo de exposición de datos reales.
- **Proceso ágil de incorporación**: tiempo máximo de evaluación de nuevas herramientas de IA: **15 días hábiles** para riesgo bajo/medio.
- **Soporte interno de IA**: equipo o canal de consulta para ayudar a los empleados a encontrar la herramienta correcta.

### Capa 3: Detección (encontrar el Shadow AI existente)

- **Auditoría de aplicaciones SaaS**: revisión trimestral de herramientas en uso no registradas en el inventario oficial.
- **Análisis de tráfico de red**: detección de llamadas a APIs de IA externas no aprobadas.
- **Revisión de gastos**: monitoreo de tarjetas corporativas y reembolsos por suscripciones a herramientas de IA.
- **Encuestas de uso**: encuesta anual a empleados sobre herramientas de IA que utilizan en su trabajo.
- **Revisión de código**: en repositorios corporativos, detección de imports/llamadas a APIs de IA no aprobadas.

### Capa 4: Respuesta (actuar ante Shadow AI detectado)

El proceso de respuesta no es punitivo — es correctivo:

```
Detección de Shadow AI
        ↓
Evaluación de riesgo (¿se expusieron datos sensibles?)
        ↓
Notificación al empleado y su manager (sin sanción automática)
        ↓
Análisis conjunto: ¿qué necesidad resolvía la herramienta?
        ↓
¿Existe alternativa aprobada? → Migración asistida
¿No existe? → Inicio de proceso de incorporación acelerado
        ↓
Documentación del incidente y lecciones aprendidas
        ↓
Si hubo exposición de datos sensibles → Activar protocolo de incidente de seguridad
```

---

## Arquitectura de IA Corporativa Aprobada

### Capas de la Arquitectura

```
┌─────────────────────────────────────────────────────────────────┐
│                     USUARIOS Y APLICACIONES                      │
│         (acceso vía interfaces aprobadas con SSO)               │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│                    API GATEWAY DE IA                             │
│   (autenticación, autorización, rate limiting, logging)         │
└─────────────────────────┬───────────────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          │               │               │
┌─────────▼──────┐ ┌──────▼──────┐ ┌─────▼───────────┐
│  MODELOS       │ │  MODELOS    │ │  MODELOS        │
│  PROPIOS       │ │  OPEN SOURCE│ │  DE PROVEEDOR   │
│  (fine-tuned)  │ │  (on-prem)  │ │  (API con DPA)  │
└─────────┬──────┘ └──────┬──────┘ └─────┬───────────┘
          └───────────────┼───────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│                    PLATAFORMA DE MLOps                           │
│   (versionado, monitoreo, experimentos, registro de modelos)    │
└─────────────────────────┬───────────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────────┐
│               CAPA DE DATOS CORPORATIVA                         │
│   (data lake / warehouse con control de acceso y clasificación) │
└─────────────────────────────────────────────────────────────────┘
```

### Principios Arquitectónicos

- **Single entry point**: todas las interacciones con IA pasan por el API Gateway corporativo.
- **Zero trust para IA**: ningún modelo tiene acceso a datos por defecto; el acceso se concede explícitamente.
- **Separación de entornos**: desarrollo, staging y producción completamente aislados.
- **Datos de producción nunca en entrenamiento directo**: los datos reales se anonimizan o sintetizan antes de usarse en entrenamiento.
- **Portabilidad**: evitar lock-in con un único proveedor de modelos; arquitectura multi-modelo.

---

## Proceso de Incorporación de Herramientas de IA

Todo empleado o equipo que identifique una herramienta de IA que desee usar debe seguir este proceso:

### Formulario de Solicitud (mínimo requerido)

```markdown
## Solicitud de Incorporación de Herramienta de IA

**Herramienta solicitada:** [Nombre y versión]
**Solicitante:** [Nombre, equipo, manager]
**Caso de uso:** [Descripción en 3-5 oraciones]
**Datos que procesará:** [Tipo y clasificación de datos]
**Alternativa interna evaluada:** [¿Se revisó el catálogo? ¿Por qué no aplica?]
**Frecuencia de uso estimada:** [Ocasional / Diario / Integrado en flujo de trabajo]
**Urgencia:** [Normal (15 días) / Urgente con justificación]
```

### Criterios de Evaluación

| Criterio | Preguntas clave |
|----------|-----------------|
| **Seguridad** | ¿Tiene DPA? ¿Dónde se almacenan los datos? ¿Cumple ISO 27001 o SOC 2? |
| **Privacidad** | ¿Cumple GDPR/LGPD? ¿Los datos se usan para reentrenamiento? |
| **Confiabilidad** | ¿El proveedor tiene SLA? ¿Qué pasa si el servicio cae? |
| **Legal** | ¿Los outputs pueden tener derechos de autor de terceros? ¿Hay riesgo de infracción IP? |
| **Negocio** | ¿El caso de uso genera valor real? ¿El costo está justificado? |
| **Alternativa** | ¿Existe una opción interna equivalente? |

### Tiempos de Respuesta por Riesgo

| Nivel de riesgo | Tiempo máximo de evaluación |
|----------------|-----------------------------|
| Bajo | 5 días hábiles |
| Medio | 15 días hábiles |
| Alto | 30 días hábiles + aprobación del Comité de IA |
| Crítico | 45 días + auditoría externa requerida |

---

## Detección y Respuesta ante Shadow AI

### Señales de Alerta Temprana

Estas señales no son prueba definitiva de Shadow AI, pero ameritan investigación:

- Incremento inusual en tráfico HTTPS hacia dominios de IA conocidos no aprobados.
- Aparición de suscripciones personales a herramientas de IA en reembolsos de gastos.
- Código en repositorios corporativos con llamadas a APIs de IA externas no registradas.
- Outputs de documentos con marcas de agua o disclaimers de herramientas de IA externas.
- Empleados que mencionan en reuniones herramientas de IA que no están en el catálogo aprobado.

### Dominios y Servicios a Monitorear

Ejemplos de dominios frecuentes en Shadow AI (lista no exhaustiva, actualizar periódicamente):

```
api.openai.com
api.anthropic.com
generativelanguage.googleapis.com
api.mistral.ai
api.cohere.com
huggingface.co/api
```

> **Nota:** monitorear no significa bloquear automáticamente. El objetivo es tener visibilidad y facilitar la incorporación formal de herramientas que generen valor real.

### Matriz de Respuesta por Severidad

| Severidad | Criterio | Respuesta |
|-----------|----------|-----------|
| **Baja** | Herramienta sin DPA, sin datos sensibles | Conversación con el empleado, redirigir al catálogo |
| **Media** | Herramienta sin DPA, con datos internos | Notificación a manager, evaluación de riesgo, proceso de incorporación |
| **Alta** | Herramienta con datos confidenciales/restringidos | Activar protocolo de seguridad, notificación a DPO, evaluación de breach |
| **Crítica** | Modelo propio entrenado con datos de producción sin proceso formal | Detención inmediata, evaluación legal, notificación ejecutiva |

---

## Cultura: La Raíz del Problema

### Por qué los controles técnicos no bastan

Un empleado motivado siempre encontrará una forma de eludir controles técnicos: usar su teléfono personal, su red doméstica, una cuenta personal. La solución duradera es **cultural**.

### Los tres mensajes que debe interiorizar toda la organización

**1. "Entiendo el riesgo"**
Los datos que pego en una herramienta de IA externa pueden salir del control de la empresa. Si son datos de clientes, puedo estar violando la confianza que ellos depositaron en nosotros y las leyes que nos obligan a protegerlos.

**2. "Tengo alternativas"**
Existe un catálogo de herramientas aprobadas. Si lo que necesito no está, hay un proceso rápido para pedirlo. Mi organización quiere que use IA — solo quiere hacerlo de forma segura.

**3. "Reportar es lo correcto"**
Si descubro que un compañero o yo mismo hemos estado usando una herramienta no aprobada, reportarlo no es delatar — es proteger al equipo. La respuesta de la organización es ayudar a regularizar, no sancionar automáticamente.

### Programa de Concientización

| Acción | Frecuencia | Audiencia |
|--------|-----------|-----------|
| Módulo de formación "Uso responsable de IA" | Onboarding + anual | Todos los empleados |
| Comunicación del catálogo de herramientas aprobadas | Trimestral | Todos los empleados |
| Taller de casos prácticos (qué sí, qué no) | Semestral | Equipos técnicos y de negocio |
| Reporte de transparencia de IA | Anual | Toda la organización |
| Canal de consulta rápida | Permanente | Todos los empleados |

---

## Indicadores de Madurez

### KPIs del Programa Anti-Shadow AI

| Indicador | Métrica | Objetivo |
|-----------|---------|----------|
| **Cobertura del inventario** | % de herramientas de IA en uso que están registradas | >95% |
| **Tiempo de incorporación** | Días promedio desde solicitud hasta aprobación | <15 días (riesgo bajo/medio) |
| **Incidentes de Shadow AI** | Número de casos detectados por trimestre | Tendencia decreciente |
| **Formación completada** | % de empleados con formación de IA al día | >90% |
| **Modelos con monitoreo activo** | % de modelos en producción con monitoreo automático | 100% |
| **Logs de auditoría disponibles** | % de interacciones de IA de alto riesgo con log completo | 100% |
| **Solicitudes pendientes** | Solicitudes de herramientas de IA sin respuesta en plazo | 0 |

---

## Hoja de Ruta de Implementación

### Fase 1: Visibilidad (meses 1-2)

El primer paso es entender qué existe hoy, sin juzgar.

- Auditoría de herramientas de IA actualmente en uso (encuesta + análisis de red + revisión de gastos).
- Inventario inicial de modelos y herramientas de IA en producción.
- Mapa de riesgos del estado actual.
- Publicación de la política de uso de IA (versión 1.0, comunicación no punitiva).

### Fase 2: Control (meses 3-4)

- Lanzamiento del catálogo de herramientas aprobadas.
- Implementación del proceso de incorporación de nuevas herramientas.
- Configuración de DLP y monitoreo de red.
- Formación obligatoria para todos los empleados.
- Proceso de regularización para herramientas detectadas en fase 1.

### Fase 3: Gobierno (meses 5-8)

- Implementación del API Gateway de IA corporativo.
- Integración de herramientas aprobadas con SSO corporativo.
- Sistema de monitoreo continuo de modelos en producción.
- Primer ciclo de auditoría interna del programa.
- Publicación del primer reporte de transparencia de IA.

### Fase 4: Optimización (meses 9-12)

- Automatización de controles de detección de Shadow AI.
- Métricas de madurez y benchmark externo.
- Revisión y actualización de políticas basada en aprendizajes.
- Evaluación de auditoría externa para modelos críticos.
- Integración del gobierno de IA en el proceso de desarrollo (AI-aware CI/CD).

---

## Síntesis: Las 10 Reglas de Oro

1. **Toda herramienta de IA debe estar en el inventario** — si no se conoce, no se puede proteger.
2. **Los datos clasificados como confidenciales o restringidos nunca salen de la organización** para ser procesados por IA externa.
3. **Ningún modelo entra a producción sin aprobación formal** y Model Card documentado.
4. **Toda interacción de IA de alto riesgo genera un log** de auditoría completo e inmutable.
5. **El catálogo de herramientas aprobadas se actualiza en máximo 15 días hábiles** tras una solicitud.
6. **El Shadow AI se regulariza, no se penaliza automáticamente** — la cultura de reporte abierto es prioritaria.
7. **Los modelos en producción se monitorean continuamente** — el despliegue no es el final del proceso.
8. **Toda decisión de IA de alto impacto tiene una revisión humana** disponible.
9. **Los proveedores de IA deben firmar DPA** antes de procesar cualquier dato corporativo.
10. **El gobierno de IA es responsabilidad de todos** — no solo del equipo de tecnología.

---

## Referencias

- [EU AI Act — Reglamento (UE) 2024/1689](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689)
- [NIST AI RMF 1.0](https://airc.nist.gov/RMF) — Marco de gestión de riesgos de IA
- [ISO/IEC 42001:2023](https://www.iso.org/standard/81230.html) — Sistema de gestión de IA
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — Riesgos de seguridad en LLMs
- [ENISA AI Cybersecurity Risks](https://www.enisa.europa.eu/publications/artificial-intelligence-cybersecurity-challenges) — Riesgos de ciberseguridad en IA
- [Gartner: Managing Shadow AI Risk](https://www.gartner.com) — Investigación sobre gestión de Shadow AI

---

*Este documento debe revisarse cada 6 meses. Los cambios en herramientas aprobadas, proveedores o regulaciones deben reflejarse en una nueva versión.*
