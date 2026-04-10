# LEO OS Project Structure

## Overview
This document outlines the directory structure and code organization of the LEO OS project, focusing on the core microservices and their integrations.

## 1. Core Modules Directory Structure
- **/contracts**:
  - `skill.proto`
  - `shared_schemas.json`
  - `generate_contracts.sh`

- **/infra**
  - `/docker-compose.yml`
  - `/postgres`
    - `/init`
      - `001_extensions.sql`
      - `002_hybrid_rag.sql`

- **/app**
  - `/core`: Pure business logic, entities, and models.
  - `/ports`: Abstract Interfaces (e.g., ABC in Python, Interfaces in TypeScript).
  - `/adapters`: Concrete implementations (REST, gRPC, DB clients, Redis).

## 2. Microservices
### MCP Gateway (Node.js/TypeScript)
- **/app/mcp-gateway**
  - `index.ts`
  - `router.ts` (MCP Tool/Resource router)
  - `jwtAuthMiddleware.ts`

### Skill Manager (Python 3.11+)
- **/app/skill-manager**
  - `core.py` (Skill orchestrator logic)
  - `ports`
    - `communication_port.py`
    - `executor_port.py`
  - `adapters`
    - `terminalAdapter.py`

### ETL & RAG Engine (Node.js + BullMQ)
- **/app/etl-rag-engine**
  - `bullmq-worker.ts`
  - `vectorServiceClient.ts`
  - `memoryServiceClient.ts`

### Memory Service (Node.js/TypeScript)
- **/app/memory-service**
  - `memoryProvider.ts`
  - `redisAdapter.ts`
  - `postgresAdapter.ts`

### Vector Service (Python)
- **/app/vector-service**
  - `embeddingModel.py`
  - `shadowIndexer.py`
  - `vectorDBClient.py`

### Decision Engine (Python)
- **/app/decision-engine**
  - `inferenceProvider.py`
  - `localAdapter.py`
  - `liteLLMAdapter.py`

## 3. API Endpoints and Flows
### MCP Gateway
- **POST /mcp/tool**: Accepts MCP Tool requests.
- **POST /mcp/resource**: Accepts MCP Resource requests.

### Skill Manager
- **GET /skills**: Lists available skills.
- **POST /skill/invoke/<skill_id>**: Invokes a specific skill with payload.

### ETL & RAG Engine
- **POST /etl/ingest**: Triggers ETL job for data ingestion.
- **POST /rag/search**: Performs hybrid retrieval using text, code, and images.

### Memory Service
- **GET /memory/get/<key>**: Retrieves value by key from memory.
- **POST /memory/set/<key>**: Sets a value in memory.
- **POST /memory/search**: Searches for values in memory.
- **POST /memory/summarize**: Summarizes data in memory.

### Vector Service
- **POST /vector/embed**: Generates embeddings using OpenAI models.
- **POST /vector/index**: Indexes vectors into PostgreSQL.
- **GET /vector/search/<query>**: Searches vectors in the vector database.

### Decision Engine
- **POST /decision/classify-intent**: Classifies user intent.
- **POST /decision/validate-output**: Validates output from skills.
- **POST /decision/question-answer**: Provides simple question answers.

## 4. Frontend Module (Optional)
- **/frontend**
  - `index.html`
  - `app.js` (Simple UI for testing module services)

## 5. Docker Compose Setup
- Define services for each microservice in `docker-compose.yml`.
- Ensure network isolation and communication between services.