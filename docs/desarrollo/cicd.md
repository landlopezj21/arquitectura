# CI/CD

Todo repositorio debe tener un pipeline automatizado de integración y despliegue continuo. Sin despliegues manuales a producción.

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

```
Push → Lint → Build → Test unitarios → Test integración → Build imagen Docker
     → Push a registry → Deploy staging → Test E2E → [Aprobación] → Deploy producción
```

### Etapas obligatorias

**1. Lint y formato**
```yaml
# Ejemplo GitHub Actions
- name: Lint
  run: npm run lint

- name: Formato
  run: npm run format:check
```

**2. Pruebas con reporte de cobertura**
```yaml
- name: Pruebas unitarias
  run: npm run test:coverage

- name: Verificar cobertura mínima
  run: npm run test:coverage:check  # Falla si < 80%
```

**3. Análisis de seguridad de dependencias**
```yaml
- name: Auditoría de dependencias
  run: npm audit --audit-level=high
```

**4. Build de imagen Docker**
```yaml
- name: Build imagen
  run: docker build -t $IMAGE_NAME:$GITHUB_SHA .

- name: Escaneo de vulnerabilidades en imagen
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: ${{ env.IMAGE_NAME }}:${{ github.sha }}
    exit-code: '1'
    severity: 'CRITICAL,HIGH'
```

---

## Política de despliegue a producción

### Despliegue automático (bajo riesgo)
- Servicios internos sin datos sensibles
- Cambios de configuración sin impacto en usuarios
- Hotfixes con aprobación del líder técnico

### Despliegue con aprobación manual (alto riesgo)
- Cambios que afectan flujos de pago o autenticación
- Migraciones de base de datos
- Cambios en APIs públicas consumidas por terceros
- Cualquier cambio en sistemas clasificados como críticos

La aprobación se realiza en el pipeline de CI/CD por el responsable técnico o de guardia.

---

## Rollback

Todo despliegue a producción debe poder revertirse en menos de 5 minutos. Estrategias aceptadas:

- **Redeploy de versión anterior:** reejecutar el pipeline con el tag de imagen anterior.
- **Feature flags:** desactivar la funcionalidad sin necesidad de redeploy.
- **Blue/Green:** redirigir tráfico al ambiente anterior.

El procedimiento de rollback debe estar documentado en el `README.md` de cada servicio.

---

## Variables de entorno y secretos

- Los secretos **nunca** van en el repositorio. Ni en `.env` commiteados.
- Usar el gestor de secretos de la organización (AWS Secrets Manager / HashiCorp Vault).
- El archivo `.env.example` debe tener todas las variables requeridas con valores de ejemplo (sin valores reales).
- Las variables de entorno requeridas por el servicio deben estar documentadas en el `README.md`.

---

## Notificaciones

El pipeline debe notificar al canal de Slack `#despliegues` en los siguientes eventos:

- ✅ Despliegue exitoso a producción
- ❌ Fallo en cualquier etapa del pipeline
- ⏳ Despliegue en espera de aprobación manual
