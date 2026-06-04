# Estándares de APIs: OpenAPI y Modelo de Madurez Richardson

---

## 1. Modelo de Madurez Richardson (RMM)

El **Richardson Maturity Model** es una escala propuesta por Leonard Richardson para medir el nivel de adopción de REST en una API. Tiene 4 niveles (0 al 3), donde el nivel 3 representa una API REST "pura" o HATEOAS.

```
Nivel 3 │ HATEOAS          ← REST completo
Nivel 2 │ HTTP Verbs       ← La mayoría de APIs "REST" reales
Nivel 1 │ Resources        ← URIs para cada recurso
Nivel 0 │ Swamp of POX     ← Un único endpoint, todo por POST
```

---

### Nivel 0 — Swamp of POX (Plain Old XML / JSON)

Un único endpoint que recibe todas las operaciones. No hay diferenciación de recursos ni uso semántico de HTTP.

**Características:**
- Un solo endpoint (`/api` o `/service`)
- Todas las operaciones van por `POST`
- El tipo de acción va en el body

**Ejemplo:**
```http
POST /api
Content-Type: application/json

{
  "action": "getDocument",
  "documentId": "123"
}
```

```http
POST /api
Content-Type: application/json

{
  "action": "createDocument",
  "title": "Contrato 2024",
  "content": "..."
}
```

**Problemas:** No cacheable, no semántico, difícil de documentar y mantener.

---

### Nivel 1 — Resources (Recursos)

Se introducen URIs para identificar recursos individuales, pero aún se usa `POST` para todo.

**Características:**
- Recursos identificados por URI
- Sin uso correcto de verbos HTTP
- No hay estado de respuesta semántico

**Ejemplo:**
```http
POST /documents
{ "action": "get", "id": "123" }

POST /documents/123
{ "action": "delete" }

POST /invoices
{ "action": "list", "status": "pending" }
```

---

### Nivel 2 — HTTP Verbs (Verbos HTTP)

Se incorporan los métodos HTTP con su semántica correcta: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`. Los códigos de respuesta también se usan correctamente.

**Características:**
- `GET` para consultar (idempotente, cacheable)
- `POST` para crear
- `PUT` / `PATCH` para actualizar
- `DELETE` para eliminar
- Códigos HTTP semánticos (`200`, `201`, `404`, `422`, etc.)

**Ejemplo:**
```http
GET /documents
→ 200 OK  [ lista de documentos ]

GET /documents/123
→ 200 OK  { "id": "123", "title": "Contrato 2024" }
→ 404 Not Found  (si no existe)

POST /documents
Content-Type: application/json
{ "title": "Nuevo Contrato", "type": "invoice" }
→ 201 Created
Location: /documents/456

PUT /documents/123
{ "title": "Contrato 2024 Actualizado" }
→ 200 OK

DELETE /documents/123
→ 204 No Content
```

> **Este es el nivel en que se encuentran la mayoría de APIs REST modernas.**

---

### Nivel 3 — HATEOAS (Hypermedia As The Engine Of Application State)

El servidor incluye en cada respuesta los enlaces (`_links`) a las acciones disponibles para ese recurso en su estado actual. El cliente no necesita conocer de antemano la estructura de URLs.

**Características:**
- Respuestas incluyen hipervínculos a acciones posibles
- El cliente descubre la API navegando por los links
- Reduce el acoplamiento entre cliente y servidor
- Formato común: HAL (`application/hal+json`), JSON:API, Siren

**Ejemplo (HAL):**
```http
GET /documents/123
→ 200 OK

{
  "id": "123",
  "title": "Contrato 2024",
  "status": "draft",
  "_links": {
    "self":    { "href": "/documents/123" },
    "approve": { "href": "/documents/123/approve", "method": "POST" },
    "reject":  { "href": "/documents/123/reject",  "method": "POST" },
    "history": { "href": "/documents/123/history" },
    "owner":   { "href": "/users/456" }
  }
}
```

Si el documento ya está aprobado, la respuesta no incluirá `approve` ni `reject` como opciones:

```json
{
  "id": "123",
  "status": "approved",
  "_links": {
    "self":    { "href": "/documents/123" },
    "archive": { "href": "/documents/123/archive", "method": "POST" },
    "history": { "href": "/documents/123/history" }
  }
}
```

---

## 2. Tabla Comparativa RMM

| Nivel | Nombre | Recursos | Verbos HTTP | HATEOAS | Ejemplo |
|---|---|---|---|---|---|
| 0 | Swamp of POX | ✗ | ✗ | ✗ | SOAP, RPC |
| 1 | Resources | ✓ | ✗ | ✗ | URIs, todo POST |
| 2 | HTTP Verbs | ✓ | ✓ | ✗ | REST convencional |
| 3 | HATEOAS | ✓ | ✓ | ✓ | REST completo / HAL |

---

## 3. OpenAPI — Estándar para APIs Síncronas (REST)

**OpenAPI Specification (OAS)** es el estándar para documentar APIs HTTP/REST síncronas. Es gestionado por la **OpenAPI Initiative** y es la evolución de Swagger.

**Versión actual:** OpenAPI 3.1.0

### Estructura base

```yaml
openapi: 3.1.0

info:
  title: Toolbox Document API
  version: 1.0.0
  description: API REST para gestión de documentos — Nivel 2 RMM
  contact:
    name: Equipo Toolbox
    email: toolbox@soaint.com

servers:
  - url: https://api.toolbox.soaint.com/v1
    description: Producción
  - url: https://api-dev.toolbox.soaint.com/v1
    description: Desarrollo

paths:
  /documents:
    get:
      operationId: listDocuments
      summary: Listar documentos
      tags: [Documents]
      parameters:
        - name: status
          in: query
          schema:
            type: string
            enum: [draft, approved, rejected, archived]
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Lista de documentos
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/DocumentList'
        '401':
          $ref: '#/components/responses/Unauthorized'

    post:
      operationId: createDocument
      summary: Crear documento
      tags: [Documents]
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateDocumentRequest'
      responses:
        '201':
          description: Documento creado
          headers:
            Location:
              schema:
                type: string
              description: URL del recurso creado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Document'
        '422':
          $ref: '#/components/responses/UnprocessableEntity'

  /documents/{documentId}:
    parameters:
      - name: documentId
        in: path
        required: true
        schema:
          type: string
          format: uuid

    get:
      operationId: getDocument
      summary: Obtener documento por ID
      tags: [Documents]
      responses:
        '200':
          description: Documento encontrado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Document'
        '404':
          $ref: '#/components/responses/NotFound'

    patch:
      operationId: updateDocument
      summary: Actualizar documento parcialmente
      tags: [Documents]
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateDocumentRequest'
      responses:
        '200':
          description: Documento actualizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Document'

    delete:
      operationId: deleteDocument
      summary: Eliminar documento
      tags: [Documents]
      responses:
        '204':
          description: Documento eliminado

  /documents/{documentId}/approve:
    post:
      operationId: approveDocument
      summary: Aprobar documento (transición de estado)
      tags: [Documents, Workflow]
      responses:
        '200':
          description: Documento aprobado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Document'
        '409':
          description: Conflicto — el documento no está en estado aprobable

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  responses:
    Unauthorized:
      description: Token JWT inválido o expirado
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    NotFound:
      description: Recurso no encontrado
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
    UnprocessableEntity:
      description: Error de validación
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/ValidationError'

  schemas:
    Document:
      type: object
      properties:
        id:
          type: string
          format: uuid
        title:
          type: string
        status:
          type: string
          enum: [draft, approved, rejected, archived]
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time
        # Nivel 3 RMM — HATEOAS
        _links:
          $ref: '#/components/schemas/DocumentLinks'

    DocumentLinks:
      type: object
      description: Hipervínculos HATEOAS a acciones disponibles según estado
      properties:
        self:
          $ref: '#/components/schemas/Link'
        approve:
          $ref: '#/components/schemas/Link'
        reject:
          $ref: '#/components/schemas/Link'
        archive:
          $ref: '#/components/schemas/Link'
        history:
          $ref: '#/components/schemas/Link'

    Link:
      type: object
      properties:
        href:
          type: string
          format: uri
        method:
          type: string
          enum: [GET, POST, PUT, PATCH, DELETE]

    DocumentList:
      type: object
      properties:
        data:
          type: array
          items:
            $ref: '#/components/schemas/Document'
        total:
          type: integer
        page:
          type: integer
        limit:
          type: integer
        _links:
          type: object
          properties:
            self:
              $ref: '#/components/schemas/Link'
            next:
              $ref: '#/components/schemas/Link'
            prev:
              $ref: '#/components/schemas/Link'

    CreateDocumentRequest:
      type: object
      required: [title, type]
      properties:
        title:
          type: string
          minLength: 3
          maxLength: 255
        type:
          type: string
          enum: [invoice, contract, report]
        content:
          type: string

    UpdateDocumentRequest:
      type: object
      properties:
        title:
          type: string
        content:
          type: string

    Error:
      type: object
      properties:
        code:
          type: string
        message:
          type: string
        timestamp:
          type: string
          format: date-time

    ValidationError:
      type: object
      properties:
        code:
          type: string
        message:
          type: string
        errors:
          type: array
          items:
            type: object
            properties:
              field:
                type: string
              message:
                type: string
```

---

## 4. AsyncAPI — Estándar para APIs Asíncronas (Eventos / Mensajería)

**AsyncAPI** es el equivalente de OpenAPI para sistemas event-driven y mensajería asíncrona. Soporta RabbitMQ, Kafka, MQTT, WebSockets, SNS/SQS, etc.

**Versión actual:** AsyncAPI 3.0

### Diferencias clave vs OpenAPI

| Concepto | OpenAPI (REST) | AsyncAPI (Eventos) |
|---|---|---|
| Interacción | Request / Response | Publish / Subscribe |
| Timing | Síncrono | Asíncrono |
| Endpoint | Path (`/documents`) | Channel (`documents.created`) |
| Operación | `get`, `post`, etc. | `send`, `receive` |
| Protocolo | HTTP/HTTPS | AMQP, Kafka, MQTT, WS |
| Modelo | Cliente espera respuesta | Productor no espera consumidor |

### Modelo de Madurez aplicado a APIs Asíncronas

El RMM es nativo de REST, pero puede adaptarse para medir la madurez de un sistema de mensajería:

| Nivel | Nombre Adaptado | Descripción |
|---|---|---|
| 0 | Cola única genérica | Un solo topic/queue para todo |
| 1 | Canales por recurso | Un canal por entidad (`documents`, `invoices`) |
| 2 | Canales + tipo de evento | Canal con routing key: `documents.created`, `documents.approved` |
| 3 | Event-driven + Schema Registry | Eventos tipados, versionados, con schema centralizado y trazabilidad |


---

## 5. Cuándo usar OpenAPI vs AsyncAPI

| Criterio | OpenAPI | AsyncAPI |
|---|---|---|
| El cliente espera respuesta inmediata | ✓ | ✗ |
| Operaciones CRUD sobre recursos | ✓ | ✗ |
| Procesamiento en background | ✗ | ✓ |
| Notificaciones a múltiples sistemas | ✗ | ✓ |
| Desacoplamiento entre microservicios | Parcial | ✓ |
| Alta disponibilidad ante picos de carga | ✗ | ✓ |
| Trazabilidad de estado de un flujo | ✓ | Con esfuerzo |

> En arquitecturas modernas es común usar **ambos**: OpenAPI para la API REST que expone el servicio al frontend, y AsyncAPI para la comunicación interna entre microservicios.

---

## 6. Herramientas

### OpenAPI
| Herramienta | Uso |
|---|---|
| [Swagger Editor](https://editor.swagger.io) | Editor online |
| [Redoc](https://github.com/Redocly/redoc) | Documentación HTML elegante |
| [Stoplight Studio](https://stoplight.io) | Editor visual de diseño primero |
| [openapi-generator](https://openapi-generator.tech) | Generación de clientes/servidores |

### AsyncAPI
| Herramienta | Uso |
|---|---|
| [AsyncAPI Studio](https://studio.asyncapi.com) | Editor visual online |
| [AsyncAPI CLI](https://github.com/asyncapi/cli) | Validación y generación |
| [Microcks](https://microcks.io) | Mock y testing de eventos |
| [EventCatalog](https://www.eventcatalog.dev) | Catálogo de eventos de dominio |

---

## 7. Buenas Prácticas Generales

- **Diseño primero (Design-First):** Escribir el contrato (OpenAPI/AsyncAPI) antes de implementar
- **Versionado de contratos:** Usar versión en la URL (`/v1/`) o en el channel (`document.created.v1`)
- **Nombres de eventos en pasado:** `DocumentCreated`, `InvoiceApproved` (algo que ya ocurrió)
- **Nombres de endpoints en sustantivos:** `/documents`, no `/getDocuments`
- **Usar correlationId** en mensajes asíncronos para trazabilidad end-to-end
- **Schema Registry** para AsyncAPI en producción: Confluent Schema Registry o AWS Glue
- **Documentar errores:** Incluir `4xx` y `5xx` en OpenAPI; dead-letter queues en AsyncAPI
