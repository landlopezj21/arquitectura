# Modelo de Gobierno de Inteligencia Artificial

[WIP]
---

## ¿Qué es el Gobierno de IA?

El gobierno de IA es el conjunto de **políticas, procesos, roles y controles** que una organización establece para garantizar que sus sistemas de inteligencia artificial sean desarrollados, desplegados y operados de manera responsable, confiable y alineada con sus objetivos estratégicos y obligaciones regulatorias.

No es un proyecto de una sola vez: es una **capacidad organizacional continua** que evoluciona junto con la tecnología, los riesgos y el marco regulatorio.

---

## ¿Por qué es necesario?

Los sistemas de IA presentan riesgos únicos que los modelos de gobierno TI tradicionales no cubren:

- **Opacidad**: los modelos de IA (especialmente los de aprendizaje profundo) son difíciles de interpretar.
- **Deriva de datos**: el comportamiento del modelo cambia cuando los datos del mundo real evolucionan.
- **Sesgo algorítmico**: los modelos pueden perpetuar o amplificar discriminaciones presentes en los datos de entrenamiento.
- **Impacto a escala**: un error en un sistema de IA puede afectar a millones de personas simultáneamente.
- **Presión regulatoria**: marcos como el EU AI Act, ISO 42001 y NIST AI RMF exigen controles formales.

---

## Los Cinco Pilares del Gobierno de IA

### 1. Estrategia y Alineación

El gobierno de IA debe estar anclado a la estrategia del negocio. Esto implica:

- Definir el **apetito de riesgo en IA** de la organización.
- Establecer qué casos de uso de IA están permitidos, restringidos o prohibidos.
- Alinear los principios de IA con los valores corporativos (ej. privacidad, equidad, sostenibilidad).
- Designar un **responsable de IA** a nivel ejecutivo (Chief AI Officer o equivalente).

### 2. Estructura de Gobernanza y Roles

Una gobernanza efectiva requiere responsabilidades claras distribuidas en tres líneas de defensa:

| Línea | Actores | Responsabilidad |
|-------|---------|-----------------|
| **1ª línea** | Equipos de datos, ML e ingeniería | Implementar controles en el desarrollo y operación de modelos |
| **2ª línea** | Riesgo, cumplimiento, ética, legal | Establecer políticas, supervisar y auditar internamente |
| **3ª línea** | Auditoría interna / externa | Validar de forma independiente el cumplimiento del modelo de gobierno |

**Comité de IA**: órgano transversal (tecnología, negocio, legal, riesgo) que revisa y aprueba casos de uso de alto riesgo antes del despliegue.

### 3. Gestión del Ciclo de Vida de Modelos (MLOps Governance)

El gobierno debe cubrir todas las fases del ciclo de vida de un modelo de IA:

```
Ideación → Diseño → Datos → Entrenamiento → Evaluación → Despliegue → Monitoreo → Retiro
    ↑                                                                               ↓
    └───────────────────── Retroalimentación continua ──────────────────────────────┘
```

**Controles clave por fase:**

- **Ideación**: evaluación de viabilidad ética y regulatoria antes de iniciar.
- **Datos**: trazabilidad de origen, calidad, consentimiento y privacidad.
- **Entrenamiento**: reproducibilidad, versionado y detección de sesgo.
- **Evaluación**: métricas de rendimiento, equidad y pruebas adversariales.
- **Despliegue**: aprobación formal (model card, AI fact sheet), registro en inventario.
- **Monitoreo**: alertas de deriva de datos/concepto, indicadores de equidad en producción.
- **Retiro**: procedimiento formal de descontinuación y archivo de artefactos.

### 4. Gestión de Riesgos y Ética

**Clasificación de riesgo**: no todos los sistemas de IA requieren el mismo nivel de control. La clasificación debe hacerse antes del desarrollo:

| Nivel de Riesgo | Características | Ejemplos | Controles requeridos |
|-----------------|-----------------|----------|----------------------|
| **Crítico** | Decisiones con impacto en derechos, seguridad o finanzas | Crédito, diagnóstico médico, contratación | Revisión humana obligatoria, auditoría externa, explicabilidad |
| **Alto** | Impacto significativo en grupos de personas | Personalización de precios, scoring de clientes | Evaluación de sesgo, aprobación del Comité de IA |
| **Medio** | Automatización de procesos internos | Clasificación de tickets, resúmenes automáticos | Pruebas de rendimiento, monitoreo básico |
| **Bajo** | Apoyo a decisiones sin impacto directo | Recomendaciones de contenido interno | Documentación mínima, revisión periódica |

**Principios éticos mínimos:**

- **Explicabilidad**: capacidad de justificar las decisiones del modelo de forma comprensible.
- **Equidad**: ausencia de discriminación injustificada por género, etnia, edad u otro atributo protegido.
- **Privacidad**: cumplimiento de GDPR, LGPD u otras normativas aplicables desde el diseño.
- **Seguridad**: resistencia a ataques adversariales, envenenamiento de datos y extracción de modelos.
- **Supervisión humana**: toda decisión de alto impacto debe tener un punto de revisión o apelación humana.

### 5. Cumplimiento Regulatorio y Marcos de Referencia

El panorama regulatorio de IA está evolucionando rápidamente. Los marcos más relevantes son:

| Marco | Alcance | Aspectos clave |
|-------|---------|----------------|
| **EU AI Act (2024)** | Regulación vinculante en la UE | Clasificación de riesgo, prohibiciones, requisitos para IA de alto riesgo |
| **ISO/IEC 42001** | Internacional (certificable) | Sistema de gestión de IA: políticas, objetivos, mejora continua |
| **NIST AI RMF** | EE.UU. (voluntario) | Marco de gestión de riesgo: Gobernar, Mapear, Medir, Gestionar |
| **OCDE Principios de IA** | Internacional (voluntario) | Principios de IA confiable adoptados por más de 40 países |
| **ISO/IEC 23894** | Internacional | Guía de gestión de riesgos específica para IA |

---

## Componentes Operativos Esenciales

### Inventario de Modelos

Un registro centralizado de todos los modelos de IA en producción, que incluya para cada uno:

- Propietario del negocio y propietario técnico.
- Clasificación de riesgo y fecha de última evaluación.
- Datos de entrenamiento utilizados (origen, fecha de corte).
- Métricas de rendimiento actuales vs. línea base.
- Fecha de próxima revisión y fecha de retiro planificada.

### Model Card / AI Fact Sheet

Documento estándar que acompaña a cada modelo desplegado:

- **Descripción del modelo**: propósito, arquitectura, versión.
- **Datos**: fuentes, período, transformaciones aplicadas.
- **Rendimiento**: métricas por segmento de población, casos de falla conocidos.
- **Limitaciones**: qué no puede hacer el modelo, en qué contextos no debe usarse.
- **Consideraciones éticas**: evaluación de sesgo, impacto en grupos vulnerables.
- **Aprobaciones**: firmas de las partes que autorizaron el despliegue.

### Proceso de Aprobación (AI Gate Review)

Antes del despliegue de modelos de riesgo medio o superior, se requiere:

```
Solicitud de despliegue
        ↓
Completar Model Card + evaluación de riesgo
        ↓
Revisión técnica (arquitectura, seguridad, calidad de datos)
        ↓
Revisión de cumplimiento y ética
        ↓
Aprobación del Comité de IA (riesgo alto/crítico)
        ↓
Despliegue con monitoreo activo
```

### Monitoreo Continuo

Los modelos en producción deben monitorearse en cuatro dimensiones:

- **Rendimiento**: precisión, recall, latencia, disponibilidad.
- **Deriva de datos** (*data drift*): cambio en la distribución de los datos de entrada.
- **Deriva de concepto** (*concept drift*): cambio en la relación entre variables de entrada y salida.
- **Equidad**: evolución de métricas de equidad entre grupos a lo largo del tiempo.

Umbrales de alerta deben estar definidos y generar tickets automáticos cuando se superan.

---

## Cultura y Capacitación

El gobierno de IA no puede sostenerse solo con controles técnicos. Requiere:

- **Formación obligatoria** en ética de IA para todos los roles que desarrollan, aprueban o usan sistemas de IA.
- **Programa de concienciación** para el resto de la organización.
- **Canal de reporte** para empleados que identifiquen usos indebidos o comportamientos inesperados en sistemas de IA.
- **Comunidad de práctica**: espacio de intercambio entre equipos de datos, legal, riesgo y negocio.

---

## Madurez del Gobierno de IA

Las organizaciones suelen evolucionar a través de cinco niveles:

| Nivel | Nombre | Descripción |
|-------|--------|-------------|
| 1 | **Inicial** | Sin procesos formales; el gobierno depende de personas clave |
| 2 | **Repetible** | Políticas básicas documentadas; aplicadas de forma inconsistente |
| 3 | **Definido** | Procesos estandarizados, inventario de modelos, revisiones formales |
| 4 | **Gestionado** | Monitoreo cuantitativo, métricas de gobierno, mejora continua |
| 5 | **Optimizado** | Gobierno automatizado, integrado en CI/CD, benchmarking externo |

La mayoría de las organizaciones se encuentra entre los niveles 1 y 3. El objetivo a mediano plazo debe ser alcanzar el nivel 3 como mínimo antes de escalar el uso de IA en decisiones de alto impacto.

---

## Hoja de Ruta Recomendada (12 meses)

| Trimestre | Acciones prioritarias |
|-----------|----------------------|
| **Q1** | Definir principios de IA, designar responsable ejecutivo, realizar inventario de modelos existentes |
| **Q2** | Clasificar riesgos del inventario, crear plantilla de Model Card, establecer Comité de IA |
| **Q3** | Implementar proceso de Gate Review, lanzar programa de formación, definir umbrales de monitoreo |
| **Q4** | Auditoría interna del modelo de gobierno, ajuste de políticas, publicar informe de transparencia |

---

## Síntesis Ejecutiva

El gobierno de IA es una **ventaja competitiva y una obligación**, no solo un costo de cumplimiento. Las organizaciones que lo implementan de forma proactiva:

- Reducen el riesgo de incidentes de IA con impacto reputacional o regulatorio.
- Generan confianza en clientes, reguladores y empleados.
- Aceleran la adopción de IA al tener marcos claros que dan certeza a los equipos.
- Están mejor posicionadas para cumplir con marcos regulatorios emergentes.

**El gobierno no frena la innovación; la hace sostenible.**

---

## Referencias

- [EU AI Act](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689) — Reglamento (UE) 2024/1689
- [NIST AI RMF](https://www.nist.gov/system/files/documents/2023/01/26/AI RMF 1.0.pdf) — Marco de gestión de riesgo de IA, NIST 2023
- [ISO/IEC 42001:2023](https://www.iso.org/standard/81230.html) — Sistema de gestión de inteligencia artificial
- [OECD AI Principles](https://oecd.ai/en/ai-principles) — Principios de la OCDE sobre IA confiable
- [Google Model Cards](https://modelcards.withgoogle.com/about) — Referencia para Model Cards
- [Microsoft Responsible AI](https://www.microsoft.com/en-us/ai/responsible-ai) — Estándares de IA responsable

---

*Documento de referencia. Revisar cada 6 meses o ante cambios regulatorios significativos.*
