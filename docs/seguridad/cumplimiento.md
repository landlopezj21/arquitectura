# Cumplimiento Normativo

Normativas aplicables a los sistemas de la organización y cómo cumplirlas.

[WIP]
---

## Marco normativo aplicable (Chile)

### Ley 19.628 — Protección de Datos Personales

Marco vigente para el tratamiento de datos personales en Chile.

**Obligaciones principales:**
- Contar con base legal para el tratamiento (consentimiento, ley, contrato).
- Informar a los titulares sobre el tratamiento de sus datos.
- Garantizar los derechos ARCO (Acceso, Rectificación, Cancelación, Oposición).
- Adoptar medidas de seguridad apropiadas al tipo de datos.

**Estado:** en proceso de modernización. La nueva ley amplía los derechos y fortalece las sanciones. Monitorear avance legislativo.

---

### Ley 21.663 — Ley Marco de Ciberseguridad (2024)

Establece obligaciones para operadores de servicios esenciales e infraestructura crítica. Crea la Agencia Nacional de Ciberseguridad (ANCI).

**Aplica a la organización si:**
- Opera servicios de telecomunicaciones, energía, salud, banca, transporte o infraestructura digital.
- Es identificada por la ANCI como operador de importancia vital (OIV).

**Obligaciones para OIVs:**
- Implementar medidas de seguridad según estándar ANCI.
- Reportar incidentes de ciberseguridad significativos dentro de 24 horas.
- Designar un responsable de ciberseguridad.
- Someterse a auditorías periódicas.

---

### Requisitos tributarios — SII

**Retención de datos:** los registros contables y de transacciones deben conservarse mínimo **6 años** según normativa del SII.

Esto afecta la política de retención de datos en sistemas de facturación, pagos y contabilidad.

---

## Buenas prácticas y marcos de referencia adoptados

Aunque no son obligatorios por ley, la organización adopta los siguientes marcos como guía:

| Marco | Área de aplicación |
|---|---|
| **NIST Cybersecurity Framework (CSF)** | Gestión de riesgos de ciberseguridad |
| **ISO 27001** | Sistema de gestión de seguridad de la información |
| **OWASP Top 10** | Seguridad de aplicaciones web |
| **CIS Benchmarks** | Hardening de infraestructura |

---

## Checklist de cumplimiento por sistema

Al lanzar un nuevo sistema, verificar:

**Privacidad y datos personales**
- [ ] Se identificaron todos los datos personales que el sistema procesa.
- [ ] Se documentó la base legal para cada tipo de tratamiento.
- [ ] Se implementaron los mecanismos para ejercer derechos ARCO.
- [ ] Se definió la política de retención y eliminación de datos.
- [ ] Se realizó DPIA si aplica (ver [Privacidad de Datos](privacidad.md)).

**Ciberseguridad**
- [ ] Se aplicaron los controles de seguridad según el nivel del sistema.
- [ ] Se configuró el logging de eventos de seguridad.
- [ ] Se realizó revisión de seguridad antes del lanzamiento.
- [ ] Se definió el procedimiento de notificación de incidentes.

**Operaciones**
- [ ] Se definieron los SLOs del sistema.
- [ ] Se configuró monitoreo y alertas.
- [ ] Se documentó el runbook operacional.
- [ ] Hay al menos un responsable de guardia (on-call) definido.

---

## Auditorías y revisiones periódicas

| Actividad | Frecuencia | Responsable |
|---|---|---|
| Revisión de permisos y accesos | Trimestral | Cada equipo |
| Auditoría de logs de seguridad | Mensual | Seguridad |
| Revisión de cumplimiento normativo | Anual | Legal + Arquitectura |
| Renovación de certificados TLS | Automática (Let's Encrypt) o 90 días antes del vencimiento | DevOps |
| Test de penetración | Anual | Proveedor externo |
