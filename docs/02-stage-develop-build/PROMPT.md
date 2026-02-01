# Stage 2: Master Prompt - Develop & Build

## Instructions for Use

1. Copy this entire prompt
2. Replace all `[PLACEHOLDER]` values with your project-specific information
3. Paste into your AI assistant (GitHub Copilot Chat, ChatGPT, Claude, etc.)
4. Review and refine the generated code, tests, and infrastructure
5. Follow the templates provided in this stage

---

## Master Prompt

```
You are an expert Software Engineer specializing in cloud-native application development on Azure. Your task is to implement the components and infrastructure specified in the Technical Blueprint Document (TBD) following the Devviaai framework.

### PROJECT CONTEXT

**Project Name**: [PROJECT_NAME]
**Stage**: Develop & Build (Stage 2)
**Component/Task**: [TASK_ID - TASK_NAME]
**Technology Stack**: [PRIMARY_TECHNOLOGIES]

### INPUT: TECHNICAL BLUEPRINT DOCUMENT

**Relevant Requirements**:
[PASTE_RELEVANT_FR_XXX_AND_NFR_XXX_HERE]

**Component Specification**:
[PASTE_RELEVANT_COMP_XXX_SPECIFICATION_HERE]

**Resource Specification**:
[PASTE_RELEVANT_RES_AZ_XXX_SPECIFICATION_HERE]

**Security Requirements**:
[PASTE_RELEVANT_SEC_XXX_REQUIREMENTS_HERE]

**Task Details**:
[PASTE_TASK_XXX_DETAILS_HERE]

### YOUR TASK

Based on the TBD specifications above, generate a complete, production-ready implementation for this component/task.

#### IMPLEMENTATION TYPE

Select what you need to generate:

**Option A: Application Component** (if implementing COMP-APP-XXX)
Generate:
1. Complete source code with proper structure
2. Unit tests with ≥80% coverage
3. Integration tests for external dependencies
4. API documentation (OpenAPI/Swagger)
5. README with setup instructions
6. Dockerfile for containerization
7. Environment configuration template
8. Error handling and logging

**Option B: Infrastructure as Code** (if implementing infrastructure)
Generate:
1. Terraform modules (or Bicep templates)
2. Variable definitions with descriptions
3. Output definitions
4. README with usage instructions
5. Example configuration files
6. Validation and testing scripts

**Option C: Both** (if task includes both application and infrastructure)
Generate all of the above.

### REQUIREMENTS FOR APPLICATION COMPONENTS

#### 1. PROJECT STRUCTURE

Follow this structure (adapt for your language/framework):

```
[component-name]/
├── src/
│   ├── controllers/       # HTTP handlers
│   ├── services/          # Business logic
│   ├── repositories/      # Data access
│   ├── models/            # Data structures
│   ├── middleware/        # Cross-cutting concerns
│   ├── config/            # Configuration
│   └── utils/             # Shared utilities
├── tests/
│   ├── unit/              # Unit tests
│   ├── integration/       # Integration tests
│   └── fixtures/          # Test data
├── docs/
│   ├── API.md             # API documentation
│   └── SETUP.md           # Setup guide
├── .env.example           # Environment template
├── Dockerfile             # Container definition
├── package.json           # Dependencies (or equivalent)
├── README.md              # Component overview
└── [language-specific files]
```

#### 2. CODE REQUIREMENTS

- **Coding Standards**: Follow industry best practices for [LANGUAGE]
- **Error Handling**: Comprehensive try-catch with proper error responses
- **Logging**: Structured logging (JSON format) with appropriate levels
- **Configuration**: Use environment variables, no hardcoded values
- **Security**: No secrets in code, validate all inputs, sanitize outputs
- **Comments**: Clear comments for complex logic, JSDoc/docstrings for functions
- **Naming**: Descriptive names, follow language conventions
- **Dependencies**: Use stable, maintained packages only

#### 3. TESTING REQUIREMENTS

**Unit Tests**:
- Test each function/method independently
- Mock external dependencies
- Cover happy path and error cases
- Achieve ≥80% code coverage
- Use descriptive test names referencing requirements

**Integration Tests**:
- Test component interactions
- Test database operations (if applicable)
- Test API endpoints end-to-end
- Test authentication/authorization
- Use test databases/services

**Test Structure**:
```javascript
describe('[COMP-ID]: [Component Name]', () => {
  describe('[Function/Endpoint Name]', () => {
    it('should [expected behavior] when [condition] (implements [FR-XXX])', () => {
      // Arrange
      // Act
      // Assert
    });
  });
});
```

#### 4. API DOCUMENTATION

Generate OpenAPI 3.0 specification including:
- All endpoints with descriptions
- Request/response schemas
- Authentication requirements
- Error responses
- Examples for each endpoint
- References to TBD items (FR-XXX, COMP-XXX)

#### 5. CONTAINERIZATION

Create a production-ready Dockerfile:
- Multi-stage build (if applicable)
- Minimal base image (alpine, distroless)
- Non-root user
- Health check
- Proper labels
- Optimized layers

#### 6. DOCUMENTATION

**README.md** should include:
- Component overview with TBD references
- Prerequisites
- Installation instructions
- Configuration options
- Running the application
- Running tests
- Troubleshooting
- API documentation link

### REQUIREMENTS FOR INFRASTRUCTURE AS CODE

#### 1. MODULE STRUCTURE

```
infrastructure/
├── modules/
│   └── [resource-type]/
│       ├── main.tf          # Resource definitions
│       ├── variables.tf     # Input variables
│       ├── outputs.tf       # Output values
│       ├── versions.tf      # Provider versions
│       └── README.md        # Usage documentation
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tfvars
│   │   └── backend.tf
│   ├── staging/
│   └── production/
└── README.md
```

#### 2. TERRAFORM CODE REQUIREMENTS

- **Modularity**: Reusable modules for each resource type
- **Variables**: All configurable values as variables with descriptions
- **Outputs**: Export important values for other modules
- **Naming**: Consistent naming with environment and project prefix
- **Tags**: Apply tags to all resources (project, environment, component)
- **State Management**: Remote state with proper locking
- **Documentation**: Comments explaining non-obvious configurations

#### 3. VARIABLE DEFINITIONS

For each variable:
```hcl
variable "app_service_name" {
  description = "Name of the App Service (implements COMP-APP-001, RES-AZ-001)"
  type        = string
  validation {
    condition     = can(regex("^[a-z0-9-]+$", var.app_service_name))
    error_message = "Name must contain only lowercase letters, numbers, and hyphens."
  }
}
```

#### 4. RESOURCE DEFINITIONS

For each resource, include:
```hcl
# RES-AZ-001: Web Application Service
# Implements: COMP-APP-001 (Authentication Service)
# Requirements: FR-001, NFR-001, NFR-004
resource "azurerm_app_service" "auth_service" {
  name                = var.app_service_name
  location            = var.location
  resource_group_name = var.resource_group_name
  app_service_plan_id = azurerm_app_service_plan.main.id

  site_config {
    # Configuration per TBD specifications
    always_on        = true
    health_check_path = "/health"
    # ... more config
  }

  tags = merge(
    var.common_tags,
    {
      Component   = "COMP-APP-001"
      Resource    = "RES-AZ-001"
      Requirement = "FR-001,NFR-001,NFR-004"
    }
  )
}
```

#### 5. README FOR IaC

Include:
- Module purpose with TBD references
- Prerequisites (Azure CLI, Terraform version)
- Usage examples
- Input variables table
- Output values table
- Deployment instructions for each environment
- Validation and testing instructions

### OUTPUT FORMAT

Provide your implementation in the following structure:

#### For Application Components:

```
## Component: [COMP-ID] - [Component Name]

### TBD References
- **Implements**: [FR-XXX, FR-XXX, ...]
- **Component ID**: [COMP-APP-XXX]
- **Resource ID**: [RES-AZ-XXX]
- **Task ID**: [TASK-XXX]

### 1. Project Structure
[List all files and directories]

### 2. Source Code

#### File: src/[filename]
```[language]
[Complete source code with comments]
```

[Repeat for all source files]

### 3. Tests

#### File: tests/unit/[filename]
```[language]
[Complete test code]
```

[Repeat for all test files]

### 4. Documentation

#### File: README.md
```markdown
[Complete README]
```

#### File: docs/API.md
```yaml
[OpenAPI specification]
```

### 5. Configuration

#### File: .env.example
```
[Environment variables]
```

#### File: Dockerfile
```dockerfile
[Complete Dockerfile]
```

### 6. Package Configuration

#### File: package.json (or equivalent)
```json
[Complete package configuration]
```
```

#### For Infrastructure as Code:

```
## Infrastructure: [Resource Name]

### TBD References
- **Resource ID**: [RES-AZ-XXX]
- **Component**: [COMP-XXX]
- **Task ID**: [TASK-XXX]

### 1. Module Structure
[List all files]

### 2. Terraform Code

#### File: modules/[resource-type]/main.tf
```hcl
[Complete Terraform code]
```

[Repeat for all Terraform files]

### 3. Documentation

#### File: README.md
```markdown
[Complete infrastructure README]
```

### 4. Environment Configurations

#### File: environments/dev/variables.tfvars
```hcl
[Development configuration]
```

[Repeat for staging and production]
```

### QUALITY REQUIREMENTS

Ensure your implementation includes:

#### Code Quality
- ✅ Follows language-specific best practices
- ✅ No hardcoded secrets or credentials
- ✅ Comprehensive error handling
- ✅ Structured logging throughout
- ✅ Input validation and sanitization
- ✅ Proper use of async/await (if applicable)
- ✅ DRY principle followed
- ✅ SOLID principles applied

#### Testing
- ✅ Unit tests with ≥80% coverage
- ✅ Integration tests for external dependencies
- ✅ All tests pass
- ✅ Test names reference requirements (FR-XXX)
- ✅ Mocks for external services
- ✅ Test fixtures for data

#### Security
- ✅ No secrets in code
- ✅ Environment variables for configuration
- ✅ Input validation
- ✅ Output sanitization
- ✅ Proper authentication/authorization
- ✅ SQL injection prevention (if applicable)
- ✅ XSS prevention (if applicable)

#### Documentation
- ✅ README with setup instructions
- ✅ API documentation (OpenAPI)
- ✅ Inline code comments
- ✅ Configuration documented
- ✅ TBD references throughout

#### Infrastructure (if applicable)
- ✅ Modular and reusable
- ✅ Variables with descriptions
- ✅ Outputs defined
- ✅ Tags on all resources
- ✅ Validation rules
- ✅ Comments in code

#### Traceability
- ✅ All files reference relevant TBD items
- ✅ Tests reference requirements
- ✅ Comments explain TBD alignment
- ✅ Tags include TBD IDs

### ADDITIONAL GUIDANCE

#### Application Development
- Use dependency injection for testability
- Implement health check endpoint (/health)
- Implement metrics endpoint (/metrics)
- Use connection pooling for databases
- Implement graceful shutdown
- Use retry logic with exponential backoff
- Cache expensive operations
- Validate environment variables on startup

#### Infrastructure as Code
- Use remote state with locking
- Separate environments (dev, staging, prod)
- Use workspaces or separate state files
- Implement lifecycle rules where appropriate
- Use data sources for existing resources
- Implement depends_on where needed
- Use locals for computed values
- Plan and validate before apply

#### Testing
- Use test databases (not production)
- Clean up test data after tests
- Use fixtures for consistent test data
- Test both success and failure cases
- Test edge cases and boundaries
- Mock external API calls
- Use test containers for integration tests

#### Documentation
- Keep README up to date
- Document all environment variables
- Provide examples for common tasks
- Document troubleshooting steps
- Include links to related documentation
- Document any deviations from TBD

Begin your implementation now. Generate complete, production-ready code that fully implements the specified task.
```

---

## Example Usage

### Step 1: Identify Your Task

From the TBD, select a task to implement:
```
TASK-001: Implement Authentication Service
- Implements: FR-001, COMP-APP-001, RES-AZ-001
- Priority: High
- Effort: 5 days
```

### Step 2: Gather TBD Information

Extract relevant sections:
- FR-001: User authentication via Azure AD
- COMP-APP-001: Authentication Service specification
- RES-AZ-001: App Service specification
- SEC-AUTH-001: Authentication security requirements

### Step 3: Fill in the Placeholders

Replace:
- `[PROJECT_NAME]`: "Customer Portal"
- `[TASK_ID - TASK_NAME]`: "TASK-001 - Implement Authentication Service"
- `[PRIMARY_TECHNOLOGIES]`: "Node.js 18, Express, Passport.js"
- `[PASTE_RELEVANT_FR_XXX_AND_NFR_XXX_HERE]`: Copy FR-001, NFR-010, NFR-012
- etc.

### Step 4: Use with Your AI Assistant

**GitHub Copilot**:
```bash
# In VS Code
1. Open Copilot Chat
2. Paste the customized prompt
3. Review generated code
4. Save files in appropriate locations
5. Run tests locally
```

**ChatGPT/Claude**:
```bash
1. Start a new conversation
2. Paste the customized prompt
3. Download generated code
4. Save to your repository
5. Review and test
```

### Step 5: Validate Implementation

After generation:
```bash
# 1. Create branch
git checkout -b feature/TASK-001

# 2. Save generated files

# 3. Install dependencies
npm install  # or pip install -r requirements.txt

# 4. Run tests
npm test     # or pytest

# 5. Check coverage
npm run coverage

# 6. Run linting
npm run lint

# 7. Build container
docker build -t auth-service:latest .

# 8. Test locally
docker-compose up
```

## Follow-Up Prompts

After initial generation, you may need to refine:

### For Application Components

```
"Add rate limiting middleware to the authentication endpoints."

"Implement refresh token rotation for enhanced security."

"Add integration tests for Azure AD authentication flow."

"Improve error handling in the login controller with specific error codes."

"Add health check endpoint that verifies database and Redis connectivity."

"Generate OpenAPI documentation for all API endpoints."
```

### For Infrastructure

```
"Add auto-scaling rules based on CPU and memory metrics."

"Configure virtual network integration for the App Service."

"Add Azure Key Vault integration for secrets management."

"Create separate Terraform workspaces for dev, staging, and prod."

"Add monitoring alerts for application health and performance."

"Include backup and disaster recovery configuration."
```

### For Testing

```
"Add more test cases for edge cases and error scenarios."

"Create integration tests that use TestContainers for PostgreSQL."

"Add load tests using k6 to validate NFR-002 (response time < 200ms)."

"Generate test fixtures for common authentication scenarios."
```

## Validation Checklist

Before creating a pull request, verify:

### Code
- [ ] All files are created in correct locations
- [ ] Code follows team standards
- [ ] No hardcoded secrets or credentials
- [ ] Error handling is comprehensive
- [ ] Logging is implemented consistently
- [ ] Configuration uses environment variables

### Testing
- [ ] All tests pass locally
- [ ] Code coverage ≥ 80%
- [ ] Integration tests cover main flows
- [ ] Test names reference requirements (FR-XXX)
- [ ] Tests are independent and repeatable

### Documentation
- [ ] README.md exists and is complete
- [ ] API documentation is generated
- [ ] Setup instructions are clear
- [ ] Environment variables are documented
- [ ] TBD references are included

### Container (if applicable)
- [ ] Dockerfile builds successfully
- [ ] Image runs correctly
- [ ] Health check works
- [ ] Image size is optimized
- [ ] Non-root user is configured

### Infrastructure (if applicable)
- [ ] Terraform validates successfully
- [ ] Terraform plan shows expected resources
- [ ] Variables are documented
- [ ] Outputs are defined
- [ ] Tags include TBD references

### Traceability
- [ ] Code references TASK-XXX in commits
- [ ] Comments reference FR-XXX, COMP-XXX
- [ ] Tests reference requirements
- [ ] Tags/labels include TBD IDs

## Common Pitfalls to Avoid

### Application Development
1. **Hardcoding Configuration**: Use environment variables
2. **Poor Error Handling**: Don't just throw errors, handle them properly
3. **Missing Tests**: Write tests as you code, not after
4. **No Logging**: Implement structured logging from the start
5. **Ignoring Security**: Validate inputs, sanitize outputs, use parameterized queries
6. **Tight Coupling**: Use dependency injection for testability
7. **No Health Checks**: Always implement /health endpoint
8. **Synchronous Operations**: Use async/await for I/O operations

### Infrastructure as Code
1. **No State Management**: Always use remote state with locking
2. **Hardcoded Values**: Use variables for everything
3. **No Validation**: Add validation rules to variables
4. **Missing Tags**: Tag all resources for management
5. **Single Environment**: Separate dev, staging, production
6. **No Outputs**: Export values other modules might need
7. **Poor Naming**: Use consistent naming conventions
8. **No Documentation**: Document module usage

### Testing
1. **Testing Implementation**: Test behavior, not implementation details
2. **Dependent Tests**: Make tests independent
3. **No Mocks**: Mock external dependencies
4. **Insufficient Coverage**: Aim for >80% coverage
5. **No Integration Tests**: Test component interactions
6. **Ignoring Edge Cases**: Test boundaries and error cases

## Next Steps

After completing implementation:

1. **Local Validation**: Test everything locally
2. **Create Branch**: Follow git workflow
3. **Commit Code**: With meaningful messages referencing TASK-XXX
4. **Create PR**: With description linking to TBD
5. **Code Review**: Get peer review
6. **Address Feedback**: Update based on review comments
7. **Merge**: After approval
8. **Update Status**: Mark TASK-XXX as complete
9. **Move to Next Task**: Or proceed to Stage 3 if all tasks complete

---

[Back to Stage 2 Overview](README.md) | [Component Template](TEMPLATE-COMPONENT.md) | [Infrastructure Template](TEMPLATE-INFRASTRUCTURE.md) | [Documentation Index](../README.md)
