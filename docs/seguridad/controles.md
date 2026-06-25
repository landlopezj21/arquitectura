# Controles de Seguridad

Controles técnicos obligatorios clasificados por categoría y nivel de sistema. Los controles marcados con ✅ son obligatorios para todos los sistemas. Los marcados con 🔴 aplican solo a sistemas **Críticos**.

---

## Autenticación y autorización

| Control | Todos | Críticos | Implementación de referencia |
|---|---|---|---|
| Autenticación mediante proveedor centralizado (SSO) | ✅ | ✅ | Auth0, Cognito, Keycloak |
| MFA obligatorio para accesos administrativos | ✅ | ✅ | TOTP, WebAuthn |
| Tokens con tiempo de expiración corto (< 1 hora para access tokens) | ✅ | ✅ | JWT con exp claim |
| Refresh tokens rotatorios con revocación | — | 🔴 | OAuth 2.0 + PKCE |
| Revisión periódica de permisos (cada 90 días) | ✅ | ✅ | Proceso manual + audit log |
| Principio de mínimo privilegio | ✅ | ✅ | IAM roles por servicio |

---

## Protección de datos en tránsito

| Control | Todos | Críticos |
|---|---|---|
| TLS 1.2 mínimo en todas las comunicaciones externas | ✅ | ✅ |
| TLS 1.3 recomendado, 1.2 permitido solo por compatibilidad | ✅ | ✅ |
| Certificados válidos (no autofirmados en producción) | ✅ | ✅ |
| mTLS entre servicios internos críticos | — | 🔴 |
| HSTS habilitado en APIs y frontends web | ✅ | ✅ |

---

## Protección de datos en reposo

| Control | Todos | Críticos |
|---|---|---|
| Cifrado de base de datos en reposo | ✅ | ✅ |
| Cifrado de backups | ✅ | ✅ |
| Datos personales sensibles cifrados a nivel de campo | — | 🔴 |
| Claves de cifrado gestionadas en KMS (no hardcodeadas) | ✅ | ✅ |

---

## Gestión de secretos

- **Prohibido** hardcodear credenciales, tokens o claves en el código.
- **Prohibido** commitear archivos `.env` con valores reales.
- Usar el gestor de secretos aprobado por la organización (AWS Secrets Manager o HashiCorp Vault).
- Las credenciales de servicio deben rotarse automáticamente cada 90 días.
- El acceso a secretos debe estar auditado.

---

## Seguridad en APIs

| Control | Descripción |
|---|---|
| Validación de input | Todo input externo debe validarse antes de procesarse |
| Sanitización de output | Prevenir XSS en respuestas que renderizan HTML |
| Rate limiting | Límites por IP y por usuario autenticado |
| Autenticación en todos los endpoints | Sin endpoints públicos con datos sensibles |
| Versionado de API | Deprecar versiones antiguas con tiempo de aviso (mínimo 3 meses) |
| CORS configurado explícitamente | No usar `*` en producción |

---

## Logging de seguridad

Los siguientes eventos deben quedar registrados en el sistema de logs centralizado:

- Intentos de autenticación (exitosos y fallidos)
- Cambios de permisos y roles
- Acceso a datos sensibles (quién accedió y cuándo)
- Errores de autorización (403)
- Cambios en configuración del sistema
- Despliegues a producción

Los logs de seguridad deben retenerse mínimo **12 meses** (en revisión) y no pueden ser modificados ni eliminados por los propios sistemas.

---

## Escaneo y pruebas de seguridad

[WIP]

| Actividad | Frecuencia | Responsable |
|---|---|---|
| Escaneo de dependencias (SCA) | CI/CD automático |
| Escaneo de imagen Docker (Trivy) | En cada build | CI/CD automático |
| Análisis estático de seguridad (SAST) | CI/CD automático |
| Prueba de penetración | Anual o ante cambios mayores | Equipo de seguridad o proveedor externo |
| Revisión de seguridad de diseño | Antes de lanzar sistemas críticos | Arquitectura + Seguridad |
