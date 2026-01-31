# Continuous Deployment Pipeline Template

**Pipeline Type**: CD (Continuous Deployment)  
**Platform**: GitHub Actions  
**Version**: 1.0  
**Status**: Production Ready

## Metadata

| Field | Value |
|-------|-------|
| **Purpose** | Automated deployment to multiple environments |
| **Triggers** | CI pipeline success, manual workflow |
| **Environments** | Development, Staging, Production |
| **Strategy** | Blue-Green / Rolling / Canary |
| **Duration** | ~20-45 minutes (varies by environment) |

---

## 1. Overview

This CD pipeline automates infrastructure provisioning and application deployment across multiple environments with appropriate safeguards and approval gates.

### Deployment Flow

```
CI Pipeline Success
    │
    ▼
Deploy to Development
    ├─→ Provision Infrastructure (Terraform)
    ├─→ Deploy Application
    ├─→ Run Smoke Tests
    └─→ Verify Health
    │
    ▼
Deploy to Staging (if dev succeeds)
    ├─→ Provision Infrastructure
    ├─→ Deploy Application
    ├─→ Run Full Test Suite
    └─→ Verify Health
    │
    ▼
Request Production Approval
    │
    ▼
Deploy to Production (manual approval)
    ├─→ Provision/Update Infrastructure
    ├─→ Blue-Green Deployment
    ├─→ Run Smoke Tests
    ├─→ Monitor for Errors
    └─→ Complete or Rollback
```

---

## 2. Complete Pipeline: GitHub Actions

### File: `.github/workflows/cd.yml`

```yaml
name: CD Pipeline

on:
  workflow_run:
    workflows: ["CI Pipeline"]
    types:
      - completed
    branches:
      - main
      - develop
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment to deploy'
        required: true
        type: choice
        options:
          - development
          - staging
          - production
      skip_tests:
        description: 'Skip smoke tests'
        required: false
        type: boolean
        default: false

env:
  TERRAFORM_VERSION: '1.5.0'
  AZURE_REGION: 'eastus'

jobs:
  # Pre-deployment checks
  pre-deployment:
    name: Pre-Deployment Checks
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' || github.event_name == 'workflow_dispatch' }}
    
    outputs:
      deploy_dev: ${{ steps.check.outputs.deploy_dev }}
      deploy_staging: ${{ steps.check.outputs.deploy_staging }}
      deploy_prod: ${{ steps.check.outputs.deploy_prod }}
      version: ${{ steps.version.outputs.version }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Determine deployments
        id: check
        run: |
          if [[ "${{ github.ref }}" == "refs/heads/develop" ]]; then
            echo "deploy_dev=true" >> $GITHUB_OUTPUT
            echo "deploy_staging=false" >> $GITHUB_OUTPUT
            echo "deploy_prod=false" >> $GITHUB_OUTPUT
          elif [[ "${{ github.ref }}" == "refs/heads/main" ]]; then
            echo "deploy_dev=false" >> $GITHUB_OUTPUT
            echo "deploy_staging=true" >> $GITHUB_OUTPUT
            echo "deploy_prod=true" >> $GITHUB_OUTPUT
          fi

      - name: Get version
        id: version
        run: |
          VERSION=$(git describe --tags --always)
          echo "version=$VERSION" >> $GITHUB_OUTPUT
          echo "Deploying version: $VERSION"

  # Development Environment
  deploy-development:
    name: Deploy to Development
    runs-on: ubuntu-latest
    timeout-minutes: 30
    needs: pre-deployment
    if: needs.pre-deployment.outputs.deploy_dev == 'true'
    environment:
      name: development
      url: https://dev.example.com

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS_DEV }}

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: ${{ env.TERRAFORM_VERSION }}

      - name: Terraform Init
        working-directory: ./infrastructure/environments/dev
        run: |
          terraform init \
            -backend-config="resource_group_name=${{ secrets.TF_STATE_RG }}" \
            -backend-config="storage_account_name=${{ secrets.TF_STATE_SA }}" \
            -backend-config="container_name=tfstate" \
            -backend-config="key=dev/terraform.tfstate"

      - name: Terraform Plan
        working-directory: ./infrastructure/environments/dev
        run: terraform plan -out=tfplan

      - name: Terraform Apply
        working-directory: ./infrastructure/environments/dev
        run: terraform apply -auto-approve tfplan

      - name: Get Terraform Outputs
        id: tf-output
        working-directory: ./infrastructure/environments/dev
        run: |
          APP_URL=$(terraform output -raw app_service_url)
          echo "app_url=$APP_URL" >> $GITHUB_OUTPUT

      - name: Deploy Application
        uses: azure/webapps-deploy@v2
        with:
          app-name: ${{ secrets.DEV_APP_NAME }}
          images: 'ghcr.io/${{ github.repository }}:${{ github.sha }}'

      - name: Run Database Migrations
        run: |
          az webapp ssh --name ${{ secrets.DEV_APP_NAME }} \
            --resource-group ${{ secrets.DEV_RG }} \
            --command "npm run migrate"

      - name: Wait for deployment
        run: sleep 30

      - name: Smoke Tests
        run: |
          curl -f ${{ steps.tf-output.outputs.app_url }}/health || exit 1
          curl -f ${{ steps.tf-output.outputs.app_url }}/api/status || exit 1

      - name: Notify Deployment
        if: always()
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
          payload: |
            {
              "text": "${{ job.status == 'success' && '✅' || '❌' }} Development deployment ${{ job.status }}",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Development Deployment*\n\nStatus: ${{ job.status }}\nVersion: ${{ needs.pre-deployment.outputs.version }}\nURL: ${{ steps.tf-output.outputs.app_url }}"
                  }
                }
              ]
            }

  # Staging Environment
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    timeout-minutes: 40
    needs: [pre-deployment, deploy-development]
    if: |
      always() &&
      needs.pre-deployment.outputs.deploy_staging == 'true' &&
      (needs.deploy-development.result == 'success' || needs.deploy-development.result == 'skipped')
    environment:
      name: staging
      url: https://staging.example.com

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS_STAGING }}

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: ${{ env.TERRAFORM_VERSION }}

      - name: Terraform Init
        working-directory: ./infrastructure/environments/staging
        run: |
          terraform init \
            -backend-config="resource_group_name=${{ secrets.TF_STATE_RG }}" \
            -backend-config="storage_account_name=${{ secrets.TF_STATE_SA }}" \
            -backend-config="container_name=tfstate" \
            -backend-config="key=staging/terraform.tfstate"

      - name: Terraform Plan
        working-directory: ./infrastructure/environments/staging
        run: terraform plan -out=tfplan

      - name: Terraform Apply
        working-directory: ./infrastructure/environments/staging
        run: terraform apply -auto-approve tfplan

      - name: Get Terraform Outputs
        id: tf-output
        working-directory: ./infrastructure/environments/staging
        run: |
          APP_URL=$(terraform output -raw app_service_url)
          APP_NAME=$(terraform output -raw app_service_name)
          echo "app_url=$APP_URL" >> $GITHUB_OUTPUT
          echo "app_name=$APP_NAME" >> $GITHUB_OUTPUT

      - name: Deploy to Staging Slot
        uses: azure/webapps-deploy@v2
        with:
          app-name: ${{ steps.tf-output.outputs.app_name }}
          slot-name: staging
          images: 'ghcr.io/${{ github.repository }}:${{ github.sha }}'

      - name: Run Database Migrations (Staging Slot)
        run: |
          az webapp ssh --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.STAGING_RG }} \
            --slot staging \
            --command "npm run migrate"

      - name: Wait for deployment
        run: sleep 30

      - name: Run Integration Tests
        run: |
          export TEST_URL="${{ steps.tf-output.outputs.app_url }}"
          npm run test:integration

      - name: Run E2E Tests
        run: |
          export TEST_URL="${{ steps.tf-output.outputs.app_url }}"
          npx playwright test

      - name: Swap Slots
        if: success()
        run: |
          az webapp deployment slot swap \
            --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.STAGING_RG }} \
            --slot staging \
            --target-slot production

      - name: Verify Production Slot
        run: |
          curl -f ${{ steps.tf-output.outputs.app_url }}/health || exit 1

      - name: Notify Deployment
        if: always()
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
          payload: |
            {
              "text": "${{ job.status == 'success' && '✅' || '❌' }} Staging deployment ${{ job.status }}",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Staging Deployment*\n\nStatus: ${{ job.status }}\nVersion: ${{ needs.pre-deployment.outputs.version }}\nURL: ${{ steps.tf-output.outputs.app_url }}"
                  }
                }
              ]
            }

  # Production Environment
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    timeout-minutes: 60
    needs: [pre-deployment, deploy-staging]
    if: |
      always() &&
      needs.pre-deployment.outputs.deploy_prod == 'true' &&
      needs.deploy-staging.result == 'success'
    environment:
      name: production
      url: https://example.com

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Create deployment
        id: deployment
        uses: actions/github-script@v7
        with:
          script: |
            const deployment = await github.rest.repos.createDeployment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              ref: context.sha,
              environment: 'production',
              required_contexts: [],
              auto_merge: false
            });
            return deployment.data.id;

      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS_PROD }}

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: ${{ env.TERRAFORM_VERSION }}

      - name: Terraform Init
        working-directory: ./infrastructure/environments/production
        run: |
          terraform init \
            -backend-config="resource_group_name=${{ secrets.TF_STATE_RG }}" \
            -backend-config="storage_account_name=${{ secrets.TF_STATE_SA }}" \
            -backend-config="container_name=tfstate" \
            -backend-config="key=production/terraform.tfstate"

      - name: Terraform Plan
        working-directory: ./infrastructure/environments/production
        run: terraform plan -out=tfplan

      - name: Terraform Apply
        working-directory: ./infrastructure/environments/production
        run: terraform apply -auto-approve tfplan

      - name: Get Terraform Outputs
        id: tf-output
        working-directory: ./infrastructure/environments/production
        run: |
          APP_URL=$(terraform output -raw app_service_url)
          APP_NAME=$(terraform output -raw app_service_name)
          echo "app_url=$APP_URL" >> $GITHUB_OUTPUT
          echo "app_name=$APP_NAME" >> $GITHUB_OUTPUT

      - name: Deploy to Blue Slot
        uses: azure/webapps-deploy@v2
        with:
          app-name: ${{ steps.tf-output.outputs.app_name }}
          slot-name: blue
          images: 'ghcr.io/${{ github.repository }}:${{ github.sha }}'

      - name: Run Database Migrations (Blue Slot)
        run: |
          az webapp ssh --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.PROD_RG }} \
            --slot blue \
            --command "npm run migrate"

      - name: Wait for deployment
        run: sleep 60

      - name: Smoke Tests on Blue Slot
        run: |
          BLUE_URL=$(az webapp deployment slot list \
            --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.PROD_RG }} \
            --query "[?name=='blue'].defaultHostName" -o tsv)
          curl -f https://$BLUE_URL/health || exit 1
          curl -f https://$BLUE_URL/api/status || exit 1

      - name: Start Traffic Shift (20%)
        run: |
          az webapp traffic-routing set \
            --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.PROD_RG }} \
            --distribution blue=20

      - name: Monitor for 5 minutes
        run: |
          echo "Monitoring error rates for 5 minutes..."
          sleep 300

      - name: Check Error Rate
        id: error-check
        run: |
          # Query Application Insights for error rate
          ERROR_RATE=$(az monitor app-insights metrics show \
            --app ${{ steps.tf-output.outputs.app_name }} \
            --metric requests/failed \
            --interval PT5M \
            --query "value.requests/failed.avg" -o tsv)
          
          if (( $(echo "$ERROR_RATE > 1" | bc -l) )); then
            echo "Error rate too high: $ERROR_RATE%"
            echo "rollback=true" >> $GITHUB_OUTPUT
            exit 1
          fi
          echo "rollback=false" >> $GITHUB_OUTPUT

      - name: Complete Traffic Shift (100%)
        if: steps.error-check.outputs.rollback == 'false'
        run: |
          az webapp deployment slot swap \
            --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.PROD_RG }} \
            --slot blue \
            --target-slot production

      - name: Rollback on Failure
        if: failure() && steps.error-check.outputs.rollback == 'true'
        run: |
          echo "Rolling back deployment..."
          az webapp traffic-routing clear \
            --name ${{ steps.tf-output.outputs.app_name }} \
            --resource-group ${{ secrets.PROD_RG }}

      - name: Verify Production
        if: success()
        run: |
          curl -f ${{ steps.tf-output.outputs.app_url }}/health || exit 1
          curl -f ${{ steps.tf-output.outputs.app_url }}/api/status || exit 1

      - name: Update deployment status
        if: always()
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.repos.createDeploymentStatus({
              owner: context.repo.owner,
              repo: context.repo.repo,
              deployment_id: ${{ steps.deployment.result }},
              state: '${{ job.status }}',
              environment_url: '${{ steps.tf-output.outputs.app_url }}'
            });

      - name: Notify Deployment
        if: always()
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
          payload: |
            {
              "text": "${{ job.status == 'success' && '🎉' || '🚨' }} Production deployment ${{ job.status }}",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Production Deployment*\n\nStatus: ${{ job.status }}\nVersion: ${{ needs.pre-deployment.outputs.version }}\nURL: ${{ steps.tf-output.outputs.app_url }}\nDeployed by: ${{ github.actor }}"
                  }
                }
              ]
            }

  # Rollback Workflow (Manual)
  rollback:
    name: Rollback Deployment
    runs-on: ubuntu-latest
    if: github.event_name == 'workflow_dispatch' && github.event.inputs.environment == 'production'
    
    steps:
      - name: Azure Login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS_PROD }}

      - name: Swap slots back
        run: |
          az webapp deployment slot swap \
            --name ${{ secrets.PROD_APP_NAME }} \
            --resource-group ${{ secrets.PROD_RG }} \
            --slot production \
            --target-slot blue

      - name: Verify Rollback
        run: |
          sleep 30
          curl -f https://${{ secrets.PROD_DOMAIN }}/health || exit 1

      - name: Notify Rollback
        uses: slackapi/slack-github-action@v1
        with:
          webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
          payload: |
            {
              "text": "⏮️ Production rolled back",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Production Rollback Complete*\n\nRolled back by: ${{ github.actor }}\nReason: Manual rollback requested"
                  }
                }
              ]
            }
```

---

## 3. Required Secrets

Configure these secrets for each environment:

### Azure Credentials

```bash
# Create service principal
az ad sp create-for-rbac \
  --name "github-actions-dev" \
  --role contributor \
  --scopes /subscriptions/{subscription-id}/resourceGroups/{rg-name} \
  --sdk-auth

# Set secret
gh secret set AZURE_CREDENTIALS_DEV --body @azure-credentials-dev.json
gh secret set AZURE_CREDENTIALS_STAGING --body @azure-credentials-staging.json
gh secret set AZURE_CREDENTIALS_PROD --body @azure-credentials-prod.json
```

### Application Secrets

| Secret Name | Description | Environment |
|-------------|-------------|-------------|
| `AZURE_CREDENTIALS_DEV` | Azure service principal for dev | Development |
| `AZURE_CREDENTIALS_STAGING` | Azure service principal for staging | Staging |
| `AZURE_CREDENTIALS_PROD` | Azure service principal for production | Production |
| `TF_STATE_RG` | Terraform state resource group | All |
| `TF_STATE_SA` | Terraform state storage account | All |
| `DEV_APP_NAME` | Development app service name | Development |
| `STAGING_APP_NAME` | Staging app service name | Staging |
| `PROD_APP_NAME` | Production app service name | Production |
| `DEV_RG` | Development resource group | Development |
| `STAGING_RG` | Staging resource group | Staging |
| `PROD_RG` | Production resource group | Production |
| `PROD_DOMAIN` | Production domain name | Production |
| `SLACK_WEBHOOK_URL` | Slack webhook for notifications | All |

---

## 4. Environment Configuration

### GitHub Environments

Create three environments in GitHub:

```bash
# Via GitHub UI: Settings -> Environments -> New environment

# development: No protection rules
# staging: No protection rules  
# production: 
#   - Required reviewers: 1-2 reviewers
#   - Wait timer: 0 minutes
#   - Deployment branches: main only
```

### Protection Rules

**Development**:
- Auto-deploy on develop branch
- No approvals required

**Staging**:
- Auto-deploy on main branch
- No approvals required
- Requires dev deployment success

**Production**:
- Manual approval required
- Requires staging deployment success
- Deployment branches: main only
- At least 1 reviewer required

---

## 5. Deployment Strategies

This template implements **Blue-Green Deployment** for production. Alternative strategies:

### Rolling Deployment

```yaml
- name: Rolling Update
  run: |
    for instance in $(az webapp list-instances --name $APP_NAME -g $RG --query "[].name" -o tsv); do
      az webapp restart --name $APP_NAME -g $RG --instance-name $instance
      sleep 60
      # Health check
      curl -f $APP_URL/health || exit 1
    done
```

### Canary Deployment

```yaml
- name: Canary Deployment (5% -> 25% -> 50% -> 100%)
  run: |
    for percentage in 5 25 50 100; do
      az webapp traffic-routing set \
        --name $APP_NAME -g $RG \
        --distribution canary=$percentage
      sleep 300
      # Check metrics
      check_error_rate || rollback_and_exit
    done
```

---

## 6. Rollback Procedures

### Automatic Rollback

Triggered automatically if:
- Health checks fail
- Error rate exceeds threshold
- Smoke tests fail

### Manual Rollback

```bash
# Via GitHub UI
gh workflow run cd.yml \
  -f environment=production \
  -f action=rollback

# Via Azure CLI
az webapp deployment slot swap \
  --name $APP_NAME \
  --resource-group $RG \
  --slot production \
  --target-slot blue
```

---

## 7. Monitoring During Deployment

### Metrics to Watch

- Request error rate
- Response time (P95, P99)
- CPU and memory usage
- Database connection pool
- External API call success rate

### Automated Checks

```yaml
- name: Check Application Insights
  run: |
    ERROR_RATE=$(az monitor app-insights metrics show ...)
    RESPONSE_TIME=$(az monitor app-insights metrics show ...)
    
    if [ $ERROR_RATE -gt 1 ]; then
      echo "Error rate too high"
      exit 1
    fi
```

---

## 8. Troubleshooting

### Deployment Fails

```bash
# Check deployment logs
az webapp log tail --name $APP_NAME -g $RG

# Check container logs
az webapp log download --name $APP_NAME -g $RG

# SSH into container
az webapp ssh --name $APP_NAME -g $RG
```

### Terraform Fails

```bash
# Check state
terraform state list

# Refresh state
terraform refresh

# Import existing resources
terraform import azurerm_app_service.main /subscriptions/.../resourceGroups/.../providers/Microsoft.Web/sites/...
```

### Slot Swap Issues

```bash
# Check slot status
az webapp deployment slot list --name $APP_NAME -g $RG

# Preview swap
az webapp deployment slot swap --name $APP_NAME -g $RG --slot blue --action preview

# Cancel swap
az webapp deployment slot swap --name $APP_NAME -g $RG --slot blue --action reset
```

---

## 9. Best Practices

### Do's ✅

1. **Always test in staging first**
2. **Use approval gates for production**
3. **Monitor deployments actively**
4. **Keep rollback procedures tested**
5. **Use blue-green or canary for production**
6. **Automate smoke tests**
7. **Log all deployment activities**
8. **Notify stakeholders of production deployments**

### Don'ts ❌

1. **Don't skip staging**
2. **Don't deploy without backup**
3. **Don't ignore monitoring alerts**
4. **Don't deploy large changes at once**
5. **Don't deploy on Fridays (unless necessary)**
6. **Don't skip approval for production**
7. **Don't ignore failed health checks**

---

## 10. Checklist

Before using this pipeline:

- [ ] All secrets configured
- [ ] Environments created with protection rules
- [ ] Terraform state backend configured
- [ ] Azure service principals created
- [ ] Deployment slots configured
- [ ] Monitoring and alerting set up
- [ ] Rollback procedure tested
- [ ] Team trained on approval process
- [ ] Runbooks documented
- [ ] Emergency contacts defined

---

**Template Version**: 1.0  
**Last Updated**: 2024-01-31

[Back to Stage 3 Overview](README.md) | [Stage 3 Prompt](PROMPT.md) | [CI Pipeline Template](TEMPLATE-PIPELINE-CI.md)
