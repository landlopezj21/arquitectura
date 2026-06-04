# Estándar AsyncAPI con RabbitMQ

## ¿Qué es AsyncAPI?

AsyncAPI es un estándar abierto para definir interfaces de APIs asíncronas (event-driven). Es el equivalente de OpenAPI/Swagger pero para mensajería y eventos. Permite documentar canales, mensajes, esquemas y bindings de brokers como RabbitMQ, Kafka, MQTT, etc.

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
  title: API Usuarios - Toolbox
  version: 1.0.0
  description: |
    Documentación de mensajería asíncrona para el ciclo de vida de usuarios en el sistema Toolbox.
    Utiliza RabbitMQ como broker AMQP para la comunicación entre microservicios.
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
  domainEvents:
    address: toolbox.events.domain
    description: Canal basado en Clave de Enrutamiento para eventos globales de dominio de usuarios.
    bindings:
      amqp:
        is: routingKey
        exchange:
          name: toolbox.events.exchange
        bindingVersion: "0.3.0"
    messages:
      UserCreatedEvent:
        $ref: '#/components/messages/UserCreatedEvent'
      UserUpdatedEvent:
        $ref: '#/components/messages/UserUpdatedEvent'

operations:
  publishUserCreatedEvent:
    action: send
    channel:
      $ref: '#/channels/domainEvents'
    summary: Publica un evento cuando un usuario nuevo es creado en el sistema
    bindings:
      amqp:
        deliveryMode: 2
        ack: true
        bindingVersion: "0.3.0"
    messages:
      - $ref: '#/channels/domainEvents/messages/UserCreatedEvent'

  publishUserUpdatedEvent:
    action: send
    channel:
      $ref: '#/channels/domainEvents'
    summary: Publica un evento cuando el perfil de un usuario es modificado
    bindings:
      amqp:
        deliveryMode: 2
        ack: true
        bindingVersion: "0.3.0"
    messages:
      - $ref: '#/channels/domainEvents/messages/UserUpdatedEvent'

components:
  securitySchemes:
    userPassword:
      type: plain
      description: Autenticación nativa con usuario y contraseña en RabbitMQ

  messages:
    UserCreatedEvent:
      name: UserCreatedEvent
      title: Evento de Usuario Creado
      contentType: application/json
      bindings:
        amqp:
          contentEncoding: UTF-8
          messageType: fhir.usuario.creado
          bindingVersion: "0.3.0"
      headers:
        type: object
        properties:
          idCorrelacion:
            type: string
            description: Identificador único de trazabilidad para el Stack ELK
          marcaTiempo:
            type: string
            format: date-time
      payload:
        schema:
          $ref: '#/components/schemas/UserCreatedPayload'

    UserUpdatedEvent:
      name: UserUpdatedEvent
      title: Evento de Usuario Modificado
      contentType: application/json
      headers:
        type: object
        properties:
          idCorrelacion:
            type: string
            description: Identificador único de trazabilidad para el Stack ELK
          marcaTiempo:
            type: string
            format: date-time
      payload:
        schema:
          $ref: '#/components/schemas/UserUpdatedPayload'

  schemas:
    UserCreatedPayload:
      type: object
      required:
        - idUsuario
        - correoElectronico
        - creadoEn
      properties:
        idUsuario:
          type: string
          format: uuid
          description: Identificador único del nuevo usuario
          example: "742b8400-e29b-41d4-a716-446655440111"
        correoElectronico:
          type: string
          format: email
          description: Dirección de correo electrónico principal
          example: jplazcano@soaint.com
        nombre:
          type: string
          description: Nombre del usuario
          example: Juan
        apellido:
          type: string
          description: Apellido del usuario
          example: Lascano
        roles:
          type: array
          items:
            type: string
          description: Lista de roles asignados en el SSO (Keycloak)
          example: admin
        creadoEn:
          type: string
          format: date-time
          description: Fecha y hora exacta de la creación


    UserUpdatedPayload:
      type: object
      required:
        - idUsuario
        - modificadoEn
        - cambios
      properties:
        idUsuario:
          type: string
          format: uuid
          description: Identificador del usuario
           afectado
          example: "742b8400-e29b-41d4-a716-446655440111" 
        modificadoEn:
          type: string
          format: date-time
          description: Fecha y hora exacta de la modificación
        cambios:
          type: object
          description: Diccionario con las propiedades modificadas y sus nuevos valores
   
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
