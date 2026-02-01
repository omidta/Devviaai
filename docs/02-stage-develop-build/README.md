# Stage 2: Develop & Build

## Overview

Stage 2 transforms the Technical Blueprint Document (TBD) into working software. Acting as a **Software Engineer**, you'll implement components, build infrastructure, and create production-ready artifacts that are tested, documented, and ready for deployment.

## Table of Contents

- [Purpose & Objectives](#purpose--objectives)
- [Role Definition](#role-definition)
- [Input Requirements](#input-requirements)
- [Output Specifications](#output-specifications)
- [Process Flow](#process-flow)
- [Integration with Other Stages](#integration-with-other-stages)
- [Quality Checklist](#quality-checklist)
- [Best Practices](#best-practices)

## Purpose & Objectives

### Primary Purpose
Convert the technical design into working, tested, and documented code that implements all requirements specified in the TBD.

### Key Objectives

1. **Component Implementation**: Build all application components (COMP-APP-XXX)
2. **Infrastructure as Code**: Create IaC for all Azure resources (RES-AZ-XXX)
3. **Testing**: Implement comprehensive test coverage (unit, integration, e2e)
4. **Documentation**: Create technical documentation for code and APIs
5. **Code Quality**: Ensure maintainable, secure, and performant code
6. **Version Control**: Manage code with proper branching and commit practices
7. **Artifact Creation**: Generate deployable artifacts (containers, packages, bundles)

## Role Definition

### Software Engineer Responsibilities

As a Software Engineer in Stage 2, you are responsible for:

- **Implementation**: Writing clean, efficient, maintainable code
- **Testing**: Creating comprehensive test suites
- **Code Review**: Participating in peer reviews
- **Documentation**: Writing clear technical documentation
- **Collaboration**: Working with other engineers and stakeholders
- **Problem Solving**: Debugging and resolving technical issues
- **Quality Assurance**: Following coding standards and best practices

### Skills Required

- Programming languages (Node.js, Python, etc. per TBD)
- Cloud development (Azure SDK, ARM, etc.)
- Testing frameworks and methodologies
- Version control (Git)
- API design and development
- Database design and SQL
- Infrastructure as Code (Terraform, Bicep)
- Containerization (Docker)

## Input Requirements

### Required Inputs

1. **Technical Blueprint Document (TBD)**
   - Requirements Registry (FR-XXX, NFR-XXX)
   - Component Catalog (COMP-APP-XXX, COMP-INF-XXX)
   - Resource Inventory (RES-AZ-XXX)
   - Security Design (SEC-XXX)
   - Task Backlog (TASK-XXX)

2. **Development Environment**
   - Access to development Azure subscription
   - Development tools installed (IDE, SDKs, CLI tools)
   - Access to code repository
   - Access to required services (databases, APIs)

3. **Standards & Guidelines**
   - Coding standards
   - API design guidelines
   - Security guidelines
   - Documentation templates

### Optional Inputs

- Existing codebase (for modernization projects)
- Reference implementations
- UI/UX designs
- API specifications (OpenAPI)

## Output Specifications

### Primary Outputs

#### 1. Application Components

**For each COMP-APP-XXX**:
- Source code in appropriate language
- Unit tests (>80% coverage)
- Integration tests
- API documentation (if applicable)
- README with setup instructions
- Configuration files (environment templates)

**Example Structure**:
```
comp-app-001-auth-service/
├── src/
│   ├── controllers/
│   ├── services/
│   ├── models/
│   └── middleware/
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
│   ├── API.md
│   └── SETUP.md
├── Dockerfile
├── package.json
├── .env.example
└── README.md
```

#### 2. Infrastructure as Code

**For all RES-AZ-XXX**:
- Terraform modules or Bicep templates
- Variable definitions
- State management configuration
- README with deployment instructions
- Validation tests

**Example Structure**:
```
infrastructure/
├── modules/
│   ├── app-service/
│   ├── postgresql/
│   ├── redis/
│   └── networking/
├── environments/
│   ├── dev/
│   ├── staging/
│   └── production/
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md
```

#### 3. Test Suites

**Test Coverage**:
- Unit tests: >80% code coverage
- Integration tests: All component interactions
- End-to-end tests: Critical user flows
- Performance tests: NFR validation
- Security tests: Vulnerability scanning

#### 4. Documentation

**Technical Documentation**:
- API documentation (OpenAPI/Swagger)
- Database schema documentation
- Architecture decision records (ADRs)
- Setup and configuration guides
- Developer onboarding guide

#### 5. Build Artifacts

**Deployable Artifacts**:
- Container images (tagged and versioned)
- Application packages (npm, pip, etc.)
- IaC modules (tested and validated)
- Configuration files
- Migration scripts

### Supporting Outputs

- **Code Review Records**: PR reviews and approvals
- **Test Reports**: Coverage and results
- **Security Scan Reports**: Vulnerability assessments
- **Performance Test Results**: Load test outcomes
- **Build Logs**: CI pipeline execution logs

## Process Flow

### Step-by-Step Process

```
1. Environment Setup
   │
   ├─→ Clone repository
   ├─→ Install dependencies
   ├─→ Configure development environment
   └─→ Set up local database/services
   │
   ▼
2. Component Implementation (per TASK-XXX)
   │
   ├─→ Create branch (feature/TASK-XXX)
   ├─→ Implement functionality
   ├─→ Write unit tests
   ├─→ Run tests locally
   └─→ Commit with meaningful messages
   │
   ▼
3. Infrastructure as Code Development
   │
   ├─→ Write Terraform/Bicep code
   ├─→ Define variables and outputs
   ├─→ Test in dev environment
   └─→ Document usage
   │
   ▼
4. Integration
   │
   ├─→ Integrate components
   ├─→ Write integration tests
   ├─→ Test component interactions
   └─→ Resolve integration issues
   │
   ▼
5. Quality Assurance
   │
   ├─→ Run full test suite
   ├─→ Check code coverage
   ├─→ Run linting and code quality tools
   ├─→ Perform security scanning
   └─→ Review against NFRs
   │
   ▼
6. Documentation
   │
   ├─→ Write API documentation
   ├─→ Update README files
   ├─→ Document configuration
   └─→ Create setup guides
   │
   ▼
7. Code Review
   │
   ├─→ Create pull request
   ├─→ Address review comments
   ├─→ Get approval
   └─→ Merge to main branch
   │
   ▼
8. Artifact Creation
   │
   ├─→ Build container images
   ├─→ Tag with version numbers
   ├─→ Push to registry
   └─→ Validate artifacts
```

## Integration with Other Stages

### Receives from Stage 1 (Discover & Design)

- **TBD Document**: Complete technical specification
- **Task IDs**: Specific work items (TASK-XXX)
- **Component Specs**: What to build (COMP-XXX)
- **Technology Stack**: Languages, frameworks, tools
- **Requirements**: All FR-XXX and NFR-XXX

### Provides to Stage 3 (Deploy & Deliver)

- **Deployable Artifacts**: Container images, packages
- **IaC Modules**: Infrastructure provisioning code
- **Configuration Templates**: Environment configurations
- **Deployment Instructions**: How to deploy
- **Test Suites**: Validation tests for deployment

### Provides to Stage 4 (Validate & Operate)

- **Application Logs**: Structured logging implementation
- **Health Endpoints**: Monitoring endpoints
- **Metrics**: Performance and business metrics
- **Documentation**: Operational guides
- **Troubleshooting Guides**: Common issues and solutions

### Provides Feedback to Stage 1

- **Design Issues**: Infeasible or problematic designs
- **Missing Requirements**: Discovered gaps
- **Technical Constraints**: Implementation blockers
- **Effort Estimates**: Actual vs. estimated effort

## Quality Checklist

### Before Completing Stage 2

#### Code Quality
- [ ] All code follows team coding standards
- [ ] Code is properly commented and documented
- [ ] No hardcoded secrets or credentials
- [ ] Error handling is comprehensive
- [ ] Logging is implemented consistently
- [ ] Code has been peer reviewed

#### Testing
- [ ] Unit test coverage ≥ 80%
- [ ] All unit tests pass
- [ ] Integration tests cover component interactions
- [ ] End-to-end tests cover critical paths
- [ ] Performance tests validate NFRs
- [ ] Security scanning shows no critical issues

#### Documentation
- [ ] README exists for each component
- [ ] API documentation is complete (OpenAPI/Swagger)
- [ ] Setup instructions are clear and tested
- [ ] Configuration options are documented
- [ ] ADRs document key technical decisions

#### Infrastructure as Code
- [ ] IaC code is modular and reusable
- [ ] Variables are properly defined
- [ ] Outputs are documented
- [ ] Code has been validated in dev environment
- [ ] State management is configured

#### Artifacts
- [ ] Container images build successfully
- [ ] Images are properly tagged (version)
- [ ] Images are pushed to registry
- [ ] Application packages are versioned
- [ ] Migration scripts are tested

#### Requirements Traceability
- [ ] All TASK-XXX items are completed
- [ ] Each TASK implements its referenced FR-XXX
- [ ] All COMP-XXX components are implemented
- [ ] NFR-XXX requirements are validated with tests

### Quality Attributes

- **Functionality**: Implements all specified requirements
- **Reliability**: Handles errors gracefully
- **Performance**: Meets NFR performance targets
- **Security**: Follows security best practices
- **Maintainability**: Code is clean and well-structured
- **Testability**: Comprehensive test coverage
- **Deployability**: Artifacts are ready for deployment

## Best Practices

### Do's ✅

1. **Follow TDD**: Write tests before or alongside implementation
2. **Small Commits**: Commit frequently with meaningful messages
3. **Branch Strategy**: Use feature branches for each task
4. **Code Reviews**: Always get peer review before merging
5. **Continuous Testing**: Run tests on every commit
6. **Document As You Go**: Don't leave documentation for the end
7. **Security First**: Never commit secrets, use environment variables
8. **Consistent Naming**: Follow team naming conventions
9. **DRY Principle**: Don't repeat yourself, reuse code
10. **SOLID Principles**: Write modular, maintainable code

### Don'ts ❌

1. **Don't Skip Tests**: Testing is not optional
2. **Don't Ignore Warnings**: Fix linting and compiler warnings
3. **Don't Hardcode**: Use configuration for environment-specific values
4. **Don't Over-Engineer**: Implement what's in the TBD, not more
5. **Don't Work in Isolation**: Communicate with team regularly
6. **Don't Ignore Performance**: Profile and optimize critical paths
7. **Don't Skip Code Review**: Even small changes benefit from review
8. **Don't Commit Directly to Main**: Always use pull requests
9. **Don't Leave TODOs**: Complete tasks or create tickets
10. **Don't Forget Error Handling**: Every function needs proper error handling

## Common Development Patterns

### Application Component Pattern

Each COMP-APP-XXX should follow this structure:

1. **Controllers/Handlers**: Handle HTTP requests
2. **Services**: Implement business logic
3. **Repositories**: Handle data access
4. **Models**: Define data structures
5. **Middleware**: Cross-cutting concerns (auth, logging)
6. **Config**: Configuration management
7. **Utils**: Shared utilities

### Infrastructure as Code Pattern

```hcl
# Module structure
module "app_service" {
  source = "./modules/app-service"
  
  name                = var.app_service_name
  resource_group_name = azurerm_resource_group.main.name
  location            = var.location
  
  # Component reference
  implements = "COMP-APP-001"
  
  # Resource reference
  resource_id = "RES-AZ-001"
  
  tags = local.common_tags
}
```

### Testing Pattern

```javascript
// Unit test structure
describe('COMP-APP-001: Authentication Service', () => {
  describe('POST /auth/login', () => {
    it('should return JWT token for valid credentials (FR-001)', async () => {
      // Arrange
      const credentials = { username: 'test', password: 'test123' };
      
      // Act
      const response = await request(app)
        .post('/auth/login')
        .send(credentials);
      
      // Assert
      expect(response.status).toBe(200);
      expect(response.body.token).toBeDefined();
    });
    
    it('should return 401 for invalid credentials', async () => {
      // Test implementation
    });
  });
});
```

### API Documentation Pattern

```yaml
# OpenAPI 3.0 specification
openapi: 3.0.0
info:
  title: Authentication Service (COMP-APP-001)
  version: 1.0.0
  description: Implements FR-001 (Azure AD Authentication)
  
paths:
  /auth/login:
    post:
      summary: Authenticate user
      description: Authenticate user via Azure AD (SEC-AUTH-001)
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginResponse'
```

## Development Workflow

### Daily Workflow

1. **Morning**: Pull latest changes, review assigned tasks
2. **Development**: Implement, test, commit (TDD cycle)
3. **Mid-Day**: Push changes, create PR if feature complete
4. **Code Review**: Review team members' PRs
5. **End of Day**: Update task status, document blockers

### Task Development Workflow

```bash
# 1. Start task
git checkout main
git pull origin main
git checkout -b feature/TASK-001

# 2. Implement
# - Write code
# - Write tests
# - Run tests locally

# 3. Commit
git add .
git commit -m "feat(COMP-APP-001): implement user authentication [TASK-001]"

# 4. Push and PR
git push origin feature/TASK-001
# Create PR with description referencing TASK-001, FR-001

# 5. Code Review
# - Address feedback
# - Update PR
# - Get approval

# 6. Merge
# - Merge PR
# - Delete branch
# - Update task status
```

## Templates Usage

### Component Template

Use [TEMPLATE-COMPONENT.md](TEMPLATE-COMPONENT.md) for:
- Creating new application components
- Consistent structure across components
- Complete implementation checklist

### Infrastructure Template

Use [TEMPLATE-INFRASTRUCTURE.md](TEMPLATE-INFRASTRUCTURE.md) for:
- Creating IaC modules
- Resource provisioning
- Environment configuration

## Getting Started

### Using This Stage

1. **Review the TBD**: Understand all requirements and design
2. **Review the Master Prompt**: See [PROMPT.md](PROMPT.md) for AI-assisted development
3. **Set Up Environment**: Configure your development environment
4. **Use Templates**: Follow component and infrastructure templates
5. **Start Development**: Pick tasks from backlog and implement
6. **Test Continuously**: Write and run tests as you develop
7. **Document**: Keep documentation up to date

### Example Usage

```bash
# 1. Set up development environment
git clone <repository>
cd <repository>
npm install  # or pip install -r requirements.txt

# 2. Review TBD
cat docs/technical-blueprint.md

# 3. Pick a task
# TASK-001: Implement Authentication Service

# 4. Use the master prompt from PROMPT.md with AI assistant

# 5. Create branch and implement
git checkout -b feature/TASK-001

# 6. Use component template
# Follow TEMPLATE-COMPONENT.md structure

# 7. Develop with TDD
# Write tests, implement code, refactor

# 8. Create PR and get review

# 9. Merge and mark task complete
```

## Tools & Technologies

### Development Tools
- **IDE**: VS Code, IntelliJ, PyCharm
- **Version Control**: Git, GitHub
- **Package Managers**: npm, pip, Maven
- **Containerization**: Docker, Docker Compose

### Testing Tools
- **Unit Testing**: Jest, pytest, JUnit
- **Integration Testing**: Supertest, pytest
- **E2E Testing**: Playwright, Cypress
- **Coverage**: Istanbul, Coverage.py
- **Load Testing**: k6, JMeter

### Code Quality Tools
- **Linting**: ESLint, Pylint, Flake8
- **Formatting**: Prettier, Black
- **Security Scanning**: Snyk, OWASP Dependency Check
- **Code Review**: GitHub PR reviews, SonarQube

### IaC Tools
- **Terraform**: For Azure resource provisioning
- **Bicep**: Alternative to Terraform for Azure
- **Validation**: tflint, terraform validate
- **Documentation**: terraform-docs

## Resources

- **[Master Prompt](PROMPT.md)**: AI-ready prompt for this stage
- **[Component Template](TEMPLATE-COMPONENT.md)**: Structure for application components
- **[Infrastructure Template](TEMPLATE-INFRASTRUCTURE.md)**: Structure for IaC modules
- **[Framework Overview](../00-overview/README.md)**: Understand the full framework
- **[Stage 1 Output](../01-stage-discover-design/README.md)**: TBD document reference

---

[Back to Documentation Index](../README.md) | [Stage 2 Prompt](PROMPT.md) | [Previous: Stage 1](../01-stage-discover-design/README.md) | [Next: Stage 3](../03-stage-deploy-deliver/README.md)
