# Operaciones y SRE

[WIP]

Esta sección define los estándares para operar sistemas en producción de forma confiable, observable y sostenible.

## Contenido

- [Observabilidad](observabilidad.md) — logs, métricas y trazas.
- [SLOs y SLAs](slos.md) — objetivos de nivel de servicio y cómo definirlos.
- [Gestión de incidentes](incidentes.md) — proceso de respuesta, roles y post-mortem.


## Filosofía operacional

> "Si duele, hazlo más seguido."

Los despliegues frecuentes y pequeños son más seguros que los grandes y espaciados. La automatización reduce el error humano. La observabilidad permite reaccionar antes de que los usuarios lo noten.

Los equipos son responsables de operar los sistemas que construyen. No existe un equipo de operaciones separado que tome la responsabilidad.