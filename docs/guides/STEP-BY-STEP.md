# Step-by-Step Guide

This comprehensive guide walks you through the complete Devviaai Framework workflow, from initial requirements to production operations. Follow this guide for your first complete project implementation.

## 📋 Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Stage 1: Discover & Design](#stage-1-discover--design)
- [Stage 2: Develop & Build](#stage-2-develop--build)
- [Stage 3: Deploy & Deliver](#stage-3-deploy--deliver)
- [Stage 4: Validate & Operate](#stage-4-validate--operate)
- [Integration & Iteration](#integration--iteration)
- [Troubleshooting Guide](#troubleshooting-guide)
- [Best Practices Summary](#best-practices-summary)

---

## Introduction

### What This Guide Covers

This guide provides detailed, step-by-step instructions for implementing a complete project using the Devviaai Framework. You'll learn:

- How to move through all four stages systematically
- How to use AI assistants effectively at each stage
- How to maintain traceability throughout the project
- How to integrate outputs between stages
- How to troubleshoot common issues

### Time Commitment

- **Reading Time**: 2-3 hours
- **Implementation Time**: Varies by project (typically 4-12 weeks)
- **First-Project Overhead**: Add 20-30% for learning curve

### Who Should Use This Guide

- Teams implementing their first Devviaai project
- Individuals wanting comprehensive understanding
- Team leads planning framework adoption
- Anyone needing detailed stage-by-stage guidance

---

## Prerequisites

### Required Knowledge

- [ ] Cloud computing basics (Azure preferred)
- [ ] Software development fundamentals
- [ ] Version control (Git)
- [ ] Command-line interface usage

### Required Tools

- [ ] **AI Assistant**: GitHub Copilot, ChatGPT Plus, or Claude
- [ ] **Code Editor**: VS Code, IntelliJ, or similar
- [ ] **Git**: Version control client
- [ ] **Azure Account**: With appropriate permissions
- [ ] **Azure CLI**: For resource management
- [ ] **Terraform**: Version 1.0+ (for Infrastructure as Code)

### Recommended Setup

```bash
# Install Azure CLI
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Install Terraform
wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip
unzip terraform_1.6.0_linux_amd64.zip
sudo mv terraform /usr/local/bin/

# Verify installations
az --version
terraform --version
git --version
```

### Project Preparation

Have ready:
- Project requirements document
- Stakeholder contacts
- Budget information
- Timeline expectations
- Compliance/security requirements

---

## Stage 1: Discover & Design

**Duration**: 1-5 days  
**Role**: Solution Architect  
**Outcome**: Technical Blueprint Document (TBD)

### Overview

Stage 1 transforms requirements into a comprehensive technical design. You'll analyze needs, design architecture, and create the TBD that guides all subsequent work.

### Step 1.1: Gather Requirements (1-2 days)

#### Activities

1. **Collect Source Materials**
   ```bash
   # Create project structure
   mkdir -p my-project/{docs,src,infrastructure,pipelines}
   cd my-project
   
   # Initialize Git
   git init
   git checkout -b main
   ```

2. **Interview Stakeholders**
   - Schedule 30-60 min sessions with key stakeholders
   - Document business goals and success criteria
   - Identify constraints (budget, timeline, technology)
   - Clarify ambiguities and assumptions

3. **Document Requirements**
   Create `docs/requirements.md`:
   ```markdown
   # Project Requirements
   
   ## Business Goals
   - [Goal 1]
   - [Goal 2]
   
   ## Functional Requirements
   - [Requirement 1]
   - [Requirement 2]
   
   ## Non-Functional Requirements
   - Performance: [Metrics]
   - Availability: [SLA]
   - Security: [Standards]
   
   ## Constraints
   - Budget: $X/month
   - Timeline: X weeks
   - Technology: [Preferences]
   ```

4. **Prioritize Requirements**
   - Mark as High, Medium, or Low priority
   - Identify must-haves vs nice-to-haves
   - Consider phased delivery approach

✅ **Success Check**: You have a clear, prioritized requirements document approved by stakeholders.

#### Common Issues

**Issue**: Vague or conflicting requirements  
**Solution**: Use the "5 Whys" technique to clarify. Document assumptions explicitly.

**Issue**: Scope creep during requirements gathering  
**Solution**: Document all requests, but park non-essential items for future phases.

### Step 1.2: Design Architecture (1-2 days)

#### Activities

1. **Create C4 Context Diagram**
   ```
   ┌─────────────────────────────────────────────┐
   │                                             │
   │           [Your System]                     │
   │     Customer Portal Application             │
   │                                             │
   └─────────────────────────────────────────────┘
            ↑                      ↑
            │                      │
         [User]               [Admin]
            
            ↓                      ↓
      [External System]    [Payment Gateway]
   ```

2. **Create C4 Container Diagram**
   ```
   ┌─────────────────────────────────────────────────┐
   │          Customer Portal System                 │
   │  ┌─────────────┐         ┌─────────────┐       │
   │  │   Web App   │────────▶│  API Layer  │       │
   │  │  (React)    │         │  (Node.js)  │       │
   │  └─────────────┘         └─────────────┘       │
   │                                 │               │
   │                                 ↓               │
   │                          ┌─────────────┐        │
   │                          │  Database   │        │
   │                          │ (PostgreSQL)│        │
   │                          └─────────────┘        │
   └─────────────────────────────────────────────────┘
   ```

3. **Select Azure Services**
   Map your containers to Azure services:
   - Web App → Azure Static Web Apps or App Service
   - API Layer → Azure App Service or Container Apps
   - Database → Azure Database for PostgreSQL
   - Storage → Azure Blob Storage
   - Cache → Azure Cache for Redis
   - Auth → Azure Active Directory

4. **Create Technology Stack Document**
   ```markdown
   # Technology Stack
   
   ## Frontend
   - Framework: React 18
   - State Management: Redux Toolkit
   - UI Library: Material-UI
   - Build Tool: Vite
   
   ## Backend
   - Runtime: Node.js 18 LTS
   - Framework: Express.js
   - ORM: Prisma
   - Testing: Jest + Supertest
   
   ## Infrastructure
   - Cloud: Azure
   - IaC: Terraform/HCL
   - CI/CD: GitHub Actions
   - Monitoring: Azure Monitor
   ```

✅ **Success Check**: You have architecture diagrams and technology choices documented.

#### Common Issues

**Issue**: Analysis paralysis choosing technologies  
**Solution**: Use proven stacks. Optimize later if needed.

**Issue**: Over-architecting for scale  
**Solution**: Design for 3x current needs, not 100x.

### Step 1.3: Generate TBD with AI (2-4 hours)

#### Activities

1. **Prepare AI Prompt**
   - Open `docs/01-stage-discover-design/PROMPT.md`
   - Copy the master prompt
   - Replace all placeholders with your project details:
     - `[PROJECT_NAME]`
     - `[PROJECT_DESCRIPTION]`
     - `[FUNCTIONAL_REQUIREMENTS]`
     - `[NON_FUNCTIONAL_REQUIREMENTS]`
     - `[CONSTRAINTS]`
     - `[TECHNOLOGY_PREFERENCES]`

2. **Use AI Assistant**
   ```
   Open your AI assistant and paste the customized prompt.
   
   Pro tip: If using ChatGPT or Claude, start with:
   "I need you to act as a Solution Architect for an Azure-based project. 
   I will provide requirements and you will create a comprehensive Technical 
   Blueprint Document following a specific template."
   ```

3. **Generate Each TBD Section**
   Work through sections systematically:
   
   **a. Requirements Registry**
   - AI generates FR-001, FR-002, etc.
   - Review for completeness and clarity
   - Ask AI: "Are there any requirements I'm missing?"

   **b. Component Catalog**
   - AI identifies COMP-APP-XXX and COMP-INF-XXX
   - Review dependencies and interfaces
   - Ask AI: "What are the risks with COMP-APP-001?"

   **c. Resource Inventory**
   - AI maps components to Azure resources
   - Review sizing and costs
   - Ask AI: "Can we optimize costs for RES-AZ-001?"

   **d. Security Design**
   - AI creates SEC-XXX items
   - Review against compliance requirements
   - Ask AI: "What security considerations am I missing?"

   **e. Task Backlog**
   - AI breaks down work into TASK-XXX items
   - Review estimates and dependencies
   - Ask AI: "What's the critical path?"

   **f. Risk Matrix**
   - AI identifies RISK-XXX items
   - Review mitigation strategies
   - Ask AI: "What's the highest risk item?"

4. **Refine and Iterate**
   - Ask AI to expand sparse sections
   - Request examples for unclear items
   - Validate technical feasibility
   - Get estimates verified

5. **Compile Final TBD**
   ```bash
   # Save the TBD
   vi docs/technical-blueprint.md
   
   # Commit to Git
   git add docs/technical-blueprint.md
   git commit -m "feat: add Technical Blueprint Document (Stage 1)"
   ```

✅ **Success Check**: Complete TBD with all sections populated and cross-referenced.

#### Common Issues

**Issue**: AI generates generic or incomplete content  
**Solution**: Provide more context. Use follow-up prompts like "expand section X with specific Azure services."

**Issue**: Inconsistent ID numbering  
**Solution**: Ask AI to "renumber all IDs sequentially" and verify manually.

### Step 1.4: Review and Approve (1-2 days)

#### Activities

1. **Quality Check**
   Use this checklist:
   - [ ] All requirements have unique IDs
   - [ ] Every component maps to requirements
   - [ ] All resources have cost estimates
   - [ ] Security covers auth, network, data
   - [ ] Tasks reference TBD items
   - [ ] Risks have mitigation plans
   - [ ] Diagrams are included
   - [ ] Technology stack is documented

2. **Stakeholder Review**
   - Schedule review meeting
   - Walk through TBD sections
   - Address questions and concerns
   - Document feedback

3. **Incorporate Feedback**
   - Update TBD based on reviews
   - Version the document (v1.1, v1.2, etc.)
   - Get final approval

4. **Baseline the TBD**
   ```bash
   git add docs/technical-blueprint.md
   git commit -m "feat: finalize TBD v1.0 - approved by stakeholders"
   git tag -a stage1-complete -m "Stage 1: Discover & Design complete"
   git push origin main --tags
   ```

✅ **Success Check**: TBD is approved and baselined in version control.

---

## Stage 2: Develop & Build

**Duration**: 2-8 weeks  
**Role**: Software Engineer  
**Outcome**: Working code, IaC, and tests

### Overview

Stage 2 implements the design from Stage 1. You'll build application components, create infrastructure as code, and ensure everything is tested and documented.

### Step 2.1: Set Up Development Environment (1 day)

#### Activities

1. **Create Project Structure**
   ```bash
   # Application code
   mkdir -p src/{api,services,models,utils,tests}
   
   # Infrastructure
   mkdir -p infrastructure/{modules,environments}
   
   # Configuration
   mkdir -p config/{dev,staging,prod}
   
   # Documentation
   mkdir -p docs/{api,architecture,runbooks}
   ```

2. **Initialize Tech Stack**
   
   **For Node.js Backend:**
   ```bash
   cd src/api
   npm init -y
   npm install express cors helmet dotenv
   npm install --save-dev jest supertest eslint
   ```

   **For Python Backend:**
   ```bash
   cd src/api
   python -m venv venv
   source venv/bin/activate
   pip install fastapi uvicorn pytest
   ```

3. **Set Up Terraform**
   ```bash
   cd infrastructure
   
   # Create main configuration
   cat > main.tf << 'EOF'
   terraform {
     required_version = ">= 1.0"
     required_providers {
       azurerm = {
         source  = "hashicorp/azurerm"
         version = "~> 3.0"
       }
     }
   }
   
   provider "azurerm" {
     features {}
   }
   EOF
   
   terraform init
   ```

4. **Configure Version Control**
   ```bash
   # Create .gitignore
   cat > .gitignore << 'EOF'
   # Dependencies
   node_modules/
   venv/
   
   # Environment
   .env
   .env.local
   *.env
   
   # Terraform
   .terraform/
   *.tfstate
   *.tfstate.backup
   
   # IDE
   .vscode/
   .idea/
   EOF
   
   git add .
   git commit -m "chore: initialize project structure"
   ```

✅ **Success Check**: Project structure is created and development tools are configured.

### Step 2.2: Implement Components (1-6 weeks)

#### Process for Each Component

For each COMP-APP-XXX in your TBD:

1. **Create Task Branch**
   ```bash
   # Example: Implementing COMP-APP-001 (Auth Service)
   git checkout -b feature/COMP-APP-001-auth-service
   ```

2. **Use Stage 2 Prompt**
   - Open `docs/02-stage-develop-build/PROMPT.md`
   - Customize with your TBD and specific TASK-XXX
   - Provide to AI assistant

3. **Generate Code**
   Example AI prompt:
   ```
   Using the TBD, implement COMP-APP-001 (Authentication Service).
   
   Requirements:
   - Implements FR-001: Azure AD authentication
   - Technology: Node.js + Express + Passport
   - Must include unit tests with >80% coverage
   - Must include API documentation
   
   Generate:
   1. API routes file
   2. Authentication middleware
   3. Unit tests
   4. Integration tests
   5. README with setup instructions
   ```

4. **Review and Refine Code**
   - Test locally
   - Run linters
   - Check test coverage
   - Review security practices

5. **Document the Component**
   ```markdown
   # COMP-APP-001: Authentication Service
   
   ## Purpose
   Handles user authentication via Azure AD OAuth 2.0.
   
   ## Implements
   - FR-001: User authentication
   - NFR-010: Multi-factor authentication
   
   ## API Endpoints
   - POST /auth/login - Initiate login
   - GET /auth/callback - OAuth callback
   - POST /auth/logout - User logout
   - GET /auth/verify - Verify token
   
   ## Configuration
   See .env.example for required variables.
   
   ## Testing
   npm test
   ```

6. **Commit and Push**
   ```bash
   git add .
   git commit -m "feat(COMP-APP-001): implement authentication service

   - Add Azure AD OAuth 2.0 integration
   - Implement session management
   - Add unit and integration tests (85% coverage)
   - Add API documentation
   
   Implements: FR-001, NFR-010
   References: TASK-001"
   
   git push origin feature/COMP-APP-001-auth-service
   ```

7. **Create Pull Request**
   - Reference TBD IDs in PR description
   - Request code review
   - Address feedback
   - Merge to main

#### Component Implementation Example

**Example: Building COMP-APP-002 (Customer Service)**

```javascript
// src/services/customerService.js
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

/**
 * Customer Service
 * Implements: COMP-APP-002 from TBD
 * References: FR-002 (CRUD operations)
 */
class CustomerService {
  
  /**
   * Create a new customer
   * @param {Object} customerData - Customer information
   * @returns {Promise<Object>} Created customer
   */
  async createCustomer(customerData) {
    return await prisma.customer.create({
      data: customerData
    });
  }

  /**
   * Get customer by ID
   * @param {string} id - Customer ID
   * @returns {Promise<Object>} Customer object
   */
  async getCustomerById(id) {
    return await prisma.customer.findUnique({
      where: { id }
    });
  }

  /**
   * Update customer
   * @param {string} id - Customer ID
   * @param {Object} updateData - Fields to update
   * @returns {Promise<Object>} Updated customer
   */
  async updateCustomer(id, updateData) {
    return await prisma.customer.update({
      where: { id },
      data: updateData
    });
  }

  /**
   * Delete customer
   * @param {string} id - Customer ID
   * @returns {Promise<Object>} Deleted customer
   */
  async deleteCustomer(id) {
    return await prisma.customer.delete({
      where: { id }
    });
  }
}

module.exports = new CustomerService();
```

```javascript
// src/services/__tests__/customerService.test.js
const customerService = require('../customerService');

/**
 * Tests for COMP-APP-002 (Customer Service)
 * Validates: FR-002 (CRUD operations)
 */
describe('CustomerService', () => {
  
  describe('createCustomer', () => {
    it('should create a new customer', async () => {
      const customerData = {
        name: 'John Doe',
        email: 'john@example.com'
      };
      
      const customer = await customerService.createCustomer(customerData);
      
      expect(customer).toHaveProperty('id');
      expect(customer.name).toBe('John Doe');
      expect(customer.email).toBe('john@example.com');
    });
  });
  
  // Additional tests...
});
```

✅ **Success Check**: Each component has code, tests, and documentation.

#### Common Issues

**Issue**: Code doesn't match TBD specifications  
**Solution**: Regularly cross-reference TBD. Update TBD if design needs to change.

**Issue**: Low test coverage  
**Solution**: Use AI to generate test cases. Aim for >80% coverage.

**Issue**: Components too tightly coupled  
**Solution**: Review dependency diagram in TBD. Refactor interfaces.

### Step 2.3: Create Infrastructure as Code (1-2 weeks)

#### Process

1. **Create Terraform Modules**
   For each RES-AZ-XXX in TBD:

   ```bash
   # Create module for RES-AZ-001 (App Service)
   mkdir -p infrastructure/modules/app-service
   cd infrastructure/modules/app-service
   ```

2. **Use AI for IaC Generation**
   ```
   Prompt: "Create a Terraform module for RES-AZ-001 (Azure App Service) from the TBD.
   
   Requirements:
   - SKU: P1v2
   - Runtime: Node.js 18
   - Must include:
     - main.tf (resource definitions)
     - variables.tf (inputs)
     - outputs.tf (outputs)
     - README.md (usage docs)
   "
   ```

3. **Example Terraform Module**
   
   **main.tf:**
   ```hcl
   # RES-AZ-001: App Service for API
   # Implements: COMP-APP-001, COMP-APP-002
   
   resource "azurerm_service_plan" "api" {
     name                = "${var.project_name}-${var.environment}-plan"
     location            = var.location
     resource_group_name = var.resource_group_name
     os_type             = "Linux"
     sku_name            = "P1v2"
   }
   
   resource "azurerm_linux_web_app" "api" {
     name                = "${var.project_name}-${var.environment}-api"
     location            = var.location
     resource_group_name = var.resource_group_name
     service_plan_id     = azurerm_service_plan.api.id
     
     site_config {
       application_stack {
         node_version = "18-lts"
       }
     }
     
     app_settings = {
       "WEBSITE_NODE_DEFAULT_VERSION" = "18-lts"
     }
   }
   ```

   **variables.tf:**
   ```hcl
   variable "project_name" {
     description = "Project name for resource naming"
     type        = string
   }
   
   variable "environment" {
     description = "Environment (dev, staging, prod)"
     type        = string
   }
   
   variable "location" {
     description = "Azure region"
     type        = string
     default     = "eastus"
   }
   
   variable "resource_group_name" {
     description = "Resource group name"
     type        = string
   }
   ```

   **outputs.tf:**
   ```hcl
   output "app_service_name" {
     description = "Name of the App Service"
     value       = azurerm_linux_web_app.api.name
   }
   
   output "app_service_hostname" {
     description = "Default hostname of the App Service"
     value       = azurerm_linux_web_app.api.default_hostname
   }
   ```

4. **Create Environment Configurations**
   
   **environments/dev/main.tf:**
   ```hcl
   terraform {
     backend "azurerm" {
       resource_group_name  = "terraform-state-rg"
       storage_account_name = "tfstate"
       container_name       = "tfstate"
       key                  = "dev.terraform.tfstate"
     }
   }
   
   module "app_service" {
     source              = "../../modules/app-service"
     project_name        = "customer-portal"
     environment         = "dev"
     location            = "eastus"
     resource_group_name = azurerm_resource_group.main.name
   }
   
   resource "azurerm_resource_group" "main" {
     name     = "customer-portal-dev-rg"
     location = "eastus"
   }
   ```

5. **Test Infrastructure**
   ```bash
   cd infrastructure/environments/dev
   
   # Initialize
   terraform init
   
   # Plan
   terraform plan -out=tfplan
   
   # Review the plan
   terraform show tfplan
   
   # Apply (in dev environment)
   terraform apply tfplan
   ```

✅ **Success Check**: All Azure resources are defined in Terraform and tested in dev environment.

#### Common Issues

**Issue**: Terraform state conflicts  
**Solution**: Use remote backend (Azure Storage) and state locking.

**Issue**: Resources too expensive in dev  
**Solution**: Use smaller SKUs for dev. Define environment-specific variables.

**Issue**: Hardcoded values  
**Solution**: Use variables and outputs. Never hardcode secrets.

### Step 2.4: Testing and Documentation (ongoing)

#### Testing Strategy

**Unit Tests** (per component):
```bash
# Node.js
npm test

# Python
pytest tests/

# Coverage report
npm run test:coverage
```

**Integration Tests**:
```javascript
// tests/integration/api.test.js
describe('API Integration Tests', () => {
  it('should authenticate and access protected endpoint', async () => {
    // 1. Login
    const loginRes = await request(app)
      .post('/auth/login')
      .send({ username: 'test@example.com', password: 'test123' });
    
    const token = loginRes.body.token;
    
    // 2. Access protected resource
    const dataRes = await request(app)
      .get('/api/customers')
      .set('Authorization', `Bearer ${token}`);
    
    expect(dataRes.status).toBe(200);
  });
});
```

**Documentation Checklist**:
- [ ] API documentation (OpenAPI/Swagger)
- [ ] Component README files
- [ ] Setup instructions
- [ ] Environment variables documentation
- [ ] Architecture diagrams updated
- [ ] Code comments for complex logic

✅ **Success Check**: All tests pass, coverage >80%, documentation is complete.

---

## Stage 3: Deploy & Deliver

**Duration**: 1-2 weeks  
**Role**: Platform Engineer  
**Outcome**: CI/CD pipelines and deployed environments

### Overview

Stage 3 automates deployment and establishes environments. You'll create CI/CD pipelines, provision infrastructure, and ensure reliable delivery.

### Step 3.1: Set Up CI Pipeline (1-2 days)

#### Activities

1. **Create GitHub Actions Workflow**
   
   **.github/workflows/ci.yml:**
   ```yaml
   # CI Pipeline for Customer Portal
   # Implements: Stage 3 - Deploy & Deliver
   # References: TBD Section 3 (Deploy & Deliver)
   
   name: CI Pipeline
   
   on:
     push:
       branches: [main, develop]
     pull_request:
       branches: [main]
   
   jobs:
     test:
       runs-on: ubuntu-latest
       
       steps:
         - uses: actions/checkout@v3
         
         - name: Setup Node.js
           uses: actions/setup-node@v3
           with:
             node-version: '18'
             cache: 'npm'
         
         - name: Install dependencies
           run: npm ci
         
         - name: Run linter
           run: npm run lint
         
         - name: Run tests
           run: npm test
         
         - name: Check coverage
           run: npm run test:coverage
         
         - name: Upload coverage
           uses: codecov/codecov-action@v3
     
     security:
       runs-on: ubuntu-latest
       
       steps:
         - uses: actions/checkout@v3
         
         - name: Run security audit
           run: npm audit
         
         - name: Run Snyk scan
           uses: snyk/actions/node@master
           env:
             SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
     
     build:
       needs: [test, security]
       runs-on: ubuntu-latest
       
       steps:
         - uses: actions/checkout@v3
         
         - name: Build application
           run: npm run build
         
         - name: Upload artifact
           uses: actions/upload-artifact@v3
           with:
             name: app-build
             path: dist/
   ```

2. **Test CI Pipeline**
   ```bash
   git add .github/workflows/ci.yml
   git commit -m "ci: add CI pipeline"
   git push origin main
   
   # Monitor at: https://github.com/YOUR_ORG/YOUR_REPO/actions
   ```

✅ **Success Check**: CI pipeline runs successfully on every commit.

### Step 3.2: Set Up CD Pipeline (2-3 days)

#### Activities

1. **Create Deployment Workflow**
   
   **.github/workflows/cd.yml:**
   ```yaml
   # CD Pipeline - Deploy to Azure
   # Implements: Stage 3 - Deploy & Deliver
   
   name: CD Pipeline
   
   on:
     push:
       branches: [main]
     workflow_dispatch:
       inputs:
         environment:
           description: 'Environment to deploy'
           required: true
           type: choice
           options:
             - dev
             - staging
             - prod
   
   jobs:
     deploy:
       runs-on: ubuntu-latest
       environment: ${{ github.event.inputs.environment || 'dev' }}
       
       steps:
         - uses: actions/checkout@v3
         
         - name: Azure Login
           uses: azure/login@v1
           with:
             creds: ${{ secrets.AZURE_CREDENTIALS }}
         
         - name: Setup Terraform
           uses: hashicorp/setup-terraform@v2
         
         - name: Terraform Init
           run: |
             cd infrastructure/environments/${{ github.event.inputs.environment || 'dev' }}
             terraform init
         
         - name: Terraform Plan
           run: |
             cd infrastructure/environments/${{ github.event.inputs.environment || 'dev' }}
             terraform plan -out=tfplan
         
         - name: Terraform Apply
           run: |
             cd infrastructure/environments/${{ github.event.inputs.environment || 'dev' }}
             terraform apply -auto-approve tfplan
         
         - name: Deploy Application
           uses: azure/webapps-deploy@v2
           with:
             app-name: ${{ secrets.AZURE_APP_NAME }}
             package: dist/
   ```

2. **Configure Secrets**
   ```bash
   # In GitHub: Settings → Secrets and variables → Actions
   
   # Add these secrets:
   # - AZURE_CREDENTIALS (service principal JSON)
   # - AZURE_APP_NAME (app service name)
   # - SNYK_TOKEN (for security scanning)
   ```

3. **Create Service Principal for Azure**
   ```bash
   # Create service principal
   az ad sp create-for-rbac \
     --name "github-actions-customer-portal" \
     --role contributor \
     --scopes /subscriptions/YOUR_SUBSCRIPTION_ID \
     --sdk-auth
   
   # Copy output JSON to AZURE_CREDENTIALS secret
   ```

✅ **Success Check**: CD pipeline can deploy to dev environment automatically.

### Step 3.3: Environment Management (1-2 days)

#### Activities

1. **Provision Environments**
   ```bash
   # Development
   cd infrastructure/environments/dev
   terraform init
   terraform apply
   
   # Staging
   cd ../staging
   terraform init
   terraform apply
   
   # Production (requires approval)
   cd ../prod
   terraform init
   terraform plan  # Review carefully
   # terraform apply  # Apply after review
   ```

2. **Configure Environment Protection**
   In GitHub: Settings → Environments
   
   **Dev Environment**:
   - No protection rules
   - Auto-deploy on push to main
   
   **Staging Environment**:
   - Require 1 reviewer
   - Deploy after dev succeeds
   
   **Production Environment**:
   - Require 2 reviewers
   - Manual approval required
   - Restricted to specific branches

3. **Set Environment Variables**
   For each environment, configure:
   ```bash
   # Dev
   az webapp config appsettings set \
     --name customer-portal-dev-api \
     --resource-group customer-portal-dev-rg \
     --settings \
       NODE_ENV=development \
       DATABASE_URL="$DEV_DB_URL" \
       AZURE_AD_TENANT_ID="$TENANT_ID"
   
   # Repeat for staging and prod
   ```

✅ **Success Check**: All three environments (dev, staging, prod) are provisioned and configured.

### Step 3.4: Validation and Monitoring Setup (1 day)

#### Activities

1. **Deploy Health Check Endpoint**
   ```javascript
   // src/api/routes/health.js
   router.get('/health', async (req, res) => {
     const health = {
       status: 'healthy',
       timestamp: new Date().toISOString(),
       checks: {
         database: await checkDatabase(),
         cache: await checkCache(),
         storage: await checkStorage()
       }
     };
     
     const allHealthy = Object.values(health.checks).every(c => c.status === 'ok');
     const statusCode = allHealthy ? 200 : 503;
     
     res.status(statusCode).json(health);
   });
   ```

2. **Configure Azure Monitor**
   ```bash
   # Enable Application Insights
   az monitor app-insights component create \
     --app customer-portal-insights \
     --location eastus \
     --resource-group customer-portal-prod-rg \
     --application-type web
   
   # Get instrumentation key
   INSTRUMENTATION_KEY=$(az monitor app-insights component show \
     --app customer-portal-insights \
     --resource-group customer-portal-prod-rg \
     --query instrumentationKey -o tsv)
   
   # Add to app settings
   az webapp config appsettings set \
     --name customer-portal-prod-api \
     --resource-group customer-portal-prod-rg \
     --settings APPLICATIONINSIGHTS_CONNECTION_STRING="InstrumentationKey=$INSTRUMENTATION_KEY"
   ```

✅ **Success Check**: Deployment pipelines work end-to-end and monitoring is configured.

---

## Stage 4: Validate & Operate

**Duration**: 1-2 weeks  
**Role**: Quality & Reliability Engineer  
**Outcome**: Validated system with monitoring and runbooks

### Overview

Stage 4 ensures the system works correctly and can be operated reliably. You'll validate requirements, set up monitoring, and create operational documentation.

### Step 4.1: Validation Testing (3-5 days)

#### Activities

1. **Create E2E Test Suite**
   
   **tests/e2e/user-journey.test.js:**
   ```javascript
   // End-to-End Test: Complete User Journey
   // Validates: FR-001 through FR-005
   
   describe('User Journey: Customer Management', () => {
     
     it('should complete full customer lifecycle', async () => {
       // 1. Login (FR-001)
       const loginRes = await request(app)
         .post('/auth/login')
         .send({ email: 'test@example.com', password: 'Test123!' });
       expect(loginRes.status).toBe(200);
       const token = loginRes.body.token;
       
       // 2. Create customer (FR-002)
       const createRes = await request(app)
         .post('/api/customers')
         .set('Authorization', `Bearer ${token}`)
         .send({ name: 'John Doe', email: 'john@example.com' });
       expect(createRes.status).toBe(201);
       const customerId = createRes.body.id;
       
       // 3. Read customer (FR-002)
       const readRes = await request(app)
         .get(`/api/customers/${customerId}`)
         .set('Authorization', `Bearer ${token}`);
       expect(readRes.status).toBe(200);
       expect(readRes.body.name).toBe('John Doe');
       
       // 4. Update customer (FR-002)
       const updateRes = await request(app)
         .put(`/api/customers/${customerId}`)
         .set('Authorization', `Bearer ${token}`)
         .send({ name: 'John Smith' });
       expect(updateRes.status).toBe(200);
       
       // 5. Export customers (FR-003)
       const exportRes = await request(app)
         .get('/api/customers/export')
         .set('Authorization', `Bearer ${token}`);
       expect(exportRes.status).toBe(200);
       expect(exportRes.header['content-type']).toContain('text/csv');
       
       // 6. Delete customer (FR-002)
       const deleteRes = await request(app)
         .delete(`/api/customers/${customerId}`)
         .set('Authorization', `Bearer ${token}`);
       expect(deleteRes.status).toBe(204);
     });
   });
   ```

2. **Performance Testing**
   
   **tests/performance/load-test.js:**
   ```javascript
   const autocannon = require('autocannon');
   
   // Validates: NFR-001 (API response time < 200ms)
   // Validates: NFR-007 (Support 10,000 concurrent users)
   
   const run = async () => {
     const result = await autocannon({
       url: 'https://customer-portal-prod.azurewebsites.net',
       connections: 100,
       duration: 60,
       requests: [
         {
           method: 'GET',
           path: '/api/customers',
           headers: {
             'Authorization': `Bearer ${process.env.TEST_TOKEN}`
           }
         }
       ]
     });
     
     console.log('Performance Test Results:');
     console.log(`Requests/sec: ${result.requests.average}`);
     console.log(`Latency p95: ${result.latency.p95}ms`);
     console.log(`Latency p99: ${result.latency.p99}ms`);
     
     // Assert NFR-001
     if (result.latency.p95 > 200) {
       throw new Error(`NFR-001 FAILED: p95 latency is ${result.latency.p95}ms (target: <200ms)`);
     }
   };
   
   run();
   ```

3. **Security Validation**
   ```bash
   # OWASP ZAP scan
   docker run -t owasp/zap2docker-stable zap-baseline.py \
     -t https://customer-portal-prod.azurewebsites.net \
     -r security-report.html
   
   # Check for exposed secrets
   docker run -v $(pwd):/repo trufflesecurity/trufflehog:latest \
     filesystem /repo
   ```

4. **Create Validation Report**
   
   **docs/validation-report.md:**
   ```markdown
   # Validation Report - Customer Portal
   
   **Date**: 2026-01-31  
   **Environment**: Production  
   **Tested By**: QA Team
   
   ## Functional Requirements Validation
   
   | ID | Requirement | Status | Evidence |
   |----|-------------|--------|----------|
   | FR-001 | Azure AD authentication | ✅ PASS | E2E test suite |
   | FR-002 | CRUD operations | ✅ PASS | E2E test suite |
   | FR-003 | Export to CSV | ✅ PASS | E2E test suite |
   
   ## Non-Functional Requirements Validation
   
   | ID | Requirement | Target | Actual | Status |
   |----|-------------|--------|--------|--------|
   | NFR-001 | API response time | <200ms (p95) | 145ms | ✅ PASS |
   | NFR-004 | System availability | 99.9% | 99.95% | ✅ PASS |
   | NFR-007 | Concurrent users | 10,000 | 12,000 | ✅ PASS |
   
   ## Security Validation
   
   | Check | Status | Notes |
   |-------|--------|-------|
   | OWASP ZAP scan | ✅ PASS | No high-severity issues |
   | Secret scanning | ✅ PASS | No exposed credentials |
   | SSL/TLS config | ✅ PASS | TLS 1.2+ enforced |
   
   ## Issues Found
   
   None - all validation tests passed.
   
   ## Sign-Off
   
   System is validated and ready for production use.
   ```

✅ **Success Check**: All requirements validated and report generated.

### Step 4.2: Monitoring Setup (2-3 days)

#### Activities

1. **Configure Application Insights**
   ```javascript
   // src/middleware/telemetry.js
   const appInsights = require('applicationinsights');
   
   appInsights.setup(process.env.APPLICATIONINSIGHTS_CONNECTION_STRING)
     .setAutoDependencyCorrelation(true)
     .setAutoCollectRequests(true)
     .setAutoCollectPerformance(true)
     .setAutoCollectExceptions(true)
     .setAutoCollectDependencies(true)
     .setAutoCollectConsole(true)
     .setUseDiskRetryCaching(true)
     .start();
   
   const client = appInsights.defaultClient;
   
   // Track custom metrics
   router.use((req, res, next) => {
     client.trackMetric({
       name: 'API Request',
       value: 1,
       properties: {
         method: req.method,
         path: req.path,
         statusCode: res.statusCode
       }
     });
     next();
   });
   ```

2. **Create Alerts**
   ```bash
   # High response time alert
   az monitor metrics alert create \
     --name high-response-time \
     --resource-group customer-portal-prod-rg \
     --scopes /subscriptions/.../resourceGroups/customer-portal-prod-rg/providers/Microsoft.Web/sites/customer-portal-prod-api \
     --condition "avg responseTime > 500" \
     --description "Alert when average response time exceeds 500ms" \
     --evaluation-frequency 1m \
     --window-size 5m \
     --severity 2
   
   # High error rate alert
   az monitor metrics alert create \
     --name high-error-rate \
     --resource-group customer-portal-prod-rg \
     --scopes /subscriptions/.../resourceGroups/customer-portal-prod-rg/providers/Microsoft.Web/sites/customer-portal-prod-api \
     --condition "avg Http5xx > 10" \
     --description "Alert when 5xx errors exceed 10 per minute" \
     --evaluation-frequency 1m \
     --window-size 5m \
     --severity 1
   ```

3. **Create Dashboard**
   
   Create `monitoring-dashboard.json` and import to Azure:
   ```json
   {
     "properties": {
       "lenses": {
         "0": {
           "order": 0,
           "parts": {
             "0": {
               "position": { "x": 0, "y": 0, "colSpan": 6, "rowSpan": 4 },
               "metadata": {
                 "type": "Extension/Microsoft_OperationsManagementSuite_Workspace/PartType/LogsDashboardPart",
                 "inputs": [
                   {
                     "name": "query",
                     "value": "requests | summarize count() by bin(timestamp, 5m) | render timechart"
                   }
                 ]
               }
             }
           }
         }
       }
     }
   }
   ```

✅ **Success Check**: Monitoring dashboards show real-time metrics and alerts are configured.

### Step 4.3: Create Runbooks (1-2 days)

#### Activities

1. **Create Operations Runbook**
   
   **docs/runbooks/operations.md:**
   ```markdown
   # Operations Runbook - Customer Portal
   
   ## System Overview
   
   **Purpose**: Customer management portal  
   **Owner**: Platform Team  
   **On-Call**: +1-555-0100 (PagerDuty)
   
   ## Architecture
   
   - Frontend: Azure Static Web Apps
   - Backend: Azure App Service (Node.js)
   - Database: Azure PostgreSQL
   - Cache: Azure Cache for Redis
   
   ## Common Operations
   
   ### Deployment
   
   ```bash
   # Deploy to staging
   gh workflow run cd.yml -f environment=staging
   
   # Deploy to production (requires approval)
   gh workflow run cd.yml -f environment=prod
   ```
   
   ### Scaling
   
   ```bash
   # Scale up App Service
   az appservice plan update \
     --name customer-portal-prod-plan \
     --resource-group customer-portal-prod-rg \
     --sku P2v2
   
   # Scale out (add instances)
   az appservice plan update \
     --name customer-portal-prod-plan \
     --resource-group customer-portal-prod-rg \
     --number-of-workers 3
   ```
   
   ### Database Maintenance
   
   ```bash
   # Manual backup
   az postgres flexible-server backup create \
     --resource-group customer-portal-prod-rg \
     --name customer-portal-prod-db \
     --backup-name manual-backup-$(date +%Y%m%d)
   
   # Check database performance
   az postgres flexible-server show \
     --resource-group customer-portal-prod-rg \
     --name customer-portal-prod-db \
     --query "{CPU:cpu,Memory:memory,Storage:storage}"
   ```
   
   ## Incident Response
   
   ### High Response Time
   
   **Symptoms**: API latency > 500ms  
   **Impact**: Slow user experience  
   **Priority**: P2
   
   **Diagnosis**:
   1. Check Application Insights for slow requests
   2. Review database query performance
   3. Check Redis cache hit rate
   4. Review App Service metrics (CPU, memory)
   
   **Resolution**:
   1. Identify slow queries and optimize
   2. Increase cache TTL if appropriate
   3. Scale App Service if resource-constrained
   4. Clear cache if stale data suspected
   
   ### Database Connection Failures
   
   **Symptoms**: 500 errors, "connection refused"  
   **Impact**: Service unavailable  
   **Priority**: P1
   
   **Diagnosis**:
   1. Check PostgreSQL server status
   2. Verify network security group rules
   3. Check connection pool settings
   4. Review firewall rules
   
   **Resolution**:
   1. Restart App Service to reset connection pool
   2. Verify database is running: `az postgres flexible-server show`
   3. Check and update firewall rules if needed
   4. Increase connection pool size in app configuration
   
   ### High Error Rate
   
   **Symptoms**: 5xx errors > 10/min  
   **Impact**: Users experiencing errors  
   **Priority**: P1
   
   **Diagnosis**:
   1. Check Application Insights exceptions
   2. Review application logs
   3. Check dependencies (database, Redis, external APIs)
   4. Review recent deployments
   
   **Resolution**:
   1. If deployment-related: rollback to previous version
   2. If dependency issue: check dependency health
   3. If application bug: create hotfix
   4. If resource constraint: scale resources
   
   ## Health Checks
   
   ### Automated Monitoring
   
   - Synthetic transactions: Every 5 minutes
   - Health endpoint: `/health` - checks DB, cache, storage
   - Uptime monitor: Pingdom checking every minute
   
   ### Manual Health Check
   
   ```bash
   # Check all resources
   az resource list \
     --resource-group customer-portal-prod-rg \
     --query "[].{Name:name, Type:type, Status:properties.provisioningState}"
   
   # Check App Service
   curl -s https://customer-portal-prod.azurewebsites.net/health | jq
   
   # Check database
   az postgres flexible-server show \
     --resource-group customer-portal-prod-rg \
     --name customer-portal-prod-db \
     --query "{State:state, Status:fullyQualifiedDomainName}"
   ```
   
   ## Contacts
   
   | Role | Name | Contact |
   |------|------|---------|
   | On-Call Engineer | Rotation | PagerDuty |
   | Platform Lead | Jane Smith | jane@company.com |
   | Database Admin | Bob Johnson | bob@company.com |
   | Security Team | security@company.com | Slack: #security |
   ```

2. **Create Incident Response Template**
   
   **docs/runbooks/incident-template.md:**
   ```markdown
   # Incident Report: [Incident ID]
   
   **Date**: YYYY-MM-DD  
   **Start Time**: HH:MM UTC  
   **End Time**: HH:MM UTC  
   **Duration**: XX minutes  
   **Severity**: P1 / P2 / P3  
   **Status**: Investigating / Mitigated / Resolved
   
   ## Impact
   
   **Affected Services**:
   - [Service name]
   
   **User Impact**:
   - [Description of user impact]
   
   **Business Impact**:
   - [Revenue impact, customer count, etc.]
   
   ## Timeline
   
   | Time (UTC) | Event |
   |------------|-------|
   | HH:MM | Issue detected via monitoring alert |
   | HH:MM | On-call engineer paged |
   | HH:MM | Investigation started |
   | HH:MM | Root cause identified |
   | HH:MM | Mitigation applied |
   | HH:MM | Service restored |
   | HH:MM | Incident closed |
   
   ## Root Cause
   
   [Detailed description of what caused the incident]
   
   ## Resolution
   
   [Description of how the incident was resolved]
   
   ## Action Items
   
   - [ ] Action item 1 (Owner: Name, Due: Date)
   - [ ] Action item 2 (Owner: Name, Due: Date)
   
   ## Lessons Learned
   
   **What went well**:
   - [Point 1]
   
   **What could be improved**:
   - [Point 1]
   
   **Follow-up**:
   - [Item 1]
   ```

✅ **Success Check**: Complete runbooks are available for operations team.

---

## Integration & Iteration

### Cross-Stage Traceability

Maintain traceability throughout:

```
FR-001 (Requirement)
  ├─→ COMP-APP-001 (Component Design - Stage 1)
  │    ├─→ src/services/auth.js (Implementation - Stage 2)
  │    ├─→ tests/auth.test.js (Tests - Stage 2)
  │    └─→ infrastructure/modules/app-service/ (IaC - Stage 2)
  ├─→ .github/workflows/cd.yml (Deployment - Stage 3)
  └─→ tests/e2e/auth.test.js (Validation - Stage 4)
```

### Iteration Process

**When to Update TBD**:
- New requirements emerge
- Technical constraints discovered during implementation
- Architecture decisions change
- Stakeholder feedback

**How to Update**:
1. Update TBD document
2. Version the change (v1.1, v1.2, etc.)
3. Communicate changes to team
4. Update affected artifacts
5. Re-validate if significant

### Feedback Loops

**Stage 2 → Stage 1**:
- Implementation challenges requiring design changes
- Performance issues requiring architecture adjustments

**Stage 3 → Stage 2**:
- Deployment issues requiring code changes
- Environment-specific configurations

**Stage 4 → All Stages**:
- Operational issues requiring fixes
- Performance problems requiring optimization
- Security vulnerabilities requiring updates

---

## Troubleshooting Guide

### Stage 1 Issues

#### Issue: Incomplete Requirements
**Symptoms**: Vague or missing requirements  
**Impact**: Incomplete or incorrect design  
**Solutions**:
- Schedule requirements workshop with stakeholders
- Use "5 Whys" technique to dig deeper
- Create user stories with acceptance criteria
- Document assumptions explicitly

#### Issue: Technology Selection Paralysis
**Symptoms**: Can't decide on technologies  
**Impact**: Delays in Stage 1 completion  
**Solutions**:
- Use decision matrix (score options on criteria)
- Consult with team on their expertise
- Choose proven technologies over cutting-edge
- Document decision with ADR

#### Issue: Over-Engineering
**Symptoms**: Complex design for simple requirements  
**Impact**: Unnecessary complexity and cost  
**Solutions**:
- Apply YAGNI principle (You Aren't Gonna Need It)
- Design for 3x current scale, not 100x
- Start simple, add complexity when needed
- Review design with senior architects

### Stage 2 Issues

#### Issue: Code Not Matching TBD
**Symptoms**: Implementation diverges from design  
**Impact**: Loss of traceability  
**Solutions**:
- Regular design reviews
- Update TBD when intentional changes needed
- Use TBD IDs in commit messages
- Pair programming for alignment

#### Issue: Low Test Coverage
**Symptoms**: Tests < 80% coverage  
**Impact**: Quality risks  
**Solutions**:
- Use AI to generate test cases
- Write tests first (TDD approach)
- Review coverage reports regularly
- Make coverage a CI gate

#### Issue: Terraform State Conflicts
**Symptoms**: "State is locked" errors  
**Impact**: Can't update infrastructure  
**Solutions**:
- Use remote backend with locking
- Don't run parallel terraform operations
- If truly stuck: `terraform force-unlock LOCK_ID`

### Stage 3 Issues

#### Issue: CI/CD Pipeline Failures
**Symptoms**: Pipeline jobs failing  
**Impact**: Can't deploy  
**Solutions**:
- Check workflow logs in GitHub Actions
- Verify secrets and variables are set
- Test locally before pushing
- Use `act` tool to test workflows locally

#### Issue: Deployment Timeouts
**Symptoms**: Deployments take too long or timeout  
**Impact**: Slow delivery  
**Solutions**:
- Increase timeout values
- Optimize build process (caching, parallel jobs)
- Use smaller deployment artifacts
- Check Azure service health

#### Issue: Environment Configuration Drift
**Symptoms**: Environments behave differently  
**Impact**: "Works on dev, fails in prod"  
**Solutions**:
- Use infrastructure as code for everything
- Avoid manual changes in production
- Document all configuration in version control
- Regular drift detection with Terraform

### Stage 4 Issues

#### Issue: Test Failures in Production
**Symptoms**: E2E tests pass in staging, fail in prod  
**Impact**: Quality concerns  
**Solutions**:
- Check environment-specific configuration
- Verify network access and firewall rules
- Check data differences between environments
- Use production-like data in staging

#### Issue: False Positive Alerts
**Symptoms**: Alerts firing when no real issue  
**Impact**: Alert fatigue  
**Solutions**:
- Tune alert thresholds
- Use appropriate evaluation windows
- Add more context to alert conditions
- Review and refine alerts regularly

#### Issue: Unclear Runbook Procedures
**Symptoms**: Team can't follow runbooks  
**Impact**: Slow incident response  
**Solutions**:
- Test runbooks in practice drills
- Get feedback from operations team
- Add screenshots and examples
- Keep runbooks up to date

---

## Best Practices Summary

### Stage 1 Best Practices

✅ **Do:**
- Start with clear requirements
- Use standard models (C4, Arc42)
- Document architecture decisions (ADRs)
- Consider security from the start
- Get stakeholder approval before proceeding

❌ **Don't:**
- Over-design for unlikely scenarios
- Ignore non-functional requirements
- Skip risk assessment
- Make assumptions without documenting
- Design in isolation

### Stage 2 Best Practices

✅ **Do:**
- Reference TBD IDs in all artifacts
- Write tests before or with code (TDD)
- Use AI for boilerplate generation
- Document complex logic
- Review code with peers

❌ **Don't:**
- Hardcode configuration values
- Skip writing tests
- Ignore security best practices
- Commit secrets to version control
- Deviate from TBD without updating it

### Stage 3 Best Practices

✅ **Do:**
- Automate everything
- Use infrastructure as code
- Implement progressive deployment
- Set up proper environment gates
- Test pipelines thoroughly

❌ **Don't:**
- Make manual changes in production
- Skip security scanning
- Deploy directly to production
- Ignore failed pipeline steps
- Use same credentials across environments

### Stage 4 Best Practices

✅ **Do:**
- Validate all requirements
- Set up comprehensive monitoring
- Create actionable alerts
- Document operational procedures
- Conduct regular health checks

❌ **Don't:**
- Skip validation testing
- Set up alerts without actions
- Neglect runbook maintenance
- Ignore security validation
- Forget to monitor costs

### General Best Practices

✅ **Do:**
- Maintain traceability with TBD IDs
- Iterate and improve continuously
- Document decisions and changes
- Collaborate with team
- Use AI assistants effectively
- Version control everything
- Review and refine regularly

❌ **Don't:**
- Skip stages or rush through them
- Ignore feedback from later stages
- Let documentation become stale
- Work in silos
- Over-rely on AI without review
- Make undocumented changes
- Assume quality without validation

---

## Conclusion

### What You've Learned

By following this guide, you now understand:
- ✅ How to move through all four stages systematically
- ✅ How to use AI assistants at each stage
- ✅ How to maintain traceability throughout
- ✅ How to troubleshoot common issues
- ✅ Best practices for each stage

### Your Next Steps

1. **Apply to Real Project**: Use this guide for your next project
2. **Customize**: Adapt the framework to your needs using [Customization Guide](CUSTOMIZATION.md)
3. **Share**: Train your team on the framework
4. **Improve**: Provide feedback to enhance the framework

### Additional Resources

- **[Quick Start Guide](QUICK-START.md)** - Refresher or quick reference
- **[Customization Guide](CUSTOMIZATION.md)** - Adapt to different scenarios
- **[Sample Project](../../examples/sample-project/README.md)** - Complete example
- **[Templates](../templates/README.md)** - All document templates
- **[Glossary](../00-overview/GLOSSARY.md)** - Terms and definitions

### Getting Help

- **Questions**: Open a discussion on GitHub
- **Issues**: Report bugs or problems
- **Contributions**: Submit improvements via PR
- **Community**: Join discussions with other users

---

**Congratulations!** You now have comprehensive knowledge of the Devviaai Framework. Happy building! 🚀

---

[Back to Guides Index](README.md) | [Documentation Index](../README.md) | [Customization Guide](CUSTOMIZATION.md)
