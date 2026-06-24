# Revisión de Código

El code review es una práctica de calidad, no un mecanismo de control. Su objetivo es encontrar problemas antes de producción, compartir conocimiento y mantener la calidad del código base.

[WIP]
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