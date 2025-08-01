# GitHub Copilot Instructions

> **Template Version:** 1.0
> **Last Updated:** [DATE]
> **Project:** [PROJECT_NAME]
> **Maintainer:** [YOUR_NAME] - Senior Software Engineer

---

## Project Overview

### Basic Information
- **Project Name:** [PROJECT_NAME]
- **Project Type:** [Web Application | Mobile App | API Service | Desktop App | Library | Microservice]
- **Domain:** [E-commerce | Finance | Healthcare | Education | Enterprise | Consumer | etc.]
- **Target Users:** [End users, developers, businesses, etc.]
- **Project Stage:** [Planning | Development | Testing | Production | Maintenance]

### Technology Stack
- **Primary Language:** [TypeScript | JavaScript | Python | Java | C# | Go | Rust | etc.]
- **Framework/Runtime:** [React | Vue | Angular | Next.js | Express | FastAPI | Spring Boot | .NET | etc.]
- **Database:** [PostgreSQL | MySQL | MongoDB | Redis | SQLite | etc.]
- **Infrastructure:** [AWS | Azure | GCP | Docker | Kubernetes | Serverless | etc.]
- **Testing:** [Jest | Vitest | PyTest | JUnit | NUnit | Cypress | Playwright | etc.]

### Architecture & Patterns
- **Architecture Style:** [Monolith | Microservices | Serverless | JAMstack | MVC | Clean Architecture]
- **Design Patterns:** [Repository | Factory | Observer | Strategy | Dependency Injection | etc.]
- **API Style:** [REST | GraphQL | gRPC | tRPC | WebSocket]
- **State Management:** [Redux | Zustand | MobX | Context API | Pinia | etc.]

---

## Development Standards & Best Practices

### Code Quality Principles
- **SOLID Principles:** Follow Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion
- **DRY (Don't Repeat Yourself):** Eliminate code duplication through abstraction and reusable components
- **KISS (Keep It Simple, Stupid):** Prefer simple, readable solutions over complex ones
- **YAGNI (You Aren't Gonna Need It):** Don't implement functionality until it's actually needed
- **Clean Code:** Write code that is easy to read, understand, and maintain

### Coding Standards

#### General Guidelines
- Use **meaningful and descriptive names** for variables, functions, classes, and modules
- **Functions should be small** and do one thing well (typically 10-20 lines)
- **Classes should follow Single Responsibility Principle**
- **Avoid deep nesting** - prefer early returns and guard clauses
- **Use consistent indentation** (2 or 4 spaces, no tabs)
- **Maximum line length: 100-120 characters**
- **Use positive conditionals** when possible (`if (isValid)` vs `if (!isInvalid)`)

#### Language-Specific Standards

##### TypeScript/JavaScript
```typescript
// ✅ Good - Descriptive names, small functions, clear types
interface UserProfile {
  id: string;
  email: string;
  firstName: string;
  lastName: string;
  createdAt: Date;
}

const validateUserEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

// ❌ Avoid - Unclear names, complex functions
const validateData = (d: any) => {
  // Complex validation logic here
};
```

- **Prefer `const` over `let`**, avoid `var`
- **Use TypeScript interfaces** for object shapes
- **Use async/await** over promises when possible
- **Use optional chaining** (`?.`) and nullish coalescing (`??`)
- **Prefer functional programming** patterns (map, filter, reduce)

##### Python (if applicable)
- Follow **PEP 8** style guide
- Use **type hints** for function parameters and return values
- Use **f-strings** for string formatting
- **Classes use PascalCase**, **functions and variables use snake_case**
- **Use list/dict comprehensions** when they improve readability

##### Other Languages
[Add language-specific guidelines based on your project stack]

### Error Handling & Logging

#### Error Handling Patterns
```typescript
// ✅ Good - Explicit error handling
const fetchUserData = async (userId: string): Promise<User | null> => {
  try {
    const response = await api.get(`/users/${userId}`);
    return response.data;
  } catch (error) {
    logger.error(`Failed to fetch user ${userId}:`, error);
    return null;
  }
};

// ✅ Good - Custom error classes
class ValidationError extends Error {
  constructor(field: string, value: unknown) {
    super(`Invalid value for field '${field}': ${value}`);
    this.name = 'ValidationError';
  }
}
```

#### Logging Standards
- **Use structured logging** (JSON format in production)
- **Include correlation IDs** for request tracing
- **Log levels:** ERROR, WARN, INFO, DEBUG
- **Never log sensitive information** (passwords, tokens, PII)
- **Include context** in error messages (user ID, operation, etc.)

### Security Best Practices

#### Authentication & Authorization
- **Never store passwords in plain text** - use proper hashing (bcrypt, Argon2)
- **Implement proper session management** with secure tokens
- **Use HTTPS everywhere** in production
- **Validate all user inputs** - never trust client-side validation alone
- **Implement rate limiting** on sensitive endpoints

#### Data Protection
- **Sanitize outputs** to prevent XSS attacks
- **Use parameterized queries** to prevent SQL injection
- **Encrypt sensitive data** at rest and in transit
- **Implement proper CORS** configuration
- **Use Content Security Policy** headers

#### API Security
- **Require authentication** for protected endpoints
- **Implement proper authorization** checks
- **Validate request content types** and sizes
- **Use API versioning** for backward compatibility
- **Document security requirements** in API specs

---

## Testing Standards

### Testing Philosophy
- **Test Pyramid:** Prioritize unit tests, supported by integration and E2E tests
- **Test-Driven Development (TDD):** Write tests before implementation when appropriate
- **Behavior-Driven Development (BDD):** Focus on testing behavior, not implementation
- **Test Coverage:** Aim for 80%+ code coverage, but focus on meaningful coverage

### Testing Patterns

#### Unit Testing
```typescript
// ✅ Good - Clear test structure, descriptive names
describe('UserValidator', () => {
  describe('validateEmail', () => {
    it('should return true for valid email addresses', () => {
      // Arrange
      const validator = new UserValidator();
      const validEmail = 'user@example.com';

      // Act
      const result = validator.validateEmail(validEmail);

      // Assert
      expect(result).toBe(true);
    });

    it('should return false for invalid email addresses', () => {
      const validator = new UserValidator();
      const invalidEmails = ['invalid-email', '@example.com', 'user@'];

      invalidEmails.forEach(email => {
        expect(validator.validateEmail(email)).toBe(false);
      });
    });
  });
});
```

#### Integration Testing
- **Test API endpoints** with real HTTP requests
- **Test database operations** with test database
- **Mock external services** to ensure reliability
- **Test error scenarios** and edge cases

#### End-to-End Testing
- **Test critical user journeys** only
- **Keep E2E tests minimal** and fast
- **Run in CI/CD pipeline** before deployments
- **Use page object pattern** for maintainability

### Test Organization
- **Test files named:** `*.test.ts`, `*.spec.ts`, or `*_test.py`
- **Co-locate tests** with source files when possible
- **Use descriptive test names** that explain the scenario
- **Group related tests** using describe blocks
- **Use setup/teardown** methods for common test data

---

## Documentation Standards

### Code Documentation
- **JSDoc/docstrings** for all public APIs
- **README files** for all packages/modules
- **Inline comments** for complex business logic only
- **Architecture Decision Records (ADRs)** for significant decisions

### API Documentation
- **OpenAPI/Swagger** specifications for REST APIs
- **GraphQL schema documentation** with descriptions
- **Include examples** for all endpoints
- **Document error responses** and status codes

### Project Documentation
- **README.md** with setup and development instructions
- **CONTRIBUTING.md** for contribution guidelines
- **CHANGELOG.md** for version history
- **docs/** folder for detailed documentation

---

## Performance & Optimization

### Performance Guidelines
- **Optimize for the common case** first
- **Measure before optimizing** - use profiling tools
- **Consider Big O complexity** for algorithms
- **Implement caching** strategically (Redis, CDN, browser cache)
- **Use database indexes** appropriately
- **Optimize bundle sizes** for frontend applications

### Database Optimization
- **Use appropriate data types** and constraints
- **Implement proper indexing** strategy
- **Avoid N+1 queries** - use eager loading
- **Use database-specific features** (stored procedures, views) when beneficial
- **Monitor query performance** and slow queries

### Frontend Performance (if applicable)
- **Code splitting** and lazy loading
- **Optimize images** and assets
- **Use CDN** for static assets
- **Implement proper caching** strategies
- **Monitor Core Web Vitals**

---

## Version Control & Git Workflow

### Branching Strategy
- **Main Branch:** `main` or `master` (production-ready code)
- **Development Branch:** `develop` (integration branch)
- **Feature Branches:** `feature/TICKET-123-short-description`
- **Bug Fix Branches:** `bugfix/TICKET-456-short-description`
- **Hotfix Branches:** `hotfix/TICKET-789-critical-fix`

### Commit Message Format
```
type(scope): subject

body (optional)

footer (optional)
```

**Types:** feat, fix, docs, style, refactor, test, chore
**Examples:**
- `feat(auth): add OAuth2 integration`
- `fix(api): resolve null pointer exception in user service`
- `docs(readme): update installation instructions`

### Pull Request Guidelines
- **Small, focused PRs** - easier to review and merge
- **Descriptive titles** and detailed descriptions
- **Link to tickets/issues** using keywords (fixes #123)
- **Include testing instructions** for reviewers
- **Require 2+ approvals** for critical code
- **Run CI/CD checks** before merging

---

## Development Workflow

### Definition of Done
A task is considered complete when:
- [ ] **Code is implemented** according to requirements
- [ ] **Unit tests written** and passing (80%+ coverage)
- [ ] **Integration tests** passing (if applicable)
- [ ] **Code reviewed** and approved by 2+ team members
- [ ] **Documentation updated** (README, API docs, comments)
- [ ] **Manual testing completed** in development environment
- [ ] **Performance impact assessed** (if applicable)
- [ ] **Security review completed** (for security-sensitive features)
- [ ] **Accessibility requirements met** (for user-facing features)
- [ ] **Ready for deployment** to staging/production

### Code Review Standards
- **Review for:** logic errors, security issues, performance problems, maintainability
- **Be constructive:** explain the "why" behind suggestions
- **Ask questions** instead of making demands
- **Praise good code** - positive reinforcement
- **Focus on the code,** not the person
- **Approve quickly** for minor changes, block for major issues

### CI/CD Pipeline
- **Automated testing** on every push/PR
- **Code quality checks** (linting, formatting, complexity)
- **Security scanning** for dependencies and code
- **Build and deployment** automation
- **Rollback capability** for production deployments

---

## Project-Specific Guidelines

### [Replace this section with project-specific information]

#### Business Logic
- [Add domain-specific rules and constraints]
- [Document business processes and workflows]
- [Include regulatory or compliance requirements]

#### Third-Party Integrations
- [List external APIs and services used]
- [Document authentication and rate limiting]
- [Include error handling for external failures]

#### Deployment & Infrastructure
- [Document deployment process and environments]
- [Include infrastructure as code practices]
- [Document monitoring and alerting setup]

---

## Team & Communication

### Team Structure
- **Tech Lead:** [Name] - Architecture decisions, technical direction
- **Senior Developers:** [Names] - Code review, mentoring, complex features
- **Developers:** [Names] - Feature development, bug fixes
- **QA Engineer:** [Name] - Testing strategy, quality assurance
- **DevOps Engineer:** [Name] - Infrastructure, deployment, monitoring

### Communication Channels
- **Daily Standups:** [Time/Platform] - Progress updates, blockers
- **Code Reviews:** [Platform] - Technical discussions
- **Architecture Discussions:** [Platform/Meeting] - Design decisions
- **Documentation:** [Wiki/Confluence] - Technical documentation

### Escalation Process
1. **Technical Issues:** Developer → Senior Developer → Tech Lead
2. **Blocking Issues:** Immediate communication in team chat
3. **Architecture Decisions:** Schedule architecture review meeting

---

## Tools & Environment

### Development Tools
- **IDE/Editor:** [VS Code | IntelliJ | PyCharm | etc.]
- **Version Control:** Git with [GitHub | GitLab | Bitbucket]
- **Package Manager:** [npm | yarn | pip | maven | etc.]
- **Build Tools:** [Webpack | Vite | Gradle | MSBuild | etc.]
- **Testing:** [Jest | Pytest | JUnit | etc.]

### Monitoring & Observability
- **Application Monitoring:** [DataDog | New Relic | AppInsights | etc.]
- **Error Tracking:** [Sentry | Rollbar | Bugsnag | etc.]
- **Logging:** [ELK Stack | Splunk | CloudWatch | etc.]
- **Performance Monitoring:** [Lighthouse | WebPageTest | etc.]

### Productivity Tools
- **Project Management:** [Jira | Linear | Asana | etc.]
- **Communication:** [Slack | Teams | Discord | etc.]
- **Documentation:** [Notion | Confluence | GitBook | etc.]
- **Design:** [Figma | Sketch | Adobe XD | etc.]

---

## Maintenance & Evolution

### Technical Debt Management
- **Regular refactoring** sessions scheduled
- **Track technical debt** in backlog with priority
- **Balance feature development** with maintenance
- **Code quality metrics** monitored over time

### Dependency Management
- **Keep dependencies up to date** with regular audits
- **Security vulnerability scanning** in CI/CD
- **Document major version upgrades** with migration guides
- **Use exact versions** in production deployments

### Knowledge Management
- **Code documentation** kept current with changes
- **Team knowledge sharing** sessions
- **Onboarding documentation** for new team members
- **Post-mortem documentation** for incidents

---

## Compliance & Standards

### Code Quality Metrics
- **Code Coverage:** Minimum 80% for new code
- **Cyclomatic Complexity:** Maximum 10 per function/method
- **Technical Debt Ratio:** Keep below 5%
- **Security Vulnerabilities:** Zero high/critical in production

### Performance Standards
- **API Response Time:** < 200ms for 95th percentile
- **Page Load Time:** < 2 seconds for initial load
- **Database Query Time:** < 100ms for simple queries
- **Error Rate:** < 0.1% in production

### Accessibility Standards (if applicable)
- **WCAG 2.1 AA compliance** for user-facing features
- **Keyboard navigation** support
- **Screen reader compatibility**
- **Color contrast ratios** meet minimum requirements

---

## Getting Started Checklist

When starting work on this project:

- [ ] Clone repository and set up development environment
- [ ] Install required dependencies and tools
- [ ] Run test suite to ensure everything works
- [ ] Read project documentation and understand architecture
- [ ] Set up IDE with recommended extensions and settings
- [ ] Configure git hooks for commit message format and pre-commit checks
- [ ] Join team communication channels
- [ ] Schedule onboarding session with tech lead
- [ ] Review recent pull requests to understand current development patterns
- [ ] Understand deployment process and environments

---

*This document is a living guide and should be updated as the project evolves. All team members are encouraged to contribute improvements and suggestions.*

**Document History:**
- v1.0 - [DATE] - Initial template creation
- [Add version history as document evolves]