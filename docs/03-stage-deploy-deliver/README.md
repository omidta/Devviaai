# Stage 3: Deploy & Deliver

## Overview

Stage 3 transforms tested code and infrastructure into running systems across environments. Acting as a **DevOps Engineer**, you'll create automated CI/CD pipelines, provision infrastructure, and deploy applications with confidence and repeatability.

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
Automate the deployment of applications and infrastructure from code repository to production environments with safety, speed, and reliability.

### Key Objectives

1. **CI Pipeline Creation**: Automate build, test, and artifact creation
2. **CD Pipeline Creation**: Automate deployment across environments
3. **Infrastructure Provisioning**: Deploy infrastructure using IaC
4. **Environment Management**: Manage dev, staging, and production environments
5. **Deployment Safety**: Implement gates, approvals, and rollback mechanisms
6. **Release Orchestration**: Coordinate deployment of multiple components
7. **Secrets Management**: Secure handling of credentials and sensitive data

## Role Definition

### DevOps Engineer Responsibilities

As a DevOps Engineer in Stage 3, you are responsible for:

- **Pipeline Development**: Creating and maintaining CI/CD pipelines
- **Infrastructure Automation**: Provisioning and managing cloud resources
- **Release Management**: Planning and executing deployments
- **Environment Configuration**: Managing environment-specific settings
- **Security**: Implementing secure deployment practices
- **Monitoring**: Setting up deployment monitoring and alerting
- **Troubleshooting**: Resolving deployment issues quickly
- **Documentation**: Maintaining deployment runbooks

### Skills Required

- CI/CD tools (GitHub Actions, Azure DevOps, Jenkins)
- Infrastructure as Code (Terraform, Bicep)
- Cloud platforms (Azure preferred)
- Container orchestration (Docker, Kubernetes)
- Scripting (Bash, PowerShell, Python)
- Version control (Git)
- Security best practices
- Troubleshooting and debugging

## Input Requirements

### Required Inputs

1. **From Stage 2 (Develop & Build)**
   - Deployable artifacts (container images, packages)
   - Infrastructure as Code modules
   - Test suites (unit, integration, e2e)
   - Configuration templates
   - Deployment instructions

2. **From Stage 1 (TBD)**
   - Resource specifications (RES-AZ-XXX)
   - Environment requirements
   - Security requirements (SEC-XXX)
   - Non-functional requirements (NFR-XXX)

3. **Infrastructure Requirements**
   - Azure subscription access
   - Service principal credentials
   - Network configurations
   - DNS records
   - SSL certificates

4. **Access & Permissions**
   - Repository access
   - Cloud platform access
   - Container registry access
   - Secret management access

### Optional Inputs

- Deployment schedule preferences
- Change management tickets
- Stakeholder approval requirements
- Compliance documentation

## Output Specifications

### Primary Outputs

#### 1. CI Pipeline (Continuous Integration)

**Purpose**: Automated build, test, and package creation

**Components**:
- Source code checkout
- Dependency installation
- Code linting and formatting
- Unit test execution
- Integration test execution
- Security scanning
- Build artifact creation
- Container image building
- Artifact publishing

**Example**: GitHub Actions workflow for Node.js application

#### 2. CD Pipeline (Continuous Deployment)

**Purpose**: Automated deployment to environments

**Components**:
- Infrastructure provisioning
- Configuration management
- Application deployment
- Database migrations
- Smoke tests
- Health checks
- Deployment verification
- Rollback capability

**Environments**:
- Development: Automatic deployment on merge
- Staging: Automatic deployment after dev success
- Production: Manual approval required

#### 3. Deployed Environments

**For each environment (Dev/Staging/Prod)**:
- Provisioned infrastructure (all RES-AZ-XXX)
- Deployed applications (all COMP-APP-XXX)
- Configured networking and security
- Applied secrets and configuration
- Verified health and functionality

#### 4. Release Documentation

**Release Notes**:
- Version number
- Deployment date/time
- Changes included
- Known issues
- Rollback procedure

**Deployment Records**:
- Deployment logs
- Test results
- Approval records
- Incident reports (if any)

### Supporting Outputs

- **Pipeline Metrics**: Build times, success rates, failure reasons
- **Deployment Dashboards**: Real-time deployment status
- **Runbooks**: Step-by-step deployment procedures
- **Incident Reports**: Post-mortems for failed deployments

## Process Flow

### Step-by-Step Process

```
1. CI Pipeline Setup
   │
   ├─→ Create workflow file (.github/workflows)
   ├─→ Define build jobs
   ├─→ Configure test execution
   ├─→ Set up artifact creation
   └─→ Configure container image building
   │
   ▼
2. CD Pipeline Setup
   │
   ├─→ Define deployment stages (dev/staging/prod)
   ├─→ Configure infrastructure provisioning
   ├─→ Set up application deployment
   ├─→ Define approval gates
   └─→ Configure rollback procedures
   │
   ▼
3. Development Environment Deployment
   │
   ├─→ Provision infrastructure (Terraform apply)
   ├─→ Deploy application
   ├─→ Run smoke tests
   └─→ Verify deployment
   │
   ▼
4. Staging Environment Deployment
   │
   ├─→ Trigger after dev success
   ├─→ Provision infrastructure
   ├─→ Deploy application
   ├─→ Run full test suite
   └─→ Verify deployment
   │
   ▼
5. Production Deployment
   │
   ├─→ Request approval
   ├─→ Wait for manual approval
   ├─→ Provision/update infrastructure
   ├─→ Deploy application (blue-green or rolling)
   ├─→ Run smoke tests
   ├─→ Monitor for errors
   └─→ Complete or rollback
   │
   ▼
6. Post-Deployment
   │
   ├─→ Verify all services healthy
   ├─→ Check monitoring dashboards
   ├─→ Update documentation
   └─→ Notify stakeholders
```

## Integration with Other Stages

### Receives from Stage 2 (Develop & Build)

- **Container Images**: Tagged and versioned images
- **IaC Modules**: Terraform/Bicep code
- **Test Suites**: Automated tests for validation
- **Configuration**: Environment templates
- **Documentation**: Deployment instructions

### Provides to Stage 4 (Validate & Operate)

- **Running Environments**: Deployed applications
- **Deployment Logs**: Historical deployment data
- **Pipeline Metrics**: CI/CD performance data
- **Configuration Documentation**: As-deployed settings
- **Rollback Procedures**: How to revert deployments

### Provides Feedback to Stage 2

- **Build Failures**: Issues in artifact creation
- **Test Failures**: Problems discovered during deployment
- **Configuration Issues**: Environment-specific problems
- **Performance Issues**: Deployment-time performance problems

### Provides Feedback to Stage 1

- **Infrastructure Issues**: Problems with resource specifications
- **Scaling Issues**: Inadequate resource sizing
- **Missing Requirements**: Discovered gaps in TBD

## Quality Checklist

### Before Completing Stage 3

#### CI Pipeline
- [ ] Pipeline builds on every commit
- [ ] All tests run automatically
- [ ] Code quality checks pass
- [ ] Security scans complete
- [ ] Artifacts are created and versioned
- [ ] Container images are tagged correctly
- [ ] Build failures notify team
- [ ] Build times are reasonable (<15 minutes)

#### CD Pipeline
- [ ] Deployment to dev is automatic
- [ ] Deployment to staging requires dev success
- [ ] Deployment to production requires approval
- [ ] Infrastructure provisioning is automated
- [ ] Application deployment is automated
- [ ] Database migrations are automated
- [ ] Rollback procedure is defined and tested
- [ ] Smoke tests run after deployment

#### Infrastructure
- [ ] All environments are provisioned
- [ ] Networking is configured correctly
- [ ] Security groups/NSGs are in place
- [ ] Secrets are stored securely
- [ ] Monitoring is configured
- [ ] Backups are enabled (production)
- [ ] Auto-scaling is configured
- [ ] DNS records are created

#### Deployment Safety
- [ ] Approval gates are configured
- [ ] Rollback tested successfully
- [ ] Blue-green or canary deployment used
- [ ] Database migrations are reversible
- [ ] Configuration changes are version controlled
- [ ] Secrets rotation is possible
- [ ] Emergency rollback procedure documented

#### Documentation
- [ ] Pipeline documentation is complete
- [ ] Runbooks are created
- [ ] Troubleshooting guides exist
- [ ] Deployment checklist is available
- [ ] Rollback procedure documented
- [ ] Emergency contacts listed

### Quality Attributes

- **Reliability**: Deployments succeed consistently (>95%)
- **Speed**: Deployment time is acceptable (<30 minutes)
- **Safety**: Rollback works when needed
- **Visibility**: Deployment status is always clear
- **Auditability**: All changes are logged
- **Repeatability**: Same results every time

## Best Practices

### Do's ✅

1. **Automate Everything**: Manual steps lead to errors
2. **Test Before Production**: Always deploy to staging first
3. **Use Approval Gates**: Require manual approval for production
4. **Version Everything**: Code, infrastructure, configuration
5. **Implement Rollback**: Always have a way back
6. **Monitor Deployments**: Watch for errors during deployment
7. **Use Secrets Management**: Never commit credentials
8. **Tag Releases**: Clear version numbers for tracking
9. **Document Procedures**: Runbooks for common tasks
10. **Measure Performance**: Track deployment metrics

### Don'ts ❌

1. **Don't Deploy Manually**: Automation prevents mistakes
2. **Don't Skip Testing**: Tests catch problems early
3. **Don't Deploy on Fridays**: Leave time for fixes
4. **Don't Ignore Failures**: Fix broken pipelines immediately
5. **Don't Share Credentials**: Use service principals
6. **Don't Deploy Without Backup**: Always have rollback ready
7. **Don't Skip Approvals**: Follow the process
8. **Don't Ignore Monitoring**: Watch deployments closely
9. **Don't Make Direct Changes**: Use pipelines, not manual edits
10. **Don't Forget Documentation**: Update runbooks regularly

## Deployment Strategies

### Blue-Green Deployment

**When to Use**: Zero-downtime production deployments

**Process**:
1. Deploy new version to "green" environment
2. Test green environment
3. Switch traffic from "blue" to "green"
4. Keep blue as rollback option

**Advantages**:
- Zero downtime
- Easy rollback
- Full testing before switch

### Rolling Deployment

**When to Use**: Gradual rollout across instances

**Process**:
1. Update instances one at a time
2. Health check each instance
3. Continue to next instance
4. Rollback if issues detected

**Advantages**:
- Gradual rollout
- Reduced risk
- No extra resources needed

### Canary Deployment

**When to Use**: Test new version with subset of users

**Process**:
1. Deploy new version to small subset (5%)
2. Monitor for issues
3. Gradually increase traffic
4. Rollback if problems detected

**Advantages**:
- Limited blast radius
- Real user testing
- Easy rollback

## Pipeline Patterns

### CI Pipeline Pattern

```yaml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - Checkout code
      - Set up environment
      - Install dependencies
      - Run linters
      - Run unit tests
      - Run integration tests
      - Build artifact
      - Security scan
      - Publish artifact
```

### CD Pipeline Pattern

```yaml
name: CD Pipeline

on:
  workflow_run:
    workflows: ["CI Pipeline"]
    types: [completed]

jobs:
  deploy-dev:
    if: github.ref == 'refs/heads/develop'
    steps:
      - Download artifact
      - Provision infrastructure (Terraform)
      - Deploy application
      - Run smoke tests
      
  deploy-staging:
    needs: deploy-dev
    if: github.ref == 'refs/heads/main'
    steps:
      - Download artifact
      - Provision infrastructure
      - Deploy application
      - Run full tests
      
  deploy-production:
    needs: deploy-staging
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
      - Manual approval required
      - Download artifact
      - Blue-green deployment
      - Smoke tests
      - Switch traffic
```

## Templates Usage

### CI Pipeline Template

Use [TEMPLATE-PIPELINE-CI.md](TEMPLATE-PIPELINE-CI.md) for:
- Creating CI workflows
- Configuring build jobs
- Setting up test execution
- Publishing artifacts

### CD Pipeline Template

Use [TEMPLATE-PIPELINE-CD.md](TEMPLATE-PIPELINE-CD.md) for:
- Creating CD workflows
- Configuring deployment stages
- Setting up approvals
- Implementing rollback

## Getting Started

### Using This Stage

1. **Review Stage 2 Outputs**: Understand what needs to be deployed
2. **Review the Master Prompt**: See [PROMPT.md](PROMPT.md) for AI-assisted setup
3. **Set Up Secrets**: Configure required credentials
4. **Create CI Pipeline**: Use template to create workflow
5. **Create CD Pipeline**: Use template for deployments
6. **Test in Dev**: Deploy to development environment
7. **Promote to Staging**: Deploy to staging after dev success
8. **Deploy to Production**: Manual approval and deployment

### Example Usage

```bash
# 1. Review artifacts from Stage 2
ls -la artifacts/

# 2. Set up GitHub secrets
gh secret set AZURE_CREDENTIALS --body @azure-credentials.json
gh secret set REGISTRY_USERNAME --body "username"
gh secret set REGISTRY_PASSWORD --body "password"

# 3. Create CI pipeline
# Copy from TEMPLATE-PIPELINE-CI.md to .github/workflows/ci.yml

# 4. Create CD pipeline
# Copy from TEMPLATE-PIPELINE-CD.md to .github/workflows/cd.yml

# 5. Commit and push
git add .github/workflows/
git commit -m "ci: add CI/CD pipelines"
git push

# 6. Monitor pipeline execution
gh run watch

# 7. Check deployment
curl https://dev.example.com/health
```

## Tools & Technologies

### CI/CD Platforms
- **GitHub Actions**: Native GitHub integration
- **Azure DevOps**: Enterprise-grade pipelines
- **GitLab CI**: Integrated with GitLab
- **Jenkins**: Open-source automation server

### Infrastructure as Code
- **Terraform**: Multi-cloud IaC
- **Bicep**: Azure-native IaC
- **ARM Templates**: Azure Resource Manager
- **Pulumi**: Code-first IaC

### Container Tools
- **Docker**: Container runtime
- **Azure Container Registry**: Private registry
- **Docker Compose**: Multi-container apps
- **Kubernetes**: Container orchestration

### Deployment Tools
- **Azure CLI**: Command-line interface
- **kubectl**: Kubernetes CLI
- **Helm**: Kubernetes package manager
- **Ansible**: Configuration management

## Security Considerations

### Secrets Management
- Store secrets in Azure Key Vault
- Use GitHub Secrets for CI/CD credentials
- Rotate secrets regularly
- Never log secrets
- Use managed identities where possible

### Access Control
- Use service principals for automation
- Apply least privilege principle
- Implement approval gates
- Audit all deployments
- Separate environments

### Network Security
- Deploy to private networks
- Use private endpoints
- Configure NSGs/firewalls
- Enable DDoS protection
- Use WAF for web apps

## Troubleshooting

### Common Issues

**Pipeline Fails to Start**:
- Check workflow syntax
- Verify triggers are correct
- Check repository permissions

**Authentication Failures**:
- Verify secrets are set correctly
- Check service principal permissions
- Ensure credentials haven't expired

**Infrastructure Provisioning Fails**:
- Check Terraform state
- Verify Azure permissions
- Review resource quotas
- Check for naming conflicts

**Deployment Fails**:
- Check application logs
- Verify configuration
- Check resource health
- Review recent changes

**Tests Fail After Deployment**:
- Check environment configuration
- Verify database migrations
- Review application logs
- Check external dependencies

## Resources

- **[Master Prompt](PROMPT.md)**: AI-ready prompt for this stage
- **[CI Pipeline Template](TEMPLATE-PIPELINE-CI.md)**: Continuous Integration workflow
- **[CD Pipeline Template](TEMPLATE-PIPELINE-CD.md)**: Continuous Deployment workflow
- **[Framework Overview](../00-overview/README.md)**: Understand the full framework
- **[Stage 2 Output](../02-stage-develop-build/README.md)**: Build artifacts reference

---

[Back to Documentation Index](../README.md) | [Stage 3 Prompt](PROMPT.md) | [Previous: Stage 2](../02-stage-develop-build/README.md) | [Next: Stage 4](../04-stage-validate-operate/README.md)
