# Estrategia de Ramas

La organización usa **GitHub Flow** como estrategia base: una rama principal (`main`) siempre desplegable, y ramas de corta duración para cada cambio.

---

## Reglas generales

- `main` es la rama de producción. Siempre debe estar en estado desplegable.
- Todo cambio llega a `main` mediante Pull Request con al menos una aprobación.
- Las ramas de trabajo deben vivir menos de 3 días. Si una rama dura más, es señal de que el cambio es demasiado grande y debe dividirse.
- No hacer commits directos a `main`. Sin excepciones.

---

## Nomenclatura de ramas

```
<tipo>/<ticket>-descripcion-corta
```

| Tipo | Cuándo usarlo | Ejemplo |
|---|---|---|
| `feat` | Nueva funcionalidad | `feat/PROJ-123-login-con-google` |
| `fix` | Corrección de bug | `fix/PROJ-456-null-pointer-checkout` |
| `chore` | Tareas técnicas, dependencias | `chore/actualizar-node-20` |
| `docs` | Solo documentación | `docs/agregar-adr-kafka` |
| `refactor` | Refactorización sin cambio de comportamiento | `refactor/PROJ-789-extraer-servicio-pagos` |
| `hotfix` | Corrección urgente en producción | `hotfix/PROJ-999-error-critico-pagos` |

---

## Flujo de trabajo

```
main
 │
 ├─── feat/PROJ-123-nueva-feature
 │         │
 │         ├── commits pequeños y frecuentes
 │         └── Pull Request → revisión → merge a main
 │
 └─── fix/PROJ-456-bug-critico
           │
           └── Pull Request → revisión → merge a main
```

### Paso a paso

1. Crear rama desde `main` actualizado.
2. Hacer commits pequeños con mensajes descriptivos (ver Conventional Commits abajo).
3. Abrir Pull Request tan pronto como haya código para revisar (puede ser borrador).
4. Obtener al menos una aprobación del equipo.
5. Pasar todos los checks de CI.
6. Hacer squash merge o merge commit según la política del repositorio.
7. Eliminar la rama tras el merge.

---

## Mensajes de commit — Conventional Commits

Todos los commits deben seguir el estándar [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>[ámbito opcional]: <descripción en imperativo>

[cuerpo opcional]

[pie opcional: referencias a tickets]
```

### Ejemplos

```
feat(pagos): agregar soporte para pago con transferencia bancaria

Se implementa el flujo de pago por transferencia usando el proveedor
Khipu. Incluye webhook de confirmación y notificación al usuario.

Closes PROJ-234
```

```
fix(auth): corregir validación de token expirado

El middleware no verificaba correctamente la fecha de expiración
en tokens generados en zona horaria UTC-3.

Fixes PROJ-567
```

```
chore: actualizar dependencias de seguridad

- express 4.18.2 → 4.19.2 (CVE-2024-29041)
- axios 1.6.7 → 1.7.2
```

### Tipos permitidos

| Tipo | Descripción |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `docs` | Solo documentación |
| `style` | Formato, sin cambio de lógica |
| `refactor` | Refactorización |
| `test` | Agregar o corregir pruebas |
| `chore` | Build, dependencias, configuración |
| `perf` | Mejora de rendimiento |
| `ci` | Cambios en pipelines CI/CD |

---

## Hotfixes

Para correcciones urgentes en producción:

1. Crear rama `hotfix/PROJ-NNN-descripcion` desde `main`.
2. Aplicar el fix con prueba de regresión.
3. Abrir PR con etiqueta `hotfix` — revisión expedita (máximo 2 horas).
4. Merge a `main` y despliegue inmediato.
