# Privacidad de Datos

Estándares para el tratamiento responsable de datos personales en todos los sistemas de la organización.

 [WIP]
---

## Principios de privacidad

**Minimización de datos:** recopilar solo los datos estrictamente necesarios para el propósito declarado. Si no se usa, no se recopila.

**Limitación de propósito:** los datos recopilados para un fin no pueden usarse para otro sin consentimiento explícito.

**Exactitud:** mantener los datos actualizados. Proveer mecanismos para que los usuarios corrijan su información.

**Limitación del plazo de conservación:** eliminar los datos cuando ya no sean necesarios para el propósito original.

**Privacidad por diseño:** incorporar la privacidad desde la fase de diseño, no como capa adicional posterior.

---

## Clasificación de datos personales

| Categoría | Ejemplos | Controles adicionales requeridos |
|---|---|---|
| **Datos de identificación** | Nombre, RUT, correo, teléfono | Cifrado en tránsito y reposo |
| **Datos financieros** | Números de cuenta, historial de pagos | Cifrado a nivel de campo, acceso auditado |
| **Datos sensibles** | Salud, religión, origen étnico, biometría | Cifrado fuerte, base legal explícita, acceso muy restringido |
| **Datos de comportamiento** | Navegación, clicks, historial de uso | Anonimización para analítica, retención limitada |

---


## Retención y eliminación de datos

[WIP]

Cada tipo de dato debe tener una política de retención documentada:

| Tipo de dato | Retención sugerida |
|---|---|
| Logs de aplicación | 90 días |
| Logs de seguridad y auditoría | 12 meses |
| Datos de transacciones | 5 años (requisito tributario Chile) |
| Datos de sesión de usuario | 30 días de inactividad |
| Datos de soporte y tickets | 3 años |

La política de retención de cada servicio debe estar en su documentación técnica.

---

## Notificación de brechas de seguridad

[WIP]

Si se detecta una brecha que afecta datos personales:

1. Notificar al responsable de privacidad en las primeras **2 horas**.
2. Evaluar el alcance e impacto en las siguientes **12 horas**.
3. Si la brecha es significativa, notificar a los titulares afectados en un plazo máximo de **72 horas**.
4. Documentar el incidente en el registro de brechas.
