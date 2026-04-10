# LEO OS Project Plan

## Overview
LEO OS is a highly modular, event-driven Agentic Operating System infrastructure that seamlessly merges the Python and Node.js ecosystems. Built on top of the Model Context Protocol (MCP), this architecture is designed to orchestrate an autonomous "Agent Team" capable of coding, reviewing, and managing organizational knowledge safely and efficiently.

## 1. Project Scope
- **Include All Current Modules:** Implement all six core modules as described: MCP Gateway, Skill Manager, ETL & RAG Engine, Memory Service, Vector Service, and Decision Engine.
- **Future Expansion:** Plan for additional modules that can be added in the future without disrupting existing architecture.

## 2. Technical Dependencies
- **Python Version:** Use Python 3.11+ to leverage modern features and improvements in the language.
- **Node.js Version:** Use Node.js 18.x LTS as it is stable and widely supported.
- **Docker Images:**
  - For PostgreSQL with `pgvector` extension, use a compatible image like `postgres:14-alpine`.
  - Ensure compatibility between Docker images for different services (e.g., Python and Node.js) to avoid conflicts.

## 3. Deployment Strategy
- **Deployment on Single Server:** Design each microservice as a separate Docker container, allowing them to run independently on a single server using Docker Compose.
- **Deployment on Multiple Servers:** Use Kubernetes or Docker Swarm to orchestrate multiple servers for high availability and scalability.
- **On-Premise Deployment:** Focus on deploying the project locally without cloud dependencies. Ensure each module has its own Docker image to facilitate easy scaling.

## 4. Security Measures
- **JWT Authentication:**
  - Implement JWT authentication at the MCP Gateway, ensuring that all requests are validated before reaching internal services.
- **API Keys for M2M Communication:** Use API keys for machine-to-machine integrations between services.
- **Internal Communication Secrets:** Use Docker secrets or environment variables to manage sensitive information like JWT and API keys.

## 5. Implementation Plan
### 1. Prepare Development Environment
- **Install Dependencies:**
  - Install `docker`, `docker-compose`, `nodejs`, `python3.11`, and necessary libraries.
- **Setup Workspace:**
  - Clone the repository from GitHub.
  - Generate contracts using `generate_contracts.sh`.

### 2. Design Core Architecture
- **Hexagonal Architecture Implementation:**
  - Define interfaces in `/app/ports` for all services.
  - Implement core logic in `/app/core`.
  - Develop adapters in `/app/adapters` for REST and gRPC communication.

### 3. Develop Microservices
#### MCP Gateway (Node.js/TypeScript)
- **Create a router that translates MCP Tool/Resource calls into internal service calls.**
- **Implement JWT authentication middleware.**

#### Skill Manager (Python 3.11+)
- **Define `app/ports/communication_port.py` and `executor_port.py`.**
- **Implement skill invocation logic using `shlex.quote()` for safe execution.**

#### ETL & RAG Engine (Node.js + BullMQ)
- **Set up BullMQ queues for ETL jobs.**
- **Develop workers that fetch, embed, and store data from TFS repositories and wiki.**

#### Memory Service (Node.js/TypeScript)
- **Implement `IMemoryProvider` interface with methods `get()`, `set()`, `search()`, and `summarize()`.**

#### Vector Service (Python)
- **Create embedding models using OpenAI.**
- **Implement shadow indexing for zero-downtime updates.**

#### Decision Engine (Python)
- **Define `InferenceProvider` port with local and remote adapter implementations.**
- **Handle intent classification, output validation, and simple question answering.**

### 4. Setup Database
- **PostgreSQL with `pgvector`:**
  - Enable the vector extension in PostgreSQL.
  - Create tables (`rag_chunks`) and indexes (`GIN`, `ivfflat`) for hybrid RAG retrieval.

### 5. Configure Logging and Tracing (future work)
- **Centralized Logging:**
  - Implement structured JSON-based logging for all modules.
  - Ensure logs include standard fields like `timestamp`, `level`, `service_name`, `correlation_id`, and `message`.

- **Distributed Tracing (future work):**
  - Instrument all services with OpenTelemetry SDK.
  - Configure Jaeger to collect traces.

### 6. Documentation Generation (future work)
- **API Documentation:**
  - Use FastAPI for Python modules and Fastify with Swagger for Node.js modules.
  - Ensure auto-generated Swagger UI is available at `/docs` or `/api-docs`.

- **Skill Documentation:**
  - Generate markdown files in `skill-manager/docs/` for each new skill.

### 7. Testing & Validation (future work)
- **Zero-Test Policy Override:**
  - Although the Zero-Test Policy is in place, ensure that critical components are validated through manual testing.

- **Staging Deployment:**
  - Deploy the project to a staging environment for comprehensive testing before moving to production.

### 8. Future Work (Planned)
- **OpenTelemetry Integration:**
  - Develop common OTel bootstrap modules and configure services accordingly.
  
- **Authorization Server:**
  - Implement an authorization server with Node.js using Keycloak or similar technologies in the future.

### 9. Project Timeline (No Fixed Deadline)
- **Phase 1:** Development of core microservices and initial deployment setup.
- **Phase 2:** Database configuration and hybrid RAG implementation.
- **Phase 3:** API documentation and logging/tracing integration.
- **Phase 4:** Future enhancements like OpenTelemetry, authorization server, etc.

### 10. Communication Protocols
- **Dynamic Communication Type:**
  - Use `COMM_TYPE` environment variable to switch between REST and gRPC dynamically.
  
- **Service-to-Service Communication:**
  - Ensure all internal services communicate securely over REST or gRPC as configured.

## Next Steps
1. **Setup Development Environment:**
   - Install necessary tools (`docker`, `docker-compose`, `nodejs`, `python3.11`).
   - Clone the repository and generate contracts using `generate_contracts.sh`.

2. **Design Core Architecture:**
   - Define interfaces in `/app/ports`.
   - Implement core logic in `/app/core`.
   - Develop adapters in `/app/adapters` for REST and gRPC communication.

3. **Develop Microservices:**
   - Start with the MCP Gateway, Skill Manager, ETL & RAG Engine, Memory Service, Vector Service, and Decision Engine.
   
4. **Configure PostgreSQL with `pgvector`:**
   - Enable vector extension and create necessary tables and indexes.

5. **Implement Hybrid RAG Retrieval:**
   - Develop the `search_rag_hybrid(...)` function for hybrid retrieval.

6. **Deploy to Staging Environment:**
   - Deploy services in a staging environment for comprehensive testing.

7. **Review and Refine:**
   - Review initial deployment and refine as necessary.