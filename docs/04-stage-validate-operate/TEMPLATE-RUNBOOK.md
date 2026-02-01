# Operational Runbook Template

**Runbook Type**: [Deployment / Incident Response / Troubleshooting / Maintenance]  
**Version**: 1.0  
**Last Updated**: [DATE]  
**Owner**: [TEAM/PERSON]

## Metadata

| Field | Value |
|-------|-------|
| **Purpose** | [Brief description] |
| **Scope** | [What this covers] |
| **Prerequisites** | [Required access, tools, knowledge] |
| **Estimated Time** | [Duration] |
| **Frequency** | [How often used] |

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Step-by-Step Procedure](#step-by-step-procedure)
4. [Verification](#verification)
5. [Rollback](#rollback)
6. [Common Issues](#common-issues)
7. [Related Runbooks](#related-runbooks)

---

## 1. Overview

### Purpose
[What this runbook accomplishes]

### When to Use
[Scenarios when this procedure is needed]

### Out of Scope
[What this runbook doesn't cover]

---

## 2. Prerequisites

### Required Access
- [ ] Azure Portal access
- [ ] GitHub repository access
- [ ] SSH access to production systems
- [ ] PagerDuty/on-call access

### Required Tools
- [ ] Azure CLI installed and logged in
- [ ] kubectl configured (if applicable)
- [ ] Git installed
- [ ] [Other tools]

### Required Knowledge
- [ ] Basic understanding of the system architecture
- [ ] Familiarity with [specific technologies]
- [ ] Access to monitoring dashboards

### Before You Begin
1. Review the current system status
2. Check for any ongoing incidents
3. Notify the team in #ops-channel
4. Ensure you have backup/rollback plan

---

## 3. Step-by-Step Procedure

### Step 1: [Action Name]

**Purpose**: [Why this step is needed]

**Commands**:
```bash
# Example command
az login
az account set --subscription "Production"
```

**Expected Output**:
```
[Show what success looks like]
```

**If This Fails**:
- Check [specific thing]
- Verify [another thing]
- See [Common Issues](#common-issues) section

### Step 2: [Next Action]

**Purpose**: [Why this step is needed]

**Commands**:
```bash
# More commands
```

**Expected Output**:
```
[Expected result]
```

**Validation**:
```bash
# How to verify this step worked
curl -f https://api.example.com/health
```

### Step 3: [Continue...]

[Repeat pattern for all steps]

---

## 4. Verification

### Health Checks

Run these checks to verify the procedure succeeded:

```bash
# 1. Check application health
curl https://example.com/health
# Expected: {"status": "healthy"}

# 2. Check metrics
az monitor metrics list \
  --resource <resource-id> \
  --metric "Percentage CPU" \
  --start-time 2024-01-01T00:00:00Z \
  --interval PT5M

# 3. Check logs for errors
az monitor app-insights query \
  --app my-app \
  --analytics-query "exceptions | where timestamp > ago(5m)"
```

### Success Criteria

- [ ] Health endpoint returns 200 OK
- [ ] No error spikes in Application Insights
- [ ] Response time within normal range
- [ ] All automated tests pass
- [ ] Monitoring shows normal metrics

### Post-Procedure Tasks

1. Update documentation if procedure changed
2. Notify team of completion
3. Update incident ticket (if applicable)
4. Record in change log

---

## 5. Rollback

### When to Rollback

Rollback if:
- Health checks fail after 5 minutes
- Error rate exceeds 5%
- Any critical functionality is broken
- Performance degrades significantly

### Rollback Procedure

#### Option 1: Quick Rollback (Slot Swap)

```bash
# Swap back to previous slot
az webapp deployment slot swap \
  --name my-app \
  --resource-group my-rg \
  --slot blue \
  --target-slot production
```

#### Option 2: Full Rollback (Redeploy Previous Version)

```bash
# Revert to previous deployment
cd /path/to/repo
git revert <commit-hash>
git push origin main

# Or redeploy specific version
az webapp deployment source sync \
  --name my-app \
  --resource-group my-rg
```

#### Option 3: Infrastructure Rollback

```bash
# Revert Terraform changes
cd infrastructure/environments/production
git checkout <previous-commit>
terraform init
terraform plan
terraform apply
```

### Post-Rollback

1. Verify system is stable
2. Notify stakeholders
3. Create incident report
4. Schedule post-mortem
5. Update runbook based on learnings

---

## 6. Common Issues

### Issue 1: [Problem Description]

**Symptoms**:
- [What you observe]
- [Error messages]

**Root Cause**:
[Why this happens]

**Resolution**:
```bash
# Steps to fix
```

**Prevention**:
[How to avoid in future]

### Issue 2: [Another Problem]

[Follow same pattern]

### Issue 3: Authentication Failures

**Symptoms**:
- Azure CLI commands fail with "Unauthorized"
- "Token expired" errors

**Resolution**:
```bash
# Re-authenticate
az login
az account set --subscription "Production"

# Verify access
az account show
```

### Issue 4: Resource Not Found

**Symptoms**:
- "Resource not found" errors
- Commands fail with 404

**Resolution**:
```bash
# Verify resource exists
az resource list --resource-group my-rg

# Check correct subscription
az account show

# Verify resource ID
az webapp show --name my-app --resource-group my-rg --query id
```

---

## 7. Related Runbooks

- [Deployment Runbook](deployment-runbook.md) - Production deployment procedure
- [Incident Response](incident-response.md) - How to handle incidents
- [Rollback Procedure](rollback-runbook.md) - Detailed rollback steps
- [Database Maintenance](database-maintenance.md) - Database operations
- [Certificate Renewal](cert-renewal.md) - SSL certificate updates

---

## 8. Examples

### Example 1: Complete Deployment Flow

```bash
# 1. Check current deployment
az webapp show --name my-app --resource-group my-rg \
  --query "{name:name, state:state, hostNames:hostNames}"

# 2. Deploy to staging slot
az webapp deployment slot create \
  --name my-app \
  --resource-group my-rg \
  --slot staging

az webapp config container set \
  --name my-app \
  --resource-group my-rg \
  --slot staging \
  --docker-custom-image-name myregistry.azurecr.io/my-app:v1.2.3

# 3. Wait and test
sleep 60
curl https://my-app-staging.azurewebsites.net/health

# 4. Swap slots
az webapp deployment slot swap \
  --name my-app \
  --resource-group my-rg \
  --slot staging

# 5. Verify production
curl https://my-app.azurewebsites.net/health
```

### Example 2: Troubleshooting High Error Rate

```bash
# 1. Check current error rate
az monitor app-insights query \
  --app my-app \
  --analytics-query "requests | where timestamp > ago(10m) | summarize ErrorRate = countif(success==false) * 100.0 / count()"

# 2. Get recent exceptions
az monitor app-insights query \
  --app my-app \
  --analytics-query "exceptions | where timestamp > ago(10m) | project timestamp, type, outerMessage | take 20"

# 3. Check dependent services
az monitor app-insights query \
  --app my-app \
  --analytics-query "dependencies | where timestamp > ago(10m) and success == false | project timestamp, type, target, resultCode"

# 4. Check logs
az webapp log tail --name my-app --resource-group my-rg
```

---

## 9. Decision Tree

```
Is this a critical incident?
├─ Yes → Follow Incident Response Runbook
└─ No → Continue with this procedure

Did all prerequisites pass?
├─ No → Stop and resolve prerequisites first
└─ Yes → Proceed to Step 1

Did Step X fail?
├─ Yes → Check Common Issues section
└─ No → Continue to next step

Did verification pass?
├─ No → Execute rollback
└─ Yes → Complete post-procedure tasks
```

---

## 10. Contact Information

### Primary Contact
- **Name**: [On-call engineer]
- **Slack**: @oncall
- **Phone**: [Emergency phone]

### Escalation Path
1. **Level 1** (0-15 min): On-call engineer
2. **Level 2** (15-30 min): Senior engineer - [Name] - [Contact]
3. **Level 3** (30-60 min): Engineering manager - [Name] - [Contact]
4. **Level 4** (>60 min): VP Engineering - [Name] - [Contact]

### Support Resources
- **Slack Channels**: #ops, #incidents, #engineering
- **Documentation**: https://wiki.example.com
- **Monitoring**: https://portal.azure.com
- **Incident Management**: https://pagerduty.com

---

## 11. Change History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2024-01-01 | [Name] | Initial version |
| 1.1 | 2024-01-15 | [Name] | Added rollback section |
| 1.2 | 2024-02-01 | [Name] | Updated based on incident #123 |

---

## 12. Appendix

### Useful Queries

```kql
// Find slow queries
dependencies
| where timestamp > ago(1h) and type == "SQL"
| where duration > 1000
| order by duration desc
| take 20

// Top errors
exceptions
| where timestamp > ago(24h)
| summarize Count = count() by type, outerMessage
| order by Count desc
| take 10

// Request rate by endpoint
requests
| where timestamp > ago(1h)
| summarize Count = count() by name
| order by Count desc
```

### Useful Commands

```bash
# Get resource metrics
az monitor metrics list \
  --resource <resource-id> \
  --metric "Percentage CPU" "Memory Percentage" \
  --start-time 2024-01-01T00:00:00Z \
  --interval PT5M

# List recent deployments
az webapp deployment list-publishing-profiles \
  --name my-app \
  --resource-group my-rg

# View container logs
az webapp log download \
  --name my-app \
  --resource-group my-rg \
  --log-file logs.zip

# SSH into container
az webapp ssh --name my-app --resource-group my-rg
```

---

**Template Version**: 1.0  
**Last Updated**: 2025-01-31

[Back to Stage 4 Overview](README.md)
