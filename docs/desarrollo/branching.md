# Estrategia de Ramas

[WIP]

La organización usa **Azure DevOps** como estrategia base: una rama principal (`main`) siempre desplegable, y ramas de corta duración para cada cambio.

---

## Reglas generales

- `main` es la rama de producción. Siempre debe estar en estado desplegable.
- Todo cambio llega a `main` mediante Pull Request con al menos una aprobación.
