---
sidebar_position: 1
---

# Referencia de API

## Introducción

La API de Arkflows proporciona una interfaz programática para interactuar con la plataforma. Esta documentación detalla los endpoints disponibles, sus parámetros y respuestas.

## Autenticación

### OAuth 2.0

```http
POST /oauth/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
client_id=YOUR_CLIENT_ID&
client_secret=YOUR_CLIENT_SECRET
```

### JWT

```http
GET /api/v1/resource
Authorization: Bearer YOUR_JWT_TOKEN
```

## Endpoints Principales

### Procesos

#### Listar Procesos
```http
GET /api/v1/processes
```

**Parámetros de Query:**
- `page`: Número de página (default: 1)
- `limit`: Elementos por página (default: 20)
- `status`: Estado del proceso
- `category`: Categoría del proceso

**Respuesta:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "description": "string",
      "status": "string",
      "category": "string",
      "createdAt": "string",
      "updatedAt": "string"
    }
  ],
  "pagination": {
    "total": "number",
    "page": "number",
    "limit": "number"
  }
}
```

#### Crear Proceso
```http
POST /api/v1/processes
Content-Type: application/json

{
  "name": "string",
  "description": "string",
  "category": "string",
  "definition": "string"
}
```

#### Obtener Proceso
```http
GET /api/v1/processes/{processId}
```

#### Actualizar Proceso
```http
PUT /api/v1/processes/{processId}
Content-Type: application/json

{
  "name": "string",
  "description": "string",
  "category": "string",
  "definition": "string"
}
```

#### Eliminar Proceso
```http
DELETE /api/v1/processes/{processId}
```

### Formularios

#### Listar Formularios
```http
GET /api/v1/forms
```

**Parámetros de Query:**
- `page`: Número de página
- `limit`: Elementos por página
- `type`: Tipo de formulario

**Respuesta:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "fields": "array",
      "createdAt": "string",
      "updatedAt": "string"
    }
  ],
  "pagination": {
    "total": "number",
    "page": "number",
    "limit": "number"
  }
}
```

#### Crear Formulario
```http
POST /api/v1/forms
Content-Type: application/json

{
  "name": "string",
  "type": "string",
  "fields": "array"
}
```

#### Obtener Formulario
```http
GET /api/v1/forms/{formId}
```

#### Actualizar Formulario
```http
PUT /api/v1/forms/{formId}
Content-Type: application/json

{
  "name": "string",
  "type": "string",
  "fields": "array"
}
```

#### Eliminar Formulario
```http
DELETE /api/v1/forms/{formId}
```

### Integraciones

#### Listar Conectores
```http
GET /api/v1/connectors
```

**Parámetros de Query:**
- `type`: Tipo de conector
- `status`: Estado del conector

**Respuesta:**
```json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "type": "string",
      "status": "string",
      "config": "object",
      "createdAt": "string",
      "updatedAt": "string"
    }
  ]
}
```

#### Crear Conector
```http
POST /api/v1/connectors
Content-Type: application/json

{
  "name": "string",
  "type": "string",
  "config": "object"
}
```

#### Probar Conector
```http
POST /api/v1/connectors/{connectorId}/test
Content-Type: application/json

{
  "config": "object"
}
```

### Instancias de Proceso

#### Iniciar Proceso
```http
POST /api/v1/processes/{processId}/instances
Content-Type: application/json

{
  "variables": "object"
}
```

#### Listar Instancias
```http
GET /api/v1/processes/{processId}/instances
```

**Parámetros de Query:**
- `status`: Estado de la instancia
- `startDate`: Fecha de inicio
- `endDate`: Fecha de fin

#### Obtener Instancia
```http
GET /api/v1/processes/{processId}/instances/{instanceId}
```

#### Completar Tarea
```http
POST /api/v1/tasks/{taskId}/complete
Content-Type: application/json

{
  "variables": "object"
}
```

## Códigos de Estado

- `200 OK`: Solicitud exitosa
- `201 Created`: Recurso creado
- `400 Bad Request`: Solicitud inválida
- `401 Unauthorized`: No autenticado
- `403 Forbidden`: No autorizado
- `404 Not Found`: Recurso no encontrado
- `500 Internal Server Error`: Error del servidor

## Manejo de Errores

```json
{
  "error": {
    "code": "string",
    "message": "string",
    "details": "object"
  }
}
```

## Límites y Cuotas

- **Rate Limiting**: 1000 requests por hora
- **Tamaño máximo de payload**: 10MB
- **Tiempo máximo de timeout**: 30 segundos

## Versiones

- **v1**: Versión actual
- **v2**: En desarrollo (beta)

## SDKs

- JavaScript/TypeScript
- Python
- Java
- .NET

## Ejemplos

### JavaScript

```javascript
const arkflows = require('@arkflows/sdk');

const client = new arkflows.Client({
  clientId: 'YOUR_CLIENT_ID',
  clientSecret: 'YOUR_CLIENT_SECRET'
});

// Listar procesos
const processes = await client.processes.list();

// Crear proceso
const process = await client.processes.create({
  name: 'Mi Proceso',
  description: 'Descripción del proceso',
  category: 'General'
});

// Iniciar instancia
const instance = await client.processes.startInstance(process.id, {
  variables: {
    key: 'value'
  }
});
```

### Python

```python
from arkflows import Client

client = Client(
    client_id='YOUR_CLIENT_ID',
    client_secret='YOUR_CLIENT_SECRET'
)

# Listar procesos
processes = client.processes.list()

# Crear proceso
process = client.processes.create(
    name='Mi Proceso',
    description='Descripción del proceso',
    category='General'
)

# Iniciar instancia
instance = client.processes.start_instance(
    process.id,
    variables={'key': 'value'}
)
```

## Recursos Adicionales

- [Guía de Inicio Rápido](./quickstart)
- [Ejemplos de Código](./examples)
- [Changelog](./changelog)
- [Soporte](./support) 