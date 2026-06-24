# CI/CD

Todo repositorio debe tener un pipeline automatizado de integración y despliegue continuo. Sin despliegues manuales a producción.

[WIP]
---

## Ambientes

| Ambiente | Propósito | Despliegue | Aprobación |
|---|---|---|---|
| `desarrollo` | Pruebas locales e integración temprana | Manual o automático desde rama feature | Ninguna |
| `staging` | Validación pre-producción, pruebas E2E | Automático al hacer merge a `main` | Ninguna |
| `producción` | Servicio real a usuarios | Automático o manual con aprobación | Requerida (ver abajo) |

---

## Pipeline estándar

Todo pipeline debe tener al menos estas etapas en orden:

[WIP]