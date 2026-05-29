# Revisión de Código

El code review es una práctica de calidad, no un mecanismo de control. Su objetivo es encontrar problemas antes de producción, compartir conocimiento y mantener la calidad del código base.

---

## Reglas del proceso

- Todo PR requiere **al menos una aprobación** de un desarrollador del equipo.
- PRs que afecten infraestructura, seguridad o APIs públicas requieren **aprobación adicional** del área correspondiente.
- El autor es responsable de mantener el PR actualizado y responder comentarios en un plazo máximo de **1 día hábil**.
- Los revisores deben completar su revisión en un plazo máximo de **1 día hábil** desde la solicitud.
- Un PR sin actividad por más de 3 días hábiles puede cerrarse y reabrirse cuando esté listo.

---

## Tamaño de los PRs

| Tamaño | Líneas cambiadas | Tiempo de revisión esperado |
|---|---|---|
| XS | < 50 | < 15 min |
| S | 50–200 | 15–30 min |
| M | 200–500 | 30–60 min |
| L | 500–1000 | > 1 hora — considerar dividir |
| XL | > 1000 | Requiere coordinación previa |

Los PRs grandes son más difíciles de revisar y más propensos a errores. Dividir el trabajo en PRs pequeños es una habilidad a desarrollar.

---

## Qué revisar

### Corrección
- ¿El código hace lo que dice el ticket?
- ¿Maneja correctamente los casos borde y errores?
- ¿Las pruebas cubren los flujos importantes?

### Diseño
- ¿Es coherente con la arquitectura del sistema?
- ¿Sigue los principios y patrones aprobados?
- ¿Hay lógica de negocio en la capa incorrecta?

### Seguridad
- ¿Hay inputs del usuario sin validar o sanitizar?
- ¿Se exponen datos sensibles en logs?
- ¿Las autorizaciones están correctamente implementadas?

### Mantenibilidad
- ¿El código es legible sin necesitar comentarios extensos?
- ¿Hay duplicación que podría extraerse?
- ¿Los nombres son claros y precisos?

---

## Cómo dar feedback

El feedback debe ser específico, constructivo y orientado al código, no a la persona. Usa estas categorías en tus comentarios:

| Prefijo | Significado |
|---|---|
| `[bloqueante]` | Debe resolverse antes del merge |
| `[sugerencia]` | Mejora recomendada, el autor decide |
| `[pregunta]` | Aclaración para entender el código |
| `[nit]` | Detalle menor de estilo, opcional |

### Ejemplo de buen feedback

```
[bloqueante] Este endpoint no verifica que el usuario autenticado tenga
permiso para acceder al recurso solicitado. Un usuario podría acceder
a datos de otro usuario pasando un ID diferente en la URL.

[sugerencia] Considerar extraer esta lógica a un método privado
`calculateDiscount()` — facilitaría escribir pruebas unitarias.

[pregunta] ¿Por qué se elige 30 segundos como timeout aquí?
¿Hay un SLA del servicio externo que justifique ese valor?

[nit] `usersList` podría ser simplemente `users`.
```

---

## Checklist del autor antes de solicitar revisión

- [ ] El código compila y las pruebas pasan localmente.
- [ ] La cobertura de pruebas no bajó respecto a `main`.
- [ ] El linter no reporta errores.
- [ ] El PR tiene una descripción clara: qué cambia y por qué.
- [ ] Se agregó o actualizó documentación si aplica.
- [ ] No hay `console.log`, `print` o código de debug en el PR.
- [ ] Las variables de entorno nuevas están documentadas en `.env.example`.
