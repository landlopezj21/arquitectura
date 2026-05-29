# Convenciones de Código

Estas convenciones aplican a todos los repositorios de la organización. El objetivo es que cualquier desarrollador pueda leer código de cualquier equipo sin fricción.

---

## Nomenclatura

| Elemento | Convención | Ejemplo |
|---|---|---|
| Variables y funciones | camelCase | `getUserById`, `totalAmount` |
| Clases e interfaces | PascalCase | `OrderService`, `PaymentRepository` |
| Constantes | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_BASE_URL` |
| Archivos de código | kebab-case | `order-service.ts`, `payment-utils.js` |
| Tablas de base de datos | snake_case | `user_orders`, `payment_methods` |
| Topics de Kafka | `dominio.entidad.evento` | `pagos.transaccion.completada` |
| Variables de entorno | UPPER_SNAKE_CASE | `DATABASE_URL`, `JWT_SECRET` |

---

## Formato de código

**Todos los repositorios deben tener un linter y formateador configurado y ejecutándose en CI.**

### TypeScript / JavaScript
- Formateador: **Prettier** con configuración estándar de la organización.
- Linter: **ESLint** con el config compartido `@mi-org/eslint-config`.
- Longitud máxima de línea: 100 caracteres.

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 100,
  "trailingComma": "es5"
}
```

### Python
- Formateador: **Black** con longitud de línea 100.
- Linter: **Ruff**.
- Type hints obligatorios en funciones públicas.

### Java / Kotlin
- Formateador: **Google Java Format** / **ktlint**.
- Checkstyle configurado en el build de Gradle/Maven.

---

## Estructura de proyectos

### Servicio backend (Node.js / TypeScript)

```
mi-servicio/
├── src/
│   ├── domain/          # Entidades y lógica de negocio pura
│   ├── application/     # Casos de uso / servicios de aplicación
│   ├── infrastructure/  # Implementaciones (BD, mensajería, HTTP)
│   │   ├── database/
│   │   ├── messaging/
│   │   └── http/
│   └── main.ts          # Punto de entrada
├── test/
│   ├── unit/
│   └── integration/
├── docs/                # Documentación específica del servicio
├── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## Comentarios y documentación

- El código debe ser autoexplicativo. Los comentarios explican el **por qué**, no el qué.
- Las funciones públicas de librerías y SDKs deben tener JSDoc / docstrings.
- Los `TODO` deben incluir el ticket asociado: `// TODO(PROJ-123): refactorizar una vez migrada la BD`.
- Prohibido dejar código comentado en la rama principal. Usar el historial de Git.

---

## Cobertura de pruebas

| Tipo de prueba | Cobertura mínima | Herramienta de referencia |
|---|---|---|
| Pruebas unitarias | 80% de lógica de dominio | Jest, Pytest, JUnit |
| Pruebas de integración | Flujos críticos de negocio | Supertest, Testcontainers |
| Pruebas end-to-end | Happy path de cada feature | Playwright, Cypress |

La cobertura se verifica en CI. Un PR que baje la cobertura por debajo del mínimo no puede hacer merge.

---

## Gestión de dependencias

- Actualizar dependencias con vulnerabilidades conocidas dentro de 5 días hábiles tras la notificación.
- No usar dependencias con licencia GPL en proyectos comerciales sin aprobación legal.
- Preferir dependencias con mantenimiento activo (último commit < 12 meses).
- Usar `package-lock.json` / `poetry.lock` / `gradle.lockfile` — siempre commitear el lockfile.
