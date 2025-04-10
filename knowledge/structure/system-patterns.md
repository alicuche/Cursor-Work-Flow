# System Patterns

## Architecture Overview

### Layers
```mermaid
flowchart TD
    AA[Active Admin]

    subgraph CLI[Client Layer]
        FE[Frontend Applications]
    end

    subgraph API[API Layer]
        GQL[GraphQL]
        RES[Rest]
    end

    SER[Service Layer]

    MOD[Model Layer]

    QUE[Queue Layer]

    CLI --> API
    API --> SER & MOD
    AA --> MOD
    SER & MOD --> QUE
```

### Data Flows

#### 1. Authentication Flow
```mermaid
sequenceDiagram
    Client->>RestBullhorn: Initialize
    RestBullhorn->>Bullhorn: OAuth Authentication
    Bullhorn-->>RestBullhorn: Access Token
    RestBullhorn-->>Client: Ready for Requests
```

#### 2. Data Synchronization Flow
```mermaid
sequenceDiagram
    Service->>SchemaExport: Get Schema
    SchemaExport->>SchemaField Model: Store Schema
    Service->>FetchId: Get Entity IDs
    FetchId->>Entity Model: Store IDs
    Service->>FetchEntity: Get Full Data
    FetchEntity->>Entity Model: Update Data
    Service->>FetchDropDown: Get Real-Data Dropdown
    FetchDropDown->>SchemaField Model: Update Data
```

#### 3. GraphQL API Flow
```mermaid
sequenceDiagram
    Client->>GraphQL: Send Query/Mutation
    GraphQL->>QueryType/MutationType: Parse and Validate
    QueryType/MutationType->>Resolvers: Execute Field Resolution
    Resolvers->>Models: Query/Update Data
    Models-->>Resolvers: Return Results
    Resolvers-->>QueryType/MutationType: Format Response
    QueryType/MutationType-->>GraphQL: Combine Results
    GraphQL-->>Client: Return JSON Response
```

#### 4. Models
  - Follow @db/schema.rb

## Key Design Patterns

### 2. Multi-tenant Pattern
- Tenant-specific configurations
- Isolated data per tenant
- Shared infrastructure

### 3. Queue-based Processing
- Asynchronous job processing with Sidekiq
- Batch processing capabilities
- Retry mechanisms for failed jobs

## Security Patterns
1. **Credential Management**
   - Encrypted storage
   - Secure token handling
   - Regular token rotation

2. **Data Protection**
   - PII encryption
   - Secure data transmission
   - Access control per tenant

3. **API Security**
   - GraphQL query depth and complexity limits
   - Rate limiting for GraphQL endpoint
   - Input validation and sanitization

## Performance Patterns
1. **Caching Strategy**
   - Redis for schema caching
   - Efficient data retrieval

2. **Batch Processing**
   - Optimized bulk operations
   - Controlled parallel processing

3. **Rate Limiting**
   - API request throttling
   - Queue-based workload management