# Best Practices for Coding Repositories

This guide outlines best practices for using Claude Code Web effectively when working with code repositories, maintaining code quality, and collaborating with teams.

## Repository Organization

### Project Structure

Maintain a clear, consistent project structure that Claude can easily understand:

```plaintext
my-project/
├── src/
│   ├── components/
│   ├── services/
│   ├── utils/
│   └── config/
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
├── scripts/
├── .gitignore
├── README.md
├── requirements.txt (or package.json)
└── docker-compose.yml
```

!!! tip "Communicating Structure to Claude"
    Start your session by describing your project structure:

    ```
    "I'm working on a Python microservice with this structure:
    - src/ contains business logic
    - src/api/ has FastAPI routes
    - src/models/ has SQLAlchemy models
    - tests/ mirrors src/ structure"
    ```

### Documentation Standards

#### README.md Best Practices

Your README should include:

```markdown
# Project Name

## Description
Brief overview of what the project does

## Tech Stack
- Language/Framework versions
- Key dependencies
- Database system

## Setup
```bash
# Installation steps
pip install -r requirements.txt
```

## Project Structure
Overview of directory organization

## Development
How to run locally, tests, linting

## Contributing
Guidelines for contributors
```

When asking Claude to work on your project, reference your README:

```
"Based on our README setup instructions, add a new database migration for [feature]"
```

### Version Control Best Practices

#### .gitignore Essentials

Ask Claude to generate comprehensive .gitignore files:

!!! example "Effective Request"
    ```
    "Generate a .gitignore file for a Node.js/React project that:
    - Excludes node_modules and build artifacts
    - Ignores IDE-specific files (VS Code, IntelliJ)
    - Excludes environment variables and secrets
    - Ignores OS-specific files (macOS, Windows, Linux)
    - Includes comments explaining each section"
    ```

#### Commit Message Standards

Use Claude to help maintain consistent commit messages:

```
"Review this commit message and improve it following conventional commits:

'fixed bug'

The changes: Added null checking to user authentication and fixed email validation regex"
```

Claude's improved version:
```
fix(auth): add null checking and fix email validation

- Add null/undefined checks before accessing user object properties
- Fix email validation regex to properly handle plus addressing
- Add test cases for edge cases

Resolves #123
```

## Code Quality Standards

### Linting and Formatting

#### Setting Up Linters

Ask Claude to configure linting tools:

!!! example "Python Project"
    ```
    "Set up a Python code quality stack with:
    - Black for formatting
    - Flake8 for linting
    - mypy for type checking
    - isort for import sorting

    Provide:
    1. Installation commands
    2. Configuration files (pyproject.toml, .flake8, etc.)
    3. Pre-commit hook setup
    4. VS Code settings for auto-formatting"
    ```

#### Code Style Consistency

Specify your project's style guide when requesting code:

```
"Generate a React component following our style guide:
- Functional components with hooks
- TypeScript with strict mode
- Props destructuring
- Named exports (not default)
- JSDoc comments for public functions
- Styled-components for CSS"
```

### Testing Practices

#### Test Coverage Standards

Establish minimum coverage expectations:

```
"Write comprehensive tests for this user service:
- Unit tests for each public method
- Integration tests for database operations
- Edge cases and error conditions
- Aim for >90% code coverage
- Use pytest with fixtures for setup"
```

#### Test Organization

Mirror your source structure in tests:

```plaintext
src/
  services/
    user_service.py
    payment_service.py
tests/
  services/
    test_user_service.py
    test_payment_service.py
```

Request tests that follow this pattern:

```
"Our tests mirror the src/ structure. Create tests for src/services/order_service.py
in tests/services/test_order_service.py following this convention"
```

### Code Review Integration

#### Pre-Review Checklist

Use Claude for pre-review checks:

```
"Before I submit this PR, review the code for:

1. Code Quality
   - Follows project style guide
   - No code duplication
   - Appropriate abstraction levels

2. Testing
   - Unit tests for new functions
   - Edge cases covered
   - Mocks properly used

3. Security
   - Input validation
   - SQL injection prevention
   - XSS protection
   - Secrets not hardcoded

4. Performance
   - No N+1 queries
   - Appropriate caching
   - Efficient algorithms

5. Documentation
   - Updated README if needed
   - Inline comments for complex logic
   - API documentation updated

Code: [paste here]"
```

## Dependency Management

### Package Management Best Practices

#### Lock Files

Always commit lock files and ask Claude to work within their constraints:

```
"Update our Express server to use helmet for security.
We use npm with package-lock.json committed.
Ensure the change is compatible with our current Node version (16.x)"
```

#### Security Audits

Regular security checks:

```
"Review our package.json dependencies:

1. Identify any packages with known vulnerabilities
2. Suggest secure alternatives where needed
3. Check for unused dependencies
4. Recommend updates (considering breaking changes)
5. Provide upgrade commands

Current package.json: [paste]"
```

### Dependency Updates

Systematic approach to updates:

```
"Help me update React from 17 to 18:

1. What breaking changes should I know about?
2. What code patterns need updating?
3. Are all our dependencies compatible?
4. Provide a step-by-step migration plan
5. Identify any deprecated features we use

Our current dependencies: [paste package.json]"
```

## Configuration Management

### Environment Variables

Use Claude to set up proper environment management:

!!! example "Environment Setup"
    ```
    "Set up environment variable management for a Node.js app:

    1. Create .env.example with all required variables (no values)
    2. Add .env to .gitignore
    3. Set up dotenv configuration
    4. Create environment validation at startup
    5. Document each variable in README

    Variables we need:
    - Database connection
    - API keys (3rd party services)
    - JWT secret
    - Environment (dev/staging/prod)
    - Port number"
    ```

### Configuration Files

Maintain separate configs for different environments:

```
config/
  ├── default.json
  ├── development.json
  ├── staging.json
  └── production.json
```

Request environment-specific configs:

```
"Create production configuration that:
- Disables debug logging
- Enables request rate limiting
- Uses connection pooling (size: 20)
- Sets strict CORS policies
- Enables compression
- Based on our development config: [paste]"
```

## Security Best Practices

### Code Security

Always request security considerations:

```
"Implement user authentication with:
- Password hashing (bcrypt, min cost factor 12)
- JWT tokens (refresh + access pattern)
- CSRF protection
- Rate limiting on login attempts
- Secure session management
- Input sanitization

Include security best practices and explain each measure"
```

### Secrets Management

Never commit secrets:

```
"Our API keys are currently hardcoded. Refactor to:
1. Use environment variables
2. Add secrets to .gitignore
3. Create secret management documentation
4. Implement validation for missing secrets
5. Show how to use in different environments

Current code: [paste]"
```

### Security Scanning

Regular security audits:

```
"Audit this code for security vulnerabilities:
- SQL injection risks
- XSS attack vectors
- CSRF vulnerabilities
- Authentication bypasses
- Authorization flaws
- Information disclosure
- Insecure dependencies

Code: [paste]

For each issue:
1. Explain the vulnerability
2. Show how it could be exploited
3. Provide secure implementation
4. Include test to verify fix"
```

## Performance Optimization

### Profiling and Benchmarking

Use Claude to set up performance monitoring:

```
"Add performance monitoring to this Express API:
1. Response time tracking per endpoint
2. Memory usage monitoring
3. Database query performance logging
4. Setup prometheus metrics
5. Create grafana dashboard config

Provide:
- Middleware implementation
- Metric definitions
- Dashboard JSON
- Documentation on interpreting metrics"
```

### Database Optimization

Request optimized database interactions:

```
"Optimize this database query:

Current: [paste SQL/ORM code]
Problem: Takes 5 seconds with 10K records

Analyze:
1. What's causing the slowness?
2. What indexes should we add?
3. Can we reduce data fetching?
4. Should we use caching?
5. Provide optimized version

Database: PostgreSQL 14
Current indexes: [list]"
```

## CI/CD Integration

### Automated Testing

Set up comprehensive CI pipelines:

!!! example "GitHub Actions Workflow"
    ```
    "Create a GitHub Actions workflow that:

    On PR:
    - Runs linting (ESLint, Prettier)
    - Runs type checking (TypeScript)
    - Runs unit tests with coverage
    - Runs integration tests
    - Builds the application
    - Comments coverage report on PR

    On merge to main:
    - All of the above
    - Builds Docker image
    - Pushes to container registry
    - Deploys to staging
    - Runs smoke tests

    Tech stack: Node.js 18, Docker, Jest
    Deployment: AWS ECS"
    ```

### Deployment Best Practices

Request deployment configurations:

```
"Create a production-ready Docker setup:

1. Multi-stage Dockerfile (build + runtime)
2. docker-compose.yml for local development
3. .dockerignore file
4. Health check endpoint
5. Graceful shutdown handling
6. Container security best practices

Application: Python FastAPI
Dependencies: PostgreSQL, Redis"
```

## Team Collaboration

### Code Review Guidelines

Establish review standards:

```
"Create a code review checklist for our team:

Categories to check:
1. Functionality (works as intended)
2. Code quality (readable, maintainable)
3. Testing (adequate coverage)
4. Security (no vulnerabilities)
5. Performance (no obvious bottlenecks)
6. Documentation (updated as needed)

Format as markdown for our CONTRIBUTING.md"
```

### Onboarding New Developers

Use Claude to create onboarding documentation:

```
"Create an onboarding guide for new developers:

1. Local environment setup (step-by-step)
2. Architecture overview
3. Key code patterns we use
4. How to run tests
5. How to submit PRs
6. Who to ask for help

Our stack: [describe]
Our structure: [describe]
Our processes: [describe]"
```

## Maintenance Practices

### Technical Debt Management

Track and address technical debt:

```
"Review this module for technical debt:

Identify:
1. Code smells (duplication, long functions, etc.)
2. Outdated patterns
3. Missing tests
4. TODO comments that should be addressed
5. Performance issues

For each:
- Severity (high/medium/low)
- Estimated effort to fix
- Impact of not fixing
- Suggested approach

Code: [paste]"
```

### Refactoring Strategies

Safe refactoring approach:

```
"Help me refactor this legacy code:

Current code: [paste]

Requirements:
1. Maintain exact same external behavior
2. Improve readability and maintainability
3. Add type hints/annotations
4. Extract reusable components
5. Add comprehensive tests
6. Update documentation

Provide:
- Step-by-step refactoring plan
- Tests to verify behavior unchanged
- Refactored code
- Migration guide"
```

### Documentation Maintenance

Keep documentation current:

```
"Our API has changed. Update the documentation:

Old API docs: [paste]

Changes:
- New endpoint: POST /api/users/{id}/preferences
- Removed endpoint: GET /api/legacy/users
- Updated: PUT /api/users now requires 'email_verified' field

Provide:
1. Updated API documentation
2. Migration guide for API consumers
3. Changelog entry
4. Updated OpenAPI/Swagger spec"
```

## Repository Hygiene

### Regular Cleanup

Maintain a clean repository:

```
"Audit our repository for cleanup:

1. Identify unused dependencies in package.json
2. Find dead code (unused functions/classes)
3. Locate TODO/FIXME comments
4. Check for outdated documentation
5. Find large files that should be in .gitignore
6. Identify test files without actual tests

Repository structure: [describe or provide tree output]"
```

### Branch Management

Establish branching strategy:

```
"Document our Git branching strategy:

Branches:
- main: production code
- develop: integration branch
- feature/*: new features
- bugfix/*: bug fixes
- hotfix/*: urgent production fixes

Include:
1. Branch creation guidelines
2. Naming conventions
3. When to merge where
4. PR requirements
5. Example workflow diagram

Format for CONTRIBUTING.md"
```

## Monitoring and Logging

### Logging Standards

Consistent logging practices:

```
"Add structured logging to this service:

Requirements:
- Use structured JSON logs
- Include correlation IDs
- Log levels: DEBUG, INFO, WARNING, ERROR
- Include context (user_id, request_id, etc.)
- Never log sensitive data (passwords, tokens)
- Integrate with our ELK stack

Current logging: [paste code]
Language: Python
Framework: FastAPI"
```

### Error Tracking

Set up error monitoring:

```
"Integrate error tracking:

1. Set up Sentry (or equivalent)
2. Configure error grouping
3. Add custom context to errors
4. Set up alerts for critical errors
5. Add source maps for stack traces
6. Configure error sampling rules

Application: React frontend + Node.js backend
Requirements: Track JS errors, API errors, and performance issues"
```

## Quick Reference Checklist

Before committing code reviewed by Claude:

- [ ] Code follows project style guide
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] No secrets or credentials committed
- [ ] Security considerations addressed
- [ ] Performance impact considered
- [ ] Backwards compatibility maintained (or migration plan provided)
- [ ] Dependencies updated in lock files
- [ ] CI/CD pipeline passes
- [ ] Code reviewed (by Claude and human)

## Next Steps

- Explore [Research Use Cases](research-use-cases.md) for non-coding applications
- Review [Non-Code Examples](non-code-examples.md) for broader usage
- Master [Advanced Usage](advanced-usage.md) techniques for complex scenarios

Remember: These best practices evolve with your team and project. Regularly review and update them based on lessons learned.
