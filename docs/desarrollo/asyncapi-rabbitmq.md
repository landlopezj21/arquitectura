# Estándar AsyncAPI con RabbitMQ

## ¿Qué es AsyncAPI?

AsyncAPI es un estándar abierto para definir interfaces de APIs asíncronas (event-driven). Es el equivalente de OpenAPI/Swagger pero para mensajería y eventos. Permite documentar canales, mensajes, esquemas y bindings de brokers como RabbitMQ, Kafka, MQTT, etc.

**Versión actual:** AsyncAPI 3.0  
**Sitio oficial:** https://www.asyncapi.com

---

## Estructura de un Documento AsyncAPI

```yaml
asyncapi: 3.0.0        # Versión del estándar
info:                  # Metadatos del servicio
servers:               # Brokers / conexiones
channels:              # Canales (queues / topics)
operations:            # Qué hace el servicio (send/receive)
components:            # Esquemas, mensajes y bindings reutilizables
```

---

## Ejemplo Base: Toolbox MQ con RabbitMQ

```yaml
asyncapi: 3.0.0

info:
  title: Toolbox Message Queue API
  version: 1.0.0
  description: |
    Documentación de mensajería asíncrona del sistema Toolbox.
    Utiliza RabbitMQ como broker AMQP para comunicación entre microservicios.
  contact:
    name: Equipo Toolbox
    email: toolbox@soaint.com
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0

servers:
  production:
    host: rabbitmq.toolbox.soaint.com:5672
    protocol: amqp
    description: RabbitMQ producción
    security:
      - $ref: '#/components/securitySchemes/userPassword'
  development:
    host: localhost:5672
    protocol: amqp
    description: RabbitMQ local para desarrollo

channels:
  documentProcessing:
    address: toolbox.documents.process
    description: Cola para procesamiento asíncrono de documentos
    bindings:
      amqp:
        is: queue
        queue:
          name: toolbox.documents.process
          durable: true
          exclusive: false
          autoDelete: false
        exchange:
          name: toolbox.exchange
          type: direct
          durable: true
          autoDelete: false
    messages:
      documentProcessRequest:
        $ref: '#/components/messages/DocumentProcessRequest'

  notificationEvents:
    address: toolbox.notifications
    description: Exchange para distribución de notificaciones a múltiples consumidores
    bindings:
      amqp:
        is: routingKey
        exchange:
          name: toolbox.notifications.exchange
          type: topic
          durable: true
          autoDelete: false
    messages:
      notificationEvent:
        $ref: '#/components/messages/NotificationEvent'

  domainEvents:
    address: toolbox.events.domain
    description: Canal de eventos de dominio (Publish/Subscribe)
    bindings:
      amqp:
        is: routingKey
        exchange:
          name: toolbox.events.exchange
          type: fanout
          durable: true
          autoDelete: false
    messages:
      domainEvent:
        $ref: '#/components/messages/DomainEvent'

operations:
  sendDocumentToProcess:
    action: send
    channel:
      $ref: '#/channels/documentProcessing'
    summary: Envía un documento para procesamiento asíncrono
    description: |
      Publicado por api-procesar-x cuando un documento debe ser
      procesado por ia-doc-engine o crisalida-engine.
    bindings:
      amqp:
        expiration: 60000
        deliveryMode: 2  # persistent
        mandatory: true
    messages:
      - $ref: '#/channels/documentProcessing/messages/documentProcessRequest'

  receiveDocumentToProcess:
    action: receive
    channel:
      $ref: '#/channels/documentProcessing'
    summary: Consume documentos pendientes de procesamiento
    description: Consumido por ia-doc-engine y crisalida-engine.
    messages:
      - $ref: '#/channels/documentProcessing/messages/documentProcessRequest'

  publishNotification:
    action: send
    channel:
      $ref: '#/channels/notificationEvents'
    summary: Publica una notificación al sistema
    description: |
      Permite enviar notificaciones por email, push, o SMS.
      Usa routing keys para filtrar por tipo: email.*, sms.*, push.*
    messages:
      - $ref: '#/channels/notificationEvents/messages/notificationEvent'

  publishDomainEvent:
    action: send
    channel:
      $ref: '#/channels/domainEvents'
    summary: Publica un evento de dominio a todos los suscriptores
    messages:
      - $ref: '#/channels/domainEvents/messages/domainEvent'

components:
  messages:
    DocumentProcessRequest:
      name: DocumentProcessRequest
      title: Solicitud de Procesamiento de Documento
      summary: Mensaje para iniciar el procesamiento de un documento con IA
      contentType: application/json
      bindings:
        amqp:
          contentEncoding: UTF-8
          messageType: toolbox.document.process
      headers:
        type: object
        properties:
          correlationId:
            type: string
            description: ID para trazabilidad del mensaje
          timestamp:
            type: string
            format: date-time
          source:
            type: string
            description: Microservicio que origina el mensaje
      payload:
        $ref: '#/components/schemas/DocumentProcessPayload'

    NotificationEvent:
      name: NotificationEvent
      title: Evento de Notificación
      contentType: application/json
      headers:
        type: object
        properties:
          routingKey:
            type: string
            description: "Formato: {channel}.{type} — ej: email.invoice, sms.alert"
      payload:
        $ref: '#/components/schemas/NotificationPayload'

    DomainEvent:
      name: DomainEvent
      title: Evento de Dominio
      contentType: application/json
      headers:
        type: object
        properties:
          eventType:
            type: string
            description: Tipo de evento de dominio
          aggregateId:
            type: string
            description: ID del agregado raíz
      payload:
        $ref: '#/components/schemas/DomainEventPayload'

  schemas:
    DocumentProcessPayload:
      type: object
      required:
        - documentId
        - processType
        - callbackUrl
      properties:
        documentId:
          type: string
          format: uuid
          description: Identificador único del documento
          example: "550e8400-e29b-41d4-a716-446655440000"
        processType:
          type: string
          enum: [OCR, CLASSIFICATION, EXTRACTION, VALIDATION]
          description: Tipo de procesamiento a aplicar
          example: "OCR"
        documentUrl:
          type: string
          format: uri
          description: URL del documento en Alfresco/ECM
          example: "https://alfresco.soaint.com/documents/550e8400"
        metadata:
          type: object
          additionalProperties: true
          description: Metadatos adicionales del documento
        callbackUrl:
          type: string
          format: uri
          description: Endpoint para notificar el resultado del procesamiento
        priority:
          type: integer
          minimum: 1
          maximum: 10
          default: 5
          description: Prioridad del procesamiento (1=baja, 10=alta)

    NotificationPayload:
      type: object
      required:
        - recipient
        - subject
        - body
      properties:
        recipient:
          type: string
          description: Destinatario (email, teléfono, deviceToken)
          example: "usuario@soaint.com"
        subject:
          type: string
          description: Asunto o título de la notificación
          example: "Documento procesado exitosamente"
        body:
          type: string
          description: Contenido del mensaje
        templateId:
          type: string
          description: ID de plantilla predefinida
        variables:
          type: object
          additionalProperties: true
          description: Variables para reemplazar en la plantilla

    DomainEventPayload:
      type: object
      required:
        - eventId
        - eventType
        - occurredAt
        - payload
      properties:
        eventId:
          type: string
          format: uuid
          description: ID único del evento
        eventType:
          type: string
          description: Tipo de evento
          example: "DocumentProcessed"
        occurredAt:
          type: string
          format: date-time
          description: Momento en que ocurrió el evento
        aggregateId:
          type: string
          description: ID del agregado que generó el evento
        aggregateType:
          type: string
          description: Tipo del agregado raíz
          example: "Document"
        payload:
          type: object
          additionalProperties: true
          description: Datos específicos del evento

  securitySchemes:
    userPassword:
      type: userPassword
      description: Autenticación con usuario y contraseña de RabbitMQ
```

---

## Patrones Implementados

### Point-to-Point (Cola directa)
Un productor envía a una cola, un único consumidor recibe el mensaje. Usado en `toolbox.documents.process`.

```
api-procesar-x  →  [Queue: toolbox.documents.process]  →  ia-doc-engine
```

### Publish/Subscribe (Fanout Exchange)
Un productor, múltiples consumidores reciben todos los mensajes. Usado en `toolbox.events.domain`.

```
api-procesar-x  →  [Fanout Exchange]  →  servicio-A
                                      →  servicio-B
                                      →  servicio-C
```

### Topic-based Routing
Enrutamiento por routing key con wildcards. Usado en `toolbox.notifications`.

```
publisher  →  [Topic Exchange]  →  email.*  →  email-service
                                →  sms.*    →  sms-service
                                →  push.*   →  push-service
```

---

## Herramientas Recomendadas

| Herramienta | Uso |
|---|---|
| [AsyncAPI Studio](https://studio.asyncapi.com) | Editor visual online |
| [AsyncAPI CLI](https://github.com/asyncapi/cli) | Validación y generación desde terminal |
| [AsyncAPI Generator](https://github.com/asyncapi/generator) | Generación de código y docs HTML |
| [Microcks](https://microcks.io) | Mock y testing de APIs asíncronas |

### Generar documentación HTML

```bash
# Instalar CLI
npm install -g @asyncapi/cli

# Validar el archivo
asyncapi validate asyncapi-rabbitmq.yaml

# Generar documentación HTML
asyncapi generate fromTemplate asyncapi-rabbitmq.yaml @asyncapi/html-template -o ./docs

# Generar código (ej. Spring Boot)
asyncapi generate fromTemplate asyncapi-rabbitmq.yaml @asyncapi/java-spring-template -o ./generated
```

---

## Buenas Prácticas

- **Nombrar canales** con notación de puntos: `{dominio}.{entidad}.{acción}` — ej: `toolbox.documents.process`
- **Usar `correlationId`** en headers para trazabilidad end-to-end
- **Definir `deliveryMode: 2`** (persistent) en mensajes críticos para sobrevivir reinicios del broker
- **Versionar mensajes** agregando un campo `version` en el payload o en el nombre del canal
- **Documentar routing keys** claramente cuando se usan exchanges de tipo `topic`
- **Centralizar esquemas** en `components/schemas` para reutilización entre mensajes
