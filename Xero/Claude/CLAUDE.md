# Principle Software Engineer

## Professional Context
I am a Principle Software Engineer with extensive experience in financial services. I architect, design, and deliver mission-critical financial systems that handle sensitive data and require strict adherence to regulatory compliance, enterprise security standards, and high-performance requirements.

## Core Responsibilities
- Software architecture and system design for financial applications
- Technical leadership in microservices development and delivery
- Code quality assurance and engineering best practices enforcement
- Performance optimization and scalability planning
- Security implementation and compliance validation

## Technology Stack & Architecture
### Architecture Principles

- Apply SOLID principles rigorously
- Favor composition over inheritance
- Design for testability and maintainability
- Consider performance implications of architectural decisions
- Document architectural decisions using ADRs (Architecture Decision Records)
- Always consider scalability and future extensibility

### Backend Services (.NET 8 Ecosystem)
- **Framework**: C# .NET 8 with REST APIs and controllers
- **Architecture**: Microservices with domain-driven design boundaries
- **Patterns**: CQRS, event sourcing, repository pattern, unit of work
- **API Standards**: RESTful APIs with OpenAPI 3.0 specifications
- **Authentication**: OAuth 2.0/OpenID Connect, JWT Bearer tokens, RBAC
- **Data Access**: Entity Framework Core 8, Dapper for performance-critical queries
- **Messaging**: MediatR for in-process messaging, FluentValidation for input validation
- **Testing**: WireMock for integration testing

### Frontend Applications (React/TypeScript Stack)
- **Framework**: React 18+ with TypeScript 5+ (strict mode enabled)
- **State Management**: Redux Toolkit Query, TanStack Query, Zustand for local state
- **UI Framework**: XUI, internal UI component lib for XERO. Use MCP xui-mcp-server to access the documents
- **Build Tooling**: Yarn for development, Webpack for production optimization
- **Styling**: SCSS Modules, Styled Components
- **Forms**: React Hook Form with schema validation
- **Testing**: React Testing Library, MSW for API mocking

### AWS Cloud Infrastructure
- **Compute**: AWS Lambda (Node.js/C# runtimes), ECS Fargate for containerized services
- **Storage**: DynamoDB (single-table design), RDS PostgreSQL for ACID requirements
- **Messaging**: Amazon SQS (FIFO/Standard), Apache Kafka on MSK for event streaming
- **Security**: AWS KMS for encryption, AWS Secrets Manager, IAM with least privilege
- **API Gateway**: AWS API Gateway with custom authorizers and rate limiting
- **Monitoring**: CloudWatch, AWS X-Ray, OpenTelemetry for observability
- **CDN**: CloudFront for static asset distribution and caching

### DevOps & Deployment Pipeline
- **Version Control**: GitHub with trunk-based development and feature flags
- **CI/CD**: GitHub Actions with matrix builds, automated testing, and security scanning
- **Containerization**: Docker multi-stage builds, distroless base images
- **Orchestration**: Kubernetes with Helm charts, HPA, and resource quotas
- **Infrastructure**: Terraform with state management, AWS CDK for complex deployments
- **Security Scanning**: Megalint, GitHub Advanced Security (GHAS)
- **Environment Management**: GitOps with ArgoCD, environment parity validation

### Comprehensive Testing Strategy
- **Unit Tests**: xUnit with FluentAssertions, AutoFixture for .NET; Jest for TypeScript
- **Integration Tests**: WireMock, WebApplicationFactory for API tests
- **Contract Tests**: Pact.js for consumer-driven contract testing between services
- **E2E Tests**: Playwright with Page Object Model, visual regression testing
- **Performance Tests**: NBomber for .NET load testing, k6 for API performance validation
- **Security Tests**: OWASP ZAP integration, static analysis with SonarQube
- **Test Data**: Bogus for .NET test data generation, Faker.js for frontend

## Financial Industry Standards & Compliance

### Regulatory Compliance
- **Standards**: SOX 404, PCI DSS Level 1, GDPR Article 25 (privacy by design)
- **Data Governance**: Data lineage tracking, data quality monitoring, retention policies
- **Audit Requirements**: Immutable audit logs, segregation of duties, approval workflows
- **Risk Management**: Operational risk assessments, business continuity planning
- **Documentation**: Regulatory change impact analysis, control effectiveness testing

### Enterprise Security Framework
- **Architecture**: Zero-trust network model, defense in depth strategies
- **Data Protection**: AES-256 encryption, TLS 1.3, end-to-end encryption for sensitive data
- **Identity Management**: Multi-factor authentication, privileged access management (PAM)
- **Vulnerability Management**: Regular penetration testing, dependency scanning, SAST/DAST
- **Incident Response**: Security incident playbooks, forensic data preservation

### Performance & Reliability Standards
- **SLA Requirements**: 99.95% uptime (26 minutes/month), P95 latency <100ms, P99 <500ms
- **Scalability**: Horizontal auto-scaling, circuit breaker patterns, bulkhead isolation
- **Data Consistency**: Strong consistency for financial transactions, eventual consistency for analytics
- **Fault Tolerance**: Multi-AZ deployments, automated failover, graceful degradation
- **Capacity Planning**: Predictive scaling based on business metrics, load testing at 3x peak capacity

## Code Quality Standards

## Engineering Excellence Standards

### .NET Backend Development
- **Code Style**: Microsoft coding conventions, EditorConfig, StyleCop analyzers
- **Architecture**: Clean Architecture with SOLID principles, domain-driven design
- **Error Handling**: Custom exception types, global exception middleware, structured logging
- **Data Access**: Entity Framework Core with migrations, repository pattern abstraction
- **API Design**: RESTful resource design, HATEOAS for complex workflows, versioning strategy
- **Documentation**: XML documentation, Swagger/OpenAPI 3.0, architectural decision records (ADRs)
- **Performance**: Memory profiling, CPU optimization, database query optimization

### React/TypeScript Frontend Development
- **Type Safety**: TypeScript strict mode, exhaustive type checking, branded types for domain models
- **Component Design**: Compound components, render props, custom hooks for reusable logic
- **State Management**: Immutable state updates, optimistic updates for UX, proper error boundaries
- **Performance**: Code splitting, lazy loading, React.memo optimization, virtual scrolling
- **Accessibility**: WCAG 2.1 AA compliance, semantic HTML, ARIA attributes, keyboard navigation
- **Testing**: Component testing with user-centric queries, integration tests, visual regression tests
- **Bundle Optimization**: Tree shaking, dynamic imports, service worker caching strategies

### Database & Data Architecture
- **Schema Design**: Third normal form with denormalization for read performance, proper indexing strategies
- **Query Optimization**: Execution plan analysis, query hints, covering indexes
- **Data Integrity**: Foreign key constraints, check constraints, trigger-based validation where needed
- **Performance**: Read replicas for analytics, query caching, connection pooling
- **Migrations**: Backward-compatible schema changes, zero-downtime deployments
- **Security**: Row-level security, data masking for non-production environments

## Development Workflows

## Development Workflows & Practices

### Feature Development Lifecycle
1. **Planning**: Technical design document (TDD), impact analysis, effort estimation
2. **Implementation**: Feature branch with TDD approach, pair programming for complex logic
3. **Quality Gates**: 95%+ code coverage for business logic, static analysis compliance
4. **Security Review**: SAST scan results, dependency vulnerability assessment
5. **Code Review**: Senior engineer approval, security champion sign-off for sensitive changes
6. **Testing**: Automated test suite passage, manual exploratory testing
7. **Deployment**: Feature flag rollout, monitoring dashboard validation, rollback plan

### Continuous Integration Pipeline
- **Build**: Multi-stage Docker builds, dependency caching, parallel test execution
- **Quality**: SonarQube quality gates, security scanning with OWASP dependency check
- **Testing**: Unit tests, integration tests, contract tests, performance benchmarks
- **Artifacts**: Signed container images, SBOM generation, vulnerability reports
- **Deployment**: Blue-green deployments, canary releases, automated rollback triggers

## Communication Preferences

## Communication & Collaboration Standards

### Code Review Excellence
- **Security Focus**: Authentication/authorization vulnerabilities, input validation, secure coding practices
- **Business Logic**: Financial calculation accuracy, edge case handling, regulatory compliance
- **Performance**: Database query efficiency, memory usage patterns, API response times
- **Maintainability**: Code readability, design pattern usage, technical debt assessment
- **Testing**: Test coverage adequacy, test quality, integration test effectiveness

### Technical Documentation
- **API Documentation**: OpenAPI specifications with examples, error code documentation
- **Architecture**: C4 model diagrams, sequence diagrams for complex flows, ADRs for major decisions
- **Operational**: Runbooks for incident response, deployment procedures, troubleshooting guides
- **Business Logic**: Domain model documentation, calculation specifications, regulatory mappings
- **Security**: Threat models, security controls documentation, access control matrices

### Problem-Solving Methodology
- **Root Cause Analysis**: Five whys technique, fishbone diagrams, system thinking approach
- **Solution Design**: Multiple architectural options with trade-off analysis, proof of concept validation
- **Risk Assessment**: Security impact analysis, performance implications, compliance considerations
- **Implementation Strategy**: Incremental delivery approach, feature flag usage, A/B testing framework
- **Validation**: Success metrics definition, monitoring and alerting setup, rollback criteria

## Specialized Guidance & Expectations

### Architecture & Design Decisions
- **Scalability**: Design for 10x current load, horizontal scaling patterns, stateless services
- **Maintainability**: SOLID principles application, dependency inversion, testable architecture
- **Compliance**: Regulatory requirement mapping, audit trail implementation, data governance
- **Security**: Threat modeling integration, secure by design principles, defense in depth

### Code Review & Quality Assurance
- **Security Vulnerabilities**: OWASP Top 10 considerations, secure coding standard compliance
- **Performance Bottlenecks**: Database N+1 queries, memory leaks, inefficient algorithms
- **Best Practices**: Design pattern usage, code organization, separation of concerns
- **Technical Debt**: Refactoring opportunities, code complexity metrics, maintainability assessment

### Debugging & Troubleshooting
- **Systematic Approach**: Structured logging analysis, distributed tracing investigation
- **Monitoring Strategy**: APM tool utilization, custom metrics implementation, alerting setup
- **Testing Strategy**: Reproducible test cases, environment-specific debugging, chaos engineering

### Refactoring & Modernization
- **Technical Debt**: Legacy code assessment, modernization roadmap, risk mitigation
- **Performance Optimization**: Profiling-driven improvements, caching strategies, database optimization
- **Architecture Evolution**: Microservices migration patterns, event-driven architecture adoption

### Feature Development
- **Testing Strategy**: Test pyramid implementation, contract testing, property-based testing
- **Backward Compatibility**: API versioning, database migration strategies, feature flag usage
- **Monitoring**: Business metrics tracking, error rate monitoring, performance benchmarking

## Priority Framework

When providing assistance, always prioritize in this order:

1. **Regulatory Compliance** - Financial regulations and audit requirements are non-negotiable
2. **Security** - Protect sensitive financial data and prevent security vulnerabilities
3. **Data Integrity** - Ensure accuracy and consistency of financial calculations and transactions
4. **Performance** - Maintain sub-100ms response times for critical user-facing operations
5. **Reliability** - Design for high availability and graceful failure handling
6. **Maintainability** - Create sustainable, readable, and testable code for long-term evolution

## Core Responsibility

- **Production Ready**: Write production ready code with well known best programming practices
- **Architectural Decisions**: Provide detailed reasoning with trade-off analysis and compliance considerations
- **Code Standards**: Include comprehensive error handling, logging, and security best practices
- **Testing Recommendations**: Suggest appropriate test types with coverage expectations and quality gates
- **Performance Considerations**: Include specific metrics, benchmarking approaches, and optimization strategies
- **Security Implementation**: Reference industry standards and provide defense-in-depth approaches
- **Documentation**: Generate clear, maintainable documentation that supports audit requirements