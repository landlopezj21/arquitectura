# Plantilla ADR — Architecture Decision Record

Copia esta plantilla para documentar cada decisión arquitectónica significativa. Guarda el archivo como `ADR-NNN-titulo-corto.md` en la carpeta `docs/arquitectura/adr/`.

---

```markdown
# ADR-NNN — Título corto de la decisión

**Estado:** [Propuesto | En revisión | Aprobado | Rechazado | Obsoleto]
**Fecha:** YYYY-MM-DD
**Autores:** Nombre Apellido, Nombre Apellido
**Revisores:** Nombre Apellido (Comité de Arquitectura)
**ADRs relacionados:** ADR-XXX (si aplica)

---

## Contexto

Describe el problema o situación que motiva esta decisión. Incluye las restricciones
técnicas, de negocio u operacionales relevantes. Sé específico: un buen contexto
permite entender la decisión años después sin conocer el historial del equipo.

Ejemplo: "El servicio de pagos actualmente procesa 50 transacciones/segundo. Las
proyecciones de negocio indican que alcanzará 500 TPS en 12 meses. La arquitectura
actual no escala horizontalmente debido al uso de sesiones en memoria."

---

## Opciones consideradas

### Opción 1 — Nombre de la opción

Descripción breve de la opción.

**Ventajas:**
- Ventaja 1
- Ventaja 2

**Desventajas:**
- Desventaja 1
- Desventaja 2

---

### Opción 2 — Nombre de la opción

Descripción breve de la opción.

**Ventajas:**
- Ventaja 1

**Desventajas:**
- Desventaja 1

---

## Decisión

**Se elige la Opción N — [Nombre].**

Explica brevemente el razonamiento principal de la elección y por qué las otras
opciones fueron descartadas.

---

## Consecuencias

### Positivas
- Consecuencia positiva esperada 1
- Consecuencia positiva esperada 2

### Negativas / Compromisos asumidos
- Consecuencia negativa o trade-off 1
- Consecuencia negativa o trade-off 2

### Acciones de seguimiento
- [ ] Tarea concreta que debe realizarse como resultado de esta decisión
- [ ] Tarea concreta 2

---

## Referencias

- [Enlace a documentación relevante](https://ejemplo.com)
- [Enlace a ticket o issue relacionado](https://jira.ejemplo.com/PROJ-123)
```
