# Stage 3: Master Prompt - Deploy & Deliver

## Instructions for Use

1. Copy this entire prompt
2. Replace all `[PLACEHOLDER]` values with your project-specific information
3. Paste into your AI assistant (GitHub Copilot Chat, ChatGPT, Claude, etc.)
4. Review and refine the generated pipelines
5. Follow the templates provided in this stage

---

## Master Prompt

```
You are an expert DevOps Engineer specializing in CI/CD automation and cloud deployments on Azure. Your task is to create comprehensive, production-ready CI/CD pipelines following the Devviaai framework.

### PROJECT CONTEXT

**Project Name**: [PROJECT_NAME]
**Stage**: Deploy & Deliver (Stage 3)
**Platform**: [CI/CD_PLATFORM] (GitHub Actions, Azure DevOps, etc.)
**Cloud Provider**: Azure
**Deployment Strategy**: [BLUE_GREEN | ROLLING | CANARY]

### INPUT SPECIFICATIONS

**From Stage 2 (Develop & Build)**:
[PASTE_ARTIFACTS_AND_BUILD_OUTPUTS_HERE]

**From Stage 1 (TBD)**:
**Resource Specifications**:
[PASTE_RES_AZ_XXX_SPECIFICATIONS_HERE]

**Environment Requirements**:
[PASTE_ENVIRONMENT_REQUIREMENTS_HERE]

**Security Requirements**:
[PASTE_SEC_XXX_REQUIREMENTS_HERE]

### YOUR TASK

Create complete CI/CD pipelines that automate the deployment process from code commit to production.

#### PIPELINE TYPE

Select what you need to generate:

**Option A: CI Pipeline (Continuous Integration)**
Generate a pipeline that:
1. Triggers on code push/PR
2. Checks out code
3. Installs dependencies
4. Runs linting and code quality checks
5. Executes unit tests
6. Executes integration tests
7. Runs security scans
8. Builds application artifacts
9. Builds container images
10. Publishes artifacts to registry

**Option B: CD Pipeline (Continuous Deployment)**
Generate a pipeline that:
1. Triggers on CI pipeline success
2. Provisions infrastructure (Terraform/Bicep)
3. Deploys to development environment
4. Runs smoke tests
5. Deploys to staging (after dev success)
6. Runs full test suite
7. Deploys to production (with approval)
8. Implements rollback capability

**Option C: Both CI and CD**
Generate complete pipelines for both continuous integration and continuous deployment.

### REQUIREMENTS FOR CI PIPELINE

#### 1. PIPELINE STRUCTURE

```yaml
name: CI Pipeline
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    # Build and test job
  
  security-scan:
    # Security scanning job
    
  publish:
    # Artifact publishing job
```

#### 2. BUILD JOB

Must include:
- Code checkout
- Environment setup (Node.js, Python, etc.)
- Dependency installation
- Caching for dependencies
- Linting and formatting checks
- Unit test execution with coverage
- Integration test execution
- Build artifact creation
- Test results publishing

#### 3. SECURITY SCANNING

Must include:
- Dependency vulnerability scanning
- Container image scanning
- Static code analysis
- Secret detection
- License compliance checking

#### 4. ARTIFACT PUBLISHING

Must include:
- Container image building
- Multi-arch support (optional)
- Image tagging (git SHA, semantic version)
- Pushing to container registry
- Artifact upload for non-container apps

#### 5. NOTIFICATIONS

Must include:
- Build status notifications
- Test failure alerts
- Security vulnerability alerts

### REQUIREMENTS FOR CD PIPELINE

#### 1. PIPELINE STRUCTURE

```yaml
name: CD Pipeline
on:
  workflow_run:
    workflows: ["CI Pipeline"]
    types: [completed]

jobs:
  deploy-dev:
    # Development deployment
    
  deploy-staging:
    # Staging deployment
    
  deploy-production:
    # Production deployment with approval
```

#### 2. INFRASTRUCTURE PROVISIONING

Must include:
- Terraform/Bicep initialization
- State management
- Plan generation
- Apply with approval (staging/prod)
- Output capture

#### 3. APPLICATION DEPLOYMENT

For each environment, must include:
- Download artifacts
- Configure environment variables
- Apply database migrations
- Deploy application
- Health check verification
- Smoke tests
- Deployment verification

#### 4. DEPLOYMENT STRATEGIES

Implement one of:
- **Blue-Green**: Deploy to inactive slot, test, then swap
- **Rolling**: Update instances gradually
- **Canary**: Deploy to small percentage, gradually increase

#### 5. APPROVAL GATES

Must include:
- Automatic deployment to dev
- Automatic deployment to staging (after dev)
- Manual approval for production
- Approval timeout configuration

#### 6. ROLLBACK CAPABILITY

Must include:
- Previous version tracking
- Rollback procedure
- Automated rollback on failure
- Manual rollback trigger

#### 7. NOTIFICATIONS

Must include:
- Deployment start notification
- Deployment success notification
- Deployment failure alerts
- Manual approval requests

### ENVIRONMENT-SPECIFIC REQUIREMENTS

#### Development Environment
- Deploy on every merge to develop branch
- No approval required
- Use minimal resources (cost optimization)
- Verbose logging enabled
- Short deployment timeout

#### Staging Environment
- Deploy on every merge to main branch
- No approval required
- Mirror production configuration
- Full test suite execution
- Medium deployment timeout

#### Production Environment
- Requires manual approval
- Blue-green or canary deployment
- Full monitoring during deployment
- Automated rollback on critical errors
- Long deployment timeout
- Change notification to stakeholders

### SECURITY REQUIREMENTS

#### Secrets Management
- All secrets stored in GitHub Secrets / Azure Key Vault
- No secrets in code or logs
- Secrets rotation capability
- Use managed identities where possible

#### Access Control
- Use service principals for Azure access
- Least privilege principle
- Separate credentials per environment
- Audit all access

#### Compliance
- Log all deployments
- Track who approved what
- Maintain deployment history
- Generate compliance reports

### OUTPUT FORMAT

Provide your pipelines in the following structure:

#### For CI Pipeline:

```
## CI Pipeline: [PROJECT_NAME]

### Overview
- **Purpose**: [Brief description]
- **Triggers**: [When pipeline runs]
- **Artifacts**: [What is produced]

### Pipeline File: .github/workflows/ci.yml

```yaml
[Complete CI pipeline YAML]
```

### Secrets Required

| Secret Name | Description | How to Obtain |
|-------------|-------------|---------------|
| [SECRET_1] | [Description] | [Instructions] |

### Usage

1. [Step by step instructions]

### Troubleshooting

[Common issues and solutions]
```

#### For CD Pipeline:

```
## CD Pipeline: [PROJECT_NAME]

### Overview
- **Purpose**: [Brief description]
- **Triggers**: [When pipeline runs]
- **Environments**: Dev, Staging, Production

### Pipeline File: .github/workflows/cd.yml

```yaml
[Complete CD pipeline YAML]
```

### Environments Configuration

#### Development
[Configuration details]

#### Staging
[Configuration details]

#### Production
[Configuration details]

### Secrets Required

| Secret Name | Description | How to Obtain |
|-------------|-------------|---------------|
| [SECRET_1] | [Description] | [Instructions] |

### Deployment Process

1. [Step by step deployment flow]

### Rollback Procedure

[How to rollback deployments]

### Troubleshooting

[Common issues and solutions]
```

### QUALITY REQUIREMENTS

Ensure your pipelines include:

#### CI Pipeline
- ✅ Runs on every push and PR
- ✅ All tests execute automatically
- ✅ Code coverage is measured and reported
- ✅ Security scans complete before artifact creation
- ✅ Artifacts are versioned with git SHA
- ✅ Build failures are clearly reported
- ✅ Caching is used for dependencies
- ✅ Build time is optimized (<15 minutes)

#### CD Pipeline
- ✅ Infrastructure provisioning is automated
- ✅ Each environment deploys sequentially
- ✅ Production requires manual approval
- ✅ Health checks verify deployment success
- ✅ Rollback procedure is implemented
- ✅ Deployment status is visible
- ✅ All secrets are managed securely
- ✅ Deployment is idempotent

#### Both Pipelines
- ✅ Well-commented and documented
- ✅ Error handling is comprehensive
- ✅ Notifications are configured
- ✅ Logs are structured and searchable
- ✅ Timeouts are configured appropriately
- ✅ Retries are implemented where needed

### ADDITIONAL GUIDANCE

#### For GitHub Actions:
- Use official actions where possible
- Cache dependencies for speed
- Use matrix strategy for multiple versions
- Implement concurrency controls
- Use environments for protection rules
- Store large files in artifacts, not repo

#### For Azure DevOps:
- Use pipeline templates for reusability
- Implement stage dependencies
- Use deployment jobs for environments
- Configure retention policies
- Use library variable groups
- Implement pipeline security

#### For Infrastructure Provisioning:
- Use remote state for Terraform
- Implement state locking
- Plan before apply
- Use workspaces for environments
- Output important values
- Handle sensitive outputs securely

#### For Application Deployment:
- Use health checks before traffic switch
- Implement gradual rollout
- Monitor error rates during deployment
- Keep previous version for quick rollback
- Test rollback procedure regularly
- Document deployment dependencies

#### For Monitoring:
- Log all deployment events
- Track deployment duration
- Monitor error rates during deployment
- Alert on deployment failures
- Create deployment dashboards
- Implement custom metrics

Begin your pipeline generation now. Create production-ready, comprehensive CI/CD pipelines that fully automate the deployment process.
```

---

## Example Usage

### Step 1: Gather Your Information

From Stage 2, collect:
- Container image names and tags
- Terraform module locations
- Application configurations
- Test suite commands
- Dependencies

From Stage 1 (TBD), collect:
- Resource specifications (RES-AZ-XXX)
- Environment requirements
- Security requirements (SEC-XXX)

### Step 2: Fill in the Placeholders

Replace:
- `[PROJECT_NAME]`: "Customer Portal"
- `[CI/CD_PLATFORM]`: "GitHub Actions"
- `[DEPLOYMENT_STRATEGY]`: "Blue-Green"
- `[PASTE_ARTIFACTS_AND_BUILD_OUTPUTS_HERE]`: List your artifacts
- `[PASTE_RES_AZ_XXX_SPECIFICATIONS_HERE]`: Copy resource specs
- etc.

### Step 3: Use with Your AI Assistant

**GitHub Copilot**:
```bash
# In VS Code
1. Open Copilot Chat
2. Paste the customized prompt
3. Review generated pipelines
4. Save to .github/workflows/
```

**ChatGPT/Claude**:
```bash
1. Start a new conversation
2. Paste the customized prompt
3. Download generated pipelines
4. Save to your repository
5. Configure secrets
```

### Step 4: Configure Secrets

After generation:
```bash
# Azure credentials
gh secret set AZURE_CREDENTIALS --body @azure-credentials.json

# Container registry
gh secret set REGISTRY_USERNAME --body "username"
gh secret set REGISTRY_PASSWORD --body "password"

# Application secrets
gh secret set DATABASE_PASSWORD --body "dbpassword"
gh secret set JWT_SECRET --body "jwtsecret"
```

### Step 5: Configure Environments

For GitHub Actions:
```bash
# Create environments
gh api repos/:owner/:repo/environments/production -f deployment_branch_policy=null

# Add protection rules via GitHub UI
# Settings -> Environments -> protection rules
```

### Step 6: Test Pipelines

```bash
# Commit pipelines
git add .github/workflows/
git commit -m "ci: add CI/CD pipelines"
git push

# Watch execution
gh run watch

# Check logs
gh run view --log
```

## Follow-Up Prompts

After initial generation, you may need refinements:

### For CI Pipeline

```
"Add a job to run end-to-end tests using Playwright."

"Implement multi-architecture container builds (amd64, arm64)."

"Add a security scan using Snyk for dependency vulnerabilities."

"Optimize build time by implementing better caching strategy."

"Add a job to generate and publish API documentation."

"Implement semantic versioning for releases."
```

### For CD Pipeline

```
"Add a canary deployment strategy for production."

"Implement automated rollback if error rate exceeds 1%."

"Add a job to run database migrations before deployment."

"Configure blue-green deployment with Azure App Service slots."

"Add smoke tests that verify critical functionality after deployment."

"Implement deployment notifications to Slack/Teams."
```

### For Both Pipelines

```
"Add concurrency controls to prevent multiple deployments."

"Implement deployment queues for production."

"Add cost estimation before infrastructure changes."

"Create a manual workflow to rollback any environment."

"Add integration with change management system."

"Implement progressive deployment with traffic splitting."
```

## Validation Checklist

Before using pipelines in production, verify:

### CI Pipeline
- [ ] Pipeline triggers on correct branches
- [ ] All tests execute successfully
- [ ] Code coverage meets minimum threshold
- [ ] Security scans complete without critical issues
- [ ] Artifacts are created and versioned correctly
- [ ] Container images build successfully
- [ ] Images are pushed to registry
- [ ] Build notifications work

### CD Pipeline
- [ ] Infrastructure provisioning works
- [ ] Development deployment is automatic
- [ ] Staging deployment follows dev success
- [ ] Production requires manual approval
- [ ] Health checks validate deployment
- [ ] Smoke tests pass after deployment
- [ ] Rollback procedure works
- [ ] Deployment notifications send

### Security
- [ ] All secrets are configured
- [ ] No secrets in logs or code
- [ ] Service principals have minimum permissions
- [ ] Environments have protection rules
- [ ] Audit logging is enabled

### Documentation
- [ ] Pipeline documentation is complete
- [ ] Secrets are documented
- [ ] Deployment process is documented
- [ ] Rollback procedure is documented
- [ ] Troubleshooting guide exists

## Common Pitfalls to Avoid

### CI Pipeline Pitfalls
1. **No Caching**: Builds are slow without dependency caching
2. **Running All Tests on PR**: Run quick tests on PR, full suite on merge
3. **Not Failing Fast**: Stop pipeline early on critical failures
4. **Poor Test Organization**: Separate unit, integration, e2e tests
5. **No Artifact Versioning**: Always version artifacts with git SHA
6. **Ignoring Build Time**: Optimize for speed
7. **No Parallel Jobs**: Run independent jobs in parallel
8. **Logging Secrets**: Be careful with debug logs

### CD Pipeline Pitfalls
1. **No Approval Gates**: Always require approval for production
2. **Deploying to All Environments**: Use progression (dev -> staging -> prod)
3. **No Rollback Plan**: Always have a way back
4. **No Health Checks**: Verify deployment succeeded
5. **Applying Infra Changes Blindly**: Always plan first
6. **No Deployment Verification**: Test after deployment
7. **Manual Configuration**: Automate all configuration
8. **No Monitoring**: Watch deployments for errors

### Security Pitfalls
1. **Secrets in Code**: Use secret management
2. **Over-Permissioned Service Principals**: Least privilege
3. **Shared Credentials**: Separate per environment
4. **No Audit Trail**: Log all deployments
5. **No Secret Rotation**: Regularly rotate secrets
6. **Trusting User Input**: Validate all inputs
7. **No Network Security**: Use private endpoints

## Next Steps

After generating pipelines:

1. **Review Code**: Understand every line
2. **Configure Secrets**: Set all required secrets
3. **Test in Dev**: Deploy to development first
4. **Validate**: Ensure everything works
5. **Document**: Update runbooks
6. **Train Team**: Ensure team knows the process
7. **Monitor**: Watch first few deployments closely
8. **Iterate**: Improve based on experience
9. **Proceed to Stage 4**: Set up monitoring and operations

---

[Back to Stage 3 Overview](README.md) | [CI Template](TEMPLATE-PIPELINE-CI.md) | [CD Template](TEMPLATE-PIPELINE-CD.md) | [Documentation Index](../README.md)
