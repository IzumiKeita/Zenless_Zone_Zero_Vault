# 🗺️ ERP3 - Core Flow & Architecture

Este documento define el flujo de datos y la jerarquía de componentes del sistema ERP3. **IA: Consultar este mapa antes de realizar cambios en la infraestructura o en la lógica de eventos.**

## 🏗️ Diagrama de Flujo (Mermaid)

Este diagrama representa la vida de un dato desde el Frontend hasta el sistema de auditoría.

```mermaid

graph TD
    subgraph Client_Zone [Zona de Cliente]
        FE[Frontend - Next.js]
    end

    subgraph Core_Zone [Zona de Operación]
        API[REST API - FastAPI]
        CORE((CORE Engine))
        DB_M[(Postgres Master)]
        DB_R[(DB Réplica)]
    end

    subgraph Event_Zone [Streaming & Async]
        DEB[Debezium CDC]
        KAFKA{Kafka Bus}
        RABBIT[RabbitMQ]
        ESPEJO[(DB Espejo Auditoría)]
    end

    subgraph Addon_Zone [Ecosistema]
        INT[Add-ons Internos]
        EXT[Add-ons Externos / Clientes]
        N8N[n8n Automation]
        MCP[MCP Server - AI Agent]
    end

    %% Flujos Principales
    FE <-->|HTTPS/JSON| API
    API <--> CORE
    CORE -->|Write| DB_M
    CORE -.->|Read Heavy| DB_R
    DB_M -.->|Replicación| DB_R

    %% Flujo de Eventos
    DB_M -->|Binlog/WAL| DEB
    DEB --> KAFKA
    KAFKA --> ESPEJO
    KAFKA --> EXT
    KAFKA --> N8N

    %% Flujo Interno
    CORE -->|Task Queue| RABBIT
    RABBIT --> INT
    INT --> CORE

    %% AI & Automation
    N8N <--> MCP
    MCP <--> CORE
```





## 🚦 Reglas de Enrutamiento de Datos (Contexto para IA)

### 1. Persistencia (Postgres)

- **Escritura:** Solo en `DB_M` (Master). Nunca realizar `INSERT/UPDATE` en la réplica.
    
- **Lectura:** Consultas transaccionales rápidas en `Master`. Reportes, históricos y auditorías pesadas en `DB_R` (Réplica).
    

### 2. Comunicación Asíncrona

- **RabbitMQ:** Usar para tareas internas del sistema que requieren respuesta o confirmación (ej: generación de PDFs, envío de correos, jobs de mantenimiento).
    
- **Kafka (via Debezium):** Usar para la "salida al mundo". Todo cambio en la base de datos se emite aquí para que sistemas externos (o el Espejo de Auditoría) reaccionen sin bloquear el Core.
    

### 3. Inteligencia & Automatización (MCP + n8n)

- **MCP Server:** Actúa como el puente de contexto. Provee a los modelos locales (Ollama/LM Studio) acceso de solo lectura a los esquemas y datos para análisis.
    
- **n8n:** Orquestador de entrada/salida. Puede inyectar datos al Core vía REST API tras procesar eventos externos (Webhooks, WhatsApp, Emails).
    

---

## 📂 Estructura de Módulos (`app.core.*`)

- `auth`: Gestión de identidad y JWT.
    
- `database`: Configuración de motores (Writer/Reader).
    
- `infrastructure`: Implementación de Brokers (Kafka/RabbitMQ).
    
- `models`: Definiciones de tablas base para el ERP.