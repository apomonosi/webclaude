# Advanced Usage

This guide covers advanced techniques, sophisticated workflows, and power-user strategies for maximizing Claude Code Web's capabilities.

## Advanced Prompting Techniques

### Meta-Prompting

Instruct Claude on how to approach the task itself:

!!! example "Meta-Prompting Pattern"
    ```
    "Before writing any code, I want you to:

    1. Ask me clarifying questions about requirements
    2. Propose 2-3 different architectural approaches
    3. Discuss trade-offs of each approach
    4. Wait for my decision
    5. Then implement the chosen approach with tests

    Let's build a caching layer for our API. Start with your questions."
    ```

This creates a collaborative dialogue rather than immediate code generation.

### Chain-of-Thought Engineering

For complex problems, explicitly request step-by-step reasoning:

```
"Solve this system design problem using chain-of-thought reasoning:

Problem: Design a URL shortener that handles 1 billion requests per day

Think through:
1. Requirements analysis (what must it do?)
2. Scale calculations (QPS, storage, bandwidth)
3. High-level architecture options
4. Database design choices
5. Caching strategy
6. Load balancing approach
7. Potential bottlenecks
8. Failure modes and mitigation

For each step, show your reasoning before moving to the next.
After the analysis, provide the system design with justifications."
```

### Constrained Optimization

Ask Claude to optimize under specific constraints:

!!! example "Optimization with Constraints"
    ```
    "Optimize this Python function for speed:

    [paste function]

    Constraints:
    - Cannot change the function signature (public API)
    - Must maintain exact same behavior for all inputs
    - Cannot use external libraries beyond stdlib
    - Must be Python 3.8+ compatible
    - Code must remain readable (no obfuscation)

    Provide:
    1. Analysis of current bottlenecks
    2. Optimization strategies considered
    3. Why you chose the approach you did
    4. Optimized code with inline comments
    5. Performance comparison (Big O)
    6. Test cases proving equivalence"
    ```

### Role-Based Prompting

Assign Claude specific expert roles:

```
"Act as a senior security engineer reviewing this authentication code.

You are particularly concerned with:
- OWASP Top 10 vulnerabilities
- Zero-trust security principles
- Defense in depth
- Least privilege
- Secure defaults

Review this code through that lens and provide:
1. Security assessment (critical/high/medium/low risks)
2. Specific vulnerabilities found
3. Attack vectors that could exploit each vulnerability
4. Secure implementations for each issue
5. Additional security measures to consider

Code:
[paste]

Be thorough and critical. This handles financial data."
```

### Comparative Analysis Prompting

Request side-by-side comparisons:

```
"Compare these three state management approaches for a large React application:

1. Context API with useReducer
2. Redux Toolkit
3. Zustand

For each, analyze:

Performance:
- Re-render behavior
- Memory usage
- Bundle size impact
- DevTools overhead

Developer Experience:
- Learning curve
- Boilerplate amount
- TypeScript support
- Testing ease
- Debug experience

Scalability:
- Large state trees
- Multiple contexts/stores
- Code splitting
- SSR compatibility

Then provide:
- Decision matrix table
- Recommendation for our use case: [describe app]
- Migration path if switching from Context API
- Code examples showing the same feature in all three"
```

## Advanced Workflows

### Multi-Stage Development Pipeline

Create sophisticated development workflows:

#### Stage 1: Architecture

```
"Let's design a real-time collaborative document editor (like Google Docs).

First, help me with the architecture:

1. Client-side architecture
   - UI framework choice
   - State management
   - Real-time sync approach
   - Offline-first strategy

2. Server-side architecture
   - Backend framework
   - Database choice
   - WebSocket vs Server-Sent Events
   - Conflict resolution algorithm

3. Infrastructure
   - Hosting platform
   - Scaling strategy
   - CDN requirements
   - Monitoring and observability

For each decision, provide 2-3 options with trade-offs.
Don't write code yet, just architecture decisions."
```

#### Stage 2: Protocol Design

```
"Based on the architecture we chose, design the WebSocket protocol:

1. Message format (JSON schema for each message type)
2. Connection lifecycle (handshake, heartbeat, reconnection)
3. Operational transforms for conflict resolution
4. Presence awareness (who's editing what)
5. Error handling and recovery

Provide:
- Protocol specification document
- State machines for client and server
- Example message flows for key scenarios
- Edge cases to handle"
```

#### Stage 3: Implementation

```
"Now implement the client-side editor component:

Using the architecture and protocol we designed, create:

1. React component for the editor
2. WebSocket connection manager
3. Operational transform logic
4. Local state with optimistic updates
5. Conflict resolution
6. TypeScript types for all messages

Include:
- Full implementation
- Unit tests for OT logic
- Integration tests for sync
- Error boundary handling
- Loading and error states"
```

#### Stage 4: Optimization

```
"Optimize the implementation:

Profile and improve:
1. Rendering performance (use React DevTools insights)
2. Network efficiency (minimize message size/frequency)
3. Memory leaks (cleanup listeners, connections)
4. Bundle size (code splitting, lazy loading)

Provide:
- Performance analysis
- Optimized code
- Before/after metrics
- Profiling methodology"
```

### Incremental Complexity Building

Start simple, add complexity iteratively:

!!! example "Incremental Development"
    **Version 1: Basic**
    ```
    "Create a simple todo list app with React:
    - Add/remove todos
    - Mark as complete
    - Local state only
    - No styling, just functionality"
    ```

    **Version 2: Persistence**
    ```
    "Enhance the todo app:
    - Add localStorage persistence
    - Load on mount, save on change
    - Handle localStorage errors"
    ```

    **Version 3: Features**
    ```
    "Add these features:
    - Categories/tags
    - Due dates
    - Priority levels
    - Filter and sort"
    ```

    **Version 4: Polish**
    ```
    "Now add:
    - Tailwind CSS styling
    - Animations (framer-motion)
    - Keyboard shortcuts
    - Accessibility (ARIA labels, focus management)"
    ```

    **Version 5: Production Ready**
    ```
    "Make it production ready:
    - Error boundaries
    - Loading states
    - Empty states
    - Input validation
    - TypeScript strict mode
    - Comprehensive tests
    - Performance optimization"
    ```

### Parallel Problem Solving

Work on multiple related tasks simultaneously:

```
"I need a complete user authentication system. Let's work on these in parallel:

Task A: Database Schema
Design PostgreSQL schema for:
- Users table
- Sessions table
- Password reset tokens
- Email verification tokens
Include: Indexes, constraints, migrations

Task B: API Endpoints
Design REST API:
- POST /auth/register
- POST /auth/login
- POST /auth/logout
- POST /auth/refresh
- POST /auth/forgot-password
- POST /auth/reset-password
Spec: OpenAPI 3.0 format

Task C: Security Implementation
Implement security measures:
- Password hashing (bcrypt)
- JWT token generation/verification
- CSRF protection
- Rate limiting
- Session management

Provide all three simultaneously, ensuring they work together cohesively."
```

## Advanced Code Review Techniques

### Layered Review Approach

Review code at multiple levels:

!!! example "Multi-Level Review"
    ```
    "Review this codebase at three levels:

    Level 1: Surface (5 minutes)
    - Code style and formatting
    - Obvious bugs
    - Missing error handling
    - Quick wins for improvement

    Level 2: Structural (15 minutes)
    - Design patterns used/misused
    - Code organization
    - Separation of concerns
    - Abstraction levels
    - Testability

    Level 3: Deep (30 minutes)
    - Algorithmic efficiency
    - Scalability issues
    - Security vulnerabilities
    - Race conditions
    - Memory leaks
    - Edge cases

    Code: [paste]

    For each level, provide:
    - Issues found (categorized by severity)
    - Specific line references
    - Suggested fixes
    - Estimated impact of fixing"
    ```

### Architectural Review

Focus on system-level concerns:

```
"Conduct an architectural review of this microservices system:

Services:
1. User Service (Node.js, MongoDB)
2. Product Service (Python, PostgreSQL)
3. Order Service (Go, PostgreSQL)
4. Payment Service (Java, PostgreSQL)
5. Notification Service (Node.js, Redis)

Communication: REST + RabbitMQ for events

Review for:

1. Service Boundaries
   - Are services appropriately sized?
   - Is domain separation logical?
   - Any services doing too much?

2. Data Management
   - Database per service pattern?
   - How is data consistency handled?
   - Transaction boundaries?
   - Data duplication strategy?

3. Communication Patterns
   - Sync vs async appropriate?
   - Event-driven design sound?
   - Circuit breakers implemented?
   - Retry logic and idempotency?

4. Scalability
   - Stateless services?
   - Horizontal scaling possible?
   - Bottlenecks?
   - Caching strategy?

5. Reliability
   - Single points of failure?
   - Failure modes and recovery?
   - Monitoring and observability?
   - Disaster recovery?

Provide:
- Current architecture diagram (mermaid)
- Issues and concerns
- Recommended architecture
- Migration path from current to recommended"
```

### Security-Focused Review

Deep security analysis:

```
"Perform a security audit on this web application:

Stack: React frontend, Node.js/Express backend, PostgreSQL

Focus areas:

1. Authentication & Authorization
   - Session management
   - Password policies
   - JWT implementation
   - Role-based access control

2. Input Validation
   - SQL injection vectors
   - XSS vulnerabilities
   - Command injection
   - Path traversal

3. Data Protection
   - Encryption at rest
   - Encryption in transit
   - Sensitive data in logs
   - PII handling

4. Infrastructure
   - CORS configuration
   - CSP headers
   - HTTPS enforcement
   - Rate limiting

5. Dependencies
   - Vulnerable packages
   - Outdated libraries
   - Supply chain risks

Code: [repository structure]

For each vulnerability:
1. CVSS score estimate
2. Exploit scenario
3. Impact assessment
4. Remediation steps
5. Prevention for future"
```

## Advanced Data Processing Patterns

### ETL Pipeline Design

Complex data transformation workflows:

!!! example "ETL Pipeline"
    ```
    "Design a production-grade ETL pipeline:

    Source: Multiple PostgreSQL databases (customer, orders, analytics)
    Destination: Data warehouse (Snowflake)
    Schedule: Every 6 hours
    Volume: ~10M rows per run

    Requirements:
    1. Incremental loading (not full refresh)
    2. Data validation and quality checks
    3. Error handling and retry logic
    4. Monitoring and alerting
    5. Idempotent operations
    6. Schema evolution handling

    Provide:

    1. Architecture diagram
    2. Python implementation (using Apache Airflow)
    3. DAG definition
    4. Data quality checks
    5. Error handling strategy
    6. Monitoring dashboard config
    7. Deployment instructions

    Include:
    - Unit tests for transformations
    - Integration tests for pipeline
    - Performance optimization
    - Cost optimization (Snowflake credits)"
    ```

### Stream Processing

Real-time data processing:

```
"Implement a real-time analytics stream processor:

Input: Kafka topic with user events (clicks, page views, purchases)
Rate: ~10,000 events/second
Processing:
- Sessionization (30-minute timeout)
- User journey tracking
- Real-time aggregations
- Anomaly detection

Output:
- PostgreSQL (aggregated metrics)
- Redis (session state)
- Elasticsearch (searchable events)
- Alert system (anomalies)

Use: Apache Flink (or suggest better alternative)

Provide:
1. Flink job implementation (Java/Scala)
2. Event schema definitions
3. State management strategy
4. Windowing functions
5. Exactly-once semantics implementation
6. Scaling configuration
7. Monitoring and debugging setup

Include handling for:
- Late arriving events
- Out-of-order events
- Backpressure
- Job recovery from failure"
```

## Advanced Testing Strategies

### Property-Based Testing

Beyond example-based tests:

!!! example "Property-Based Tests"
    ```
    "Write property-based tests for this sorting function:

    [paste function]

    Using hypothesis (Python) or fast-check (JavaScript), create tests that verify:

    1. Properties:
       - Output length equals input length
       - Output is sorted
       - Output contains same elements as input
       - Idempotence (sort(sort(x)) = sort(x))
       - Stability (for equal elements)

    2. Invariants:
       - No element appears more times than in input
       - All elements in output were in input
       - Reverse sort then sort = sort

    3. Edge cases (generated):
       - Empty arrays
       - Single element
       - All duplicates
       - Already sorted
       - Reverse sorted
       - Special values (null, undefined, NaN)

    Provide:
    - Complete property-based test suite
    - Custom generators for complex types
    - Shrinking examples for failures
    - Performance test properties
    - Integration with existing test framework"
    ```

### Mutation Testing

Validate test quality:

```
"Set up mutation testing for this codebase:

Code: [describe project]
Current test coverage: 85%

Tasks:

1. Configure mutation testing tool (Stryker/PITest)
2. Run initial mutation testing
3. Analyze results:
   - Mutation score
   - Survived mutants
   - Timeout mutants
   - Killed mutants

4. Improve tests to kill survived mutants
5. Identify redundant tests
6. Set up mutation testing in CI/CD

Provide:
- Configuration files
- Analysis of current mutation score
- Improved tests
- CI integration
- Team documentation on mutation testing"
```

### Chaos Engineering

Test system resilience:

```
"Implement chaos engineering for our microservices:

Services: [list]
Infrastructure: Kubernetes on AWS

Chaos experiments:

1. Network chaos
   - Latency injection
   - Packet loss
   - Partition (split-brain)

2. Resource chaos
   - CPU stress
   - Memory stress
   - Disk I/O stress

3. Infrastructure chaos
   - Pod failures
   - Node failures
   - Zone failures

4. Application chaos
   - Process kills
   - Configuration changes
   - Dependency failures

Using: Chaos Mesh or Litmus Chaos

Provide:
1. Chaos experiment definitions
2. Observability setup (metrics, logs, traces)
3. Success criteria for each experiment
4. Rollback procedures
5. Schedule for running experiments
6. Incident response integration
7. Post-experiment analysis template"
```

## Advanced Integration Patterns

### API Integration Framework

Robust third-party API integration:

!!! example "API Integration"
    ```
    "Create a robust integration framework for third-party APIs:

    Requirements:
    1. Support multiple API providers (REST, GraphQL)
    2. Automatic retry with exponential backoff
    3. Circuit breaker pattern
    4. Rate limiting (respect API quotas)
    5. Response caching
    6. Request/response logging
    7. Error classification and handling
    8. Webhook handling
    9. OAuth2 authentication
    10. Mock mode for testing

    Provide:

    1. Base integration class/module
    2. Provider-specific implementations (example: 2-3 APIs)
    3. Configuration management
    4. Error handling hierarchy
    5. Monitoring and alerting
    6. Testing utilities
    7. Usage documentation

    Language: TypeScript
    Include: Comprehensive tests and usage examples"
    ```

### Event-Driven Architecture

Complex event processing:

```
"Design an event-driven architecture:

System: E-commerce platform

Events:
- OrderPlaced
- PaymentProcessed
- InventoryReserved
- ShipmentCreated
- OrderFulfilled
- OrderCancelled

Requirements:
1. Event sourcing for orders
2. CQRS (read/write separation)
3. Saga pattern for distributed transactions
4. Event replay capability
5. Eventual consistency handling
6. Dead letter queue
7. Event versioning

Provide:

1. Event schemas (JSON Schema)
2. Event store design (EventStore or PostgreSQL)
3. Saga orchestration implementation
4. Read model projections
5. Event handlers for each event type
6. Consistency boundary definitions
7. Error handling and compensation
8. Monitoring and debugging tools

Technology: Node.js with EventStore or custom implementation

Include:
- Complete implementation
- Sequence diagrams for key flows
- Failure scenario handling
- Performance considerations
- Operational runbook"
```

## Advanced Performance Optimization

### Profiling-Driven Optimization

Data-driven performance improvement:

!!! example "Performance Optimization"
    ```
    "Optimize this slow application using profiling data:

    Application: Django REST API
    Problem: Response times >2 seconds (target: <200ms)

    Profiling data:
    - 60% time in database queries
    - 25% time in serialization
    - 10% time in business logic
    - 5% time in network

    Process:

    1. Database Optimization
       - Analyze query patterns
       - Add indexes
       - Optimize N+1 queries
       - Implement query result caching
       - Consider read replicas

    2. Serialization Optimization
       - Analyze serializer complexity
       - Use select_related/prefetch_related
       - Implement lazy serialization
       - Consider alternative serializers

    3. Application Optimization
       - Profile Python code
       - Optimize hot paths
       - Add caching layers
       - Implement async where beneficial

    4. Infrastructure
       - Connection pooling
       - Load balancing
       - CDN for static assets
       - Compression

    Provide:
    - Detailed optimization plan
    - Prioritized by impact
    - Implementation code
    - Before/after benchmarks
    - Monitoring setup to track improvements"
    ```

### Database Performance Tuning

Advanced database optimization:

```
"Tune this PostgreSQL database for performance:

Database: 500GB, ~1M queries/day
Issues:
- Slow queries (avg 800ms)
- Lock contention
- Bloated tables
- Inefficient indexes

Analyze and optimize:

1. Query Optimization
   - Identify slow queries (pg_stat_statements)
   - EXPLAIN ANALYZE for top queries
   - Rewrite inefficient queries
   - Add missing indexes
   - Remove redundant indexes

2. Schema Optimization
   - Normalize/denormalize as appropriate
   - Partitioning strategy for large tables
   - Archiving old data

3. Configuration Tuning
   - Memory settings (shared_buffers, work_mem)
   - Connection pooling (pgBouncer)
   - Autovacuum tuning
   - Checkpoint tuning

4. Monitoring Setup
   - Key metrics to track
   - Alerting rules
   - Query performance tracking

Provide:
- Analysis of current state
- Optimization recommendations (prioritized)
- Configuration changes
- SQL migration scripts
- Monitoring dashboard config
- Maintenance procedures"
```

## Advanced Security Patterns

### Zero-Trust Security Implementation

Comprehensive security architecture:

!!! example "Zero-Trust Architecture"
    ```
    "Implement zero-trust security for our cloud infrastructure:

    Infrastructure:
    - Kubernetes cluster (EKS)
    - Multiple microservices
    - PostgreSQL and Redis
    - S3 storage
    - External APIs

    Zero-Trust Principles:
    1. Never trust, always verify
    2. Least privilege access
    3. Micro-segmentation
    4. Continuous verification

    Implement:

    1. Identity & Access
       - Service mesh with mTLS (Istio/Linkerd)
       - SPIFFE/SPIRE for service identity
       - RBAC policies
       - Secrets management (Vault)

    2. Network Security
       - Network policies (Calico/Cilium)
       - Ingress/egress rules
       - Service-to-service auth
       - WAF integration

    3. Data Security
       - Encryption at rest
       - Encryption in transit
       - Data classification
       - DLP policies

    4. Monitoring & Response
       - Audit logging
       - Anomaly detection
       - Automated response
       - Compliance reporting

    Provide:
    - Architecture diagram
    - Implementation for each component
    - Configuration files (IaC - Terraform)
    - Security policies
    - Testing procedures
    - Compliance mapping (SOC2, ISO 27001)
    - Operational runbook"
    ```

## Next-Level Collaboration Patterns

### Documentation-First Development

Generate comprehensive documentation:

```
"Using documentation-first approach, design a new feature:

Feature: Multi-tenant SaaS billing system

Create these documents (in order):

1. Product Requirements Document (PRD)
   - Problem statement
   - User stories
   - Success metrics
   - Non-goals

2. Technical Design Document (TDD)
   - Architecture
   - Data models
   - API contracts
   - Security considerations
   - Migration strategy

3. API Documentation (OpenAPI 3.0)
   - All endpoints
   - Request/response schemas
   - Authentication
   - Error responses
   - Examples

4. Implementation Plan
   - Tasks breakdown
   - Dependencies
   - Timeline
   - Risk assessment

5. Testing Strategy
   - Unit test plan
   - Integration test plan
   - E2E test scenarios
   - Performance test criteria

6. Deployment Plan
   - Rollout strategy
   - Feature flags
   - Monitoring
   - Rollback procedure

Only after all documentation is reviewed and approved:
7. Implementation code

This ensures alignment before coding begins."
```

## Conclusion

Advanced usage of Claude Code Web involves:

- **Sophisticated prompting** techniques that guide Claude's approach
- **Multi-stage workflows** that break complex problems into manageable pieces
- **Layered analysis** that examines code and systems at multiple levels
- **Production-grade practices** for security, performance, and reliability
- **Comprehensive testing** strategies beyond simple unit tests
- **Documentation-first** approaches for better collaboration

The key to advanced usage is treating Claude as a knowledgeable collaborator rather than just a code generator. Structure your interactions to leverage its analytical capabilities, iterate deliberately, and always validate with domain expertise.

### Further Reading

- [Introduction](introduction.md) - Understand core capabilities
- [Effective Usage](effective-usage.md) - Master communication techniques
- [Best Practices](best-practices.md) - Maintain code quality
- [Research Use Cases](research-use-cases.md) - Explore non-coding applications

---

**Remember**: Advanced techniques are powerful but require careful application. Always review, test, and validate Claude's suggestions in your specific context.
