# Stage 4: Validate & Operate

## Overview

Stage 4 ensures deployed systems meet requirements and remain healthy in production. Acting as an **Site Reliability Engineer (SRE)**, you'll implement monitoring, establish operational procedures, validate performance against NFRs, and maintain system health.

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
Validate that deployed systems meet all requirements, establish comprehensive monitoring and alerting, and ensure reliable ongoing operations.

### Key Objectives

1. **Requirements Validation**: Verify all FR-XXX and NFR-XXX are met
2. **Monitoring Setup**: Implement comprehensive observability
3. **Alerting Configuration**: Create meaningful alerts and escalations
4. **Performance Validation**: Confirm NFRs (response time, availability, etc.)
5. **Operational Readiness**: Establish runbooks and procedures
6. **Incident Response**: Define and practice incident management
7. **Continuous Improvement**: Analyze metrics and optimize

## Role Definition

### Site Reliability Engineer Responsibilities

As an SRE in Stage 4, you are responsible for:

- **Monitoring**: Implementing comprehensive observability
- **Alerting**: Creating actionable alerts
- **Incident Response**: Managing and resolving incidents
- **Performance Analysis**: Analyzing system performance
- **Capacity Planning**: Forecasting and planning for growth
- **Automation**: Automating operational tasks
- **Documentation**: Maintaining runbooks and procedures
- **Post-Mortems**: Learning from incidents

### Skills Required

- Monitoring tools (Azure Monitor, Application Insights, Prometheus)
- Alerting and incident management
- Performance analysis and optimization
- Query languages (KQL, PromQL)
- Troubleshooting and debugging
- Documentation
- On-call practices
- Communication skills

## Input Requirements

### Required Inputs

1. **From Stage 3 (Deploy & Deliver)**
   - Running environments (dev, staging, production)
   - Deployment logs and metrics
   - Application URLs and endpoints
   - Infrastructure configuration

2. **From Stage 1 (TBD)**
   - Non-functional requirements (NFR-XXX)
   - SLA commitments
   - Performance targets
   - Security requirements

3. **From Stage 2 (Develop & Build)**
   - Application logs structure
   - Health check endpoints
   - Metrics endpoints
   - Error handling patterns

### Optional Inputs

- Historical performance data
- Known issues and workarounds
- User feedback and complaints
- Business KPIs

## Output Specifications

### Primary Outputs

#### 1. Monitoring Dashboard

**Purpose**: Centralized view of system health

**Components**:
- Application metrics (requests, errors, latency)
- Infrastructure metrics (CPU, memory, disk, network)
- Business metrics (active users, transactions, revenue)
- Deployment markers
- Alert indicators

#### 2. Alerting Rules

**Purpose**: Proactive problem detection

**Alert Types**:
- **Critical**: Page on-call immediately
- **Warning**: Create ticket for investigation
- **Info**: Log for analysis

**Alert Categories**:
- Availability (service down, health check failures)
- Performance (high latency, slow queries)
- Errors (error rate spike, exceptions)
- Capacity (high CPU, memory exhaustion, disk full)
- Security (authentication failures, suspicious activity)

#### 3. Operational Runbooks

**Purpose**: Standardized operational procedures

**Runbooks**:
- Deployment procedures
- Rollback procedures
- Incident response
- Common troubleshooting
- Maintenance tasks
- Disaster recovery

#### 4. Validation Report

**Purpose**: Confirm all requirements are met

**Validates**:
- All functional requirements (FR-XXX)
- All non-functional requirements (NFR-XXX)
- Performance targets
- Security controls
- Compliance requirements

#### 5. On-Call Schedule

**Purpose**: 24/7 incident response coverage

**Components**:
- Rotation schedule
- Escalation paths
- Contact information
- Handoff procedures

### Supporting Outputs

- **SLI/SLO Definitions**: Service level indicators and objectives
- **Capacity Plan**: Growth projections and scaling plan
- **Performance Baselines**: Normal operating metrics
- **Incident Reports**: Post-mortem documents
- **Improvement Backlog**: Identified optimizations

## Process Flow

### Step-by-Step Process

```
1. Monitoring Setup
   │
   ├─→ Configure Application Insights
   ├─→ Set up Log Analytics workspace
   ├─→ Configure custom metrics
   ├─→ Enable distributed tracing
   └─→ Configure retention policies
   │
   ▼
2. Dashboard Creation
   │
   ├─→ Create system health dashboard
   ├─→ Create performance dashboard
   ├─→ Create business metrics dashboard
   ├─→ Create cost monitoring dashboard
   └─→ Share with team
   │
   ▼
3. Alerting Configuration
   │
   ├─→ Define alert rules
   ├─→ Set thresholds based on baselines
   ├─→ Configure notification channels
   ├─→ Set up escalation policies
   └─→ Test alerts
   │
   ▼
4. Requirements Validation
   │
   ├─→ Test all functional requirements
   ├─→ Measure performance metrics
   ├─→ Verify availability SLA
   ├─→ Validate security controls
   └─→ Document results
   │
   ▼
5. Runbook Creation
   │
   ├─→ Document deployment procedures
   ├─→ Create troubleshooting guides
   ├─→ Write incident response procedures
   ├─→ Document common issues
   └─→ Review with team
   │
   ▼
6. Load Testing
   │
   ├─→ Create load test scenarios
   ├─→ Execute load tests
   ├─→ Analyze results
   ├─→ Identify bottlenecks
   └─→ Optimize as needed
   │
   ▼
7. Incident Drill
   │
   ├─→ Simulate production incident
   ├─→ Practice response procedures
   ├─→ Test alerting and escalation
   ├─→ Document lessons learned
   └─→ Update procedures
   │
   ▼
8. Go-Live Readiness
   │
   ├─→ Complete validation checklist
   ├─→ Obtain stakeholder sign-off
   ├─→ Establish on-call schedule
   ├─→ Brief support team
   └─→ Enable production monitoring
```

## Integration with Other Stages

### Receives from Stage 3 (Deploy & Deliver)

- **Deployed Systems**: Running applications and infrastructure
- **Monitoring Endpoints**: Health checks, metrics endpoints
- **Deployment History**: When and what was deployed
- **Configuration**: As-deployed settings

### Provides Feedback to Stage 3

- **Deployment Issues**: Problems discovered post-deployment
- **Configuration Problems**: Incorrect settings
- **Performance Issues**: Slow deployments or rollbacks
- **Monitoring Gaps**: Missing telemetry

### Provides Feedback to Stage 2

- **Code Issues**: Bugs discovered in production
- **Performance Problems**: Inefficient code or queries
- **Missing Instrumentation**: Gaps in logging or metrics
- **Error Handling**: Inadequate error handling

### Provides Feedback to Stage 1

- **NFR Violations**: Requirements not met
- **Capacity Issues**: Insufficient resources
- **Missing Requirements**: Discovered operational needs
- **Design Issues**: Architectural problems

## Quality Checklist

### Before Go-Live

#### Monitoring
- [ ] Application Insights configured
- [ ] Custom metrics are collected
- [ ] Logs are structured and searchable
- [ ] Distributed tracing works
- [ ] Dashboards are created and shared
- [ ] Log retention is configured

#### Alerting
- [ ] Critical alerts are defined
- [ ] Warning alerts are defined
- [ ] Alert thresholds are set
- [ ] Notification channels configured
- [ ] Escalation policies defined
- [ ] Alerts have runbook links
- [ ] Alert fatigue is minimized

#### Validation
- [ ] All FR-XXX tested and passing
- [ ] All NFR-XXX measured and meeting targets
- [ ] Performance targets met
- [ ] Availability SLA achievable
- [ ] Security controls verified
- [ ] Compliance requirements met

#### Operational Readiness
- [ ] Runbooks are complete
- [ ] On-call schedule established
- [ ] Team is trained
- [ ] Incident response tested
- [ ] Rollback tested
- [ ] Backup and restore tested
- [ ] Disaster recovery plan exists

#### Documentation
- [ ] Architecture documented
- [ ] Monitoring guide complete
- [ ] Troubleshooting guide exists
- [ ] Known issues documented
- [ ] Contact information updated
- [ ] Change log maintained

### Quality Attributes

- **Observability**: Can understand system state from metrics
- **Reliability**: System meets availability SLA
- **Performance**: System meets performance NFRs
- **Responsiveness**: Incidents detected and resolved quickly
- **Maintainability**: Operations are documented and repeatable

## Best Practices

### Do's ✅

1. **Monitor What Matters**: Focus on user-impacting metrics
2. **Actionable Alerts**: Every alert should require action
3. **Document Everything**: Runbooks for common tasks
4. **Test in Production**: Chaos engineering and game days
5. **Learn from Incidents**: Post-mortems without blame
6. **Automate Toil**: Reduce manual operational work
7. **Set SLOs**: Define and track service level objectives
8. **Capacity Planning**: Forecast and plan for growth
9. **Practice Incidents**: Regular incident response drills
10. **Continuous Improvement**: Regular retrospectives

### Don'ts ❌

1. **Don't Alert on Everything**: Causes alert fatigue
2. **Don't Ignore Warnings**: Small problems become big
3. **Don't Skip Post-Mortems**: Learn from every incident
4. **Don't Wing It**: Have documented procedures
5. **Don't Sacrifice Reliability for Speed**: Balance is key
6. **Don't Ignore User Feedback**: Users tell you what's broken
7. **Don't Over-Complicate**: Simple solutions are better
8. **Don't Neglect Security Monitoring**: Watch for threats
9. **Don't Forget Business Metrics**: Technical + business = complete picture
10. **Don't Burnout**: Sustainable on-call practices

## Monitoring Strategy

### Golden Signals

Monitor these four key metrics:

1. **Latency**: How long requests take
2. **Traffic**: How many requests received
3. **Errors**: Rate of failed requests
4. **Saturation**: How "full" the system is

### RED Method (for services)

- **Rate**: Requests per second
- **Errors**: Failed requests per second
- **Duration**: Time per request

### USE Method (for resources)

- **Utilization**: Percentage used
- **Saturation**: Queue length
- **Errors**: Error count

## SLI/SLO/SLA Definitions

### Service Level Indicator (SLI)

Quantitative measure of service level:
- Request success rate: 99.9%
- Response time P95: < 200ms
- Availability: 99.95%

### Service Level Objective (SLO)

Target value for an SLI:
- 99.9% of requests succeed
- 95% of requests complete in < 200ms
- Service available 99.95% of time

### Service Level Agreement (SLA)

Commitment to customers:
- 99.9% uptime guaranteed
- < $X credits if not met
- 24/7 support included

## Templates Usage

### Monitoring Template

Use [TEMPLATE-MONITORING.md](TEMPLATE-MONITORING.md) for:
- Configuring Azure Monitor
- Creating dashboards
- Setting up alerts
- Defining SLIs/SLOs

### Runbook Template

Use [TEMPLATE-RUNBOOK.md](TEMPLATE-RUNBOOK.md) for:
- Documenting procedures
- Troubleshooting guides
- Incident response
- Maintenance tasks

## Getting Started

### Using This Stage

1. **Review Deployed System**: Understand what's running
2. **Review Master Prompt**: See [PROMPT.md](PROMPT.md) for AI-assisted setup
3. **Configure Monitoring**: Set up Application Insights
4. **Create Dashboards**: Visualize key metrics
5. **Set Up Alerts**: Define alert rules
6. **Validate Requirements**: Test all NFRs
7. **Create Runbooks**: Document procedures
8. **Establish On-Call**: Set up rotation

### Example Usage

```bash
# 1. Access Azure Monitor
az monitor app-insights component show \
  --app my-app --resource-group my-rg

# 2. Query logs
az monitor app-insights query \
  --app my-app \
  --analytics-query "requests | where timestamp > ago(1h) | summarize count() by resultCode"

# 3. Create alert
az monitor metrics alert create \
  --name high-error-rate \
  --resource-group my-rg \
  --scopes /subscriptions/.../resourceGroups/.../providers/Microsoft.Insights/components/my-app \
  --condition "avg requests/failed > 1" \
  --window-size 5m \
  --evaluation-frequency 1m

# 4. Run load test
npm run load-test -- --users 1000 --duration 10m

# 5. Check SLI compliance
npm run sli-report
```

## Tools & Technologies

### Monitoring & Observability
- **Azure Monitor**: Cloud-native monitoring
- **Application Insights**: APM solution
- **Log Analytics**: Log aggregation and search
- **Azure Workbooks**: Interactive reports
- **Grafana**: Visualization (alternative)
- **Prometheus**: Metrics (alternative)

### Alerting
- **Azure Monitor Alerts**: Native alerting
- **PagerDuty**: Incident management
- **Opsgenie**: Alert management
- **Slack/Teams**: Collaboration and notifications

### Load Testing
- **Azure Load Testing**: Cloud-based load testing
- **k6**: Modern load testing
- **JMeter**: Traditional load testing
- **Artillery**: npm-based load testing

### Incident Management
- **PagerDuty**: On-call management
- **Opsgenie**: Incident response
- **Statuspage**: Public status pages
- **Jira**: Ticket tracking

## Troubleshooting Common Issues

### High Latency

**Symptoms**: P95 response time > threshold
**Investigation**:
```bash
# Query slow requests
az monitor app-insights query --app my-app \
  --analytics-query "requests | where duration > 1000 | top 100 by duration"

# Check dependencies
az monitor app-insights query --app my-app \
  --analytics-query "dependencies | where duration > 500 | summarize count() by name"
```

**Resolution**: Optimize slow queries, add caching, scale resources

### High Error Rate

**Symptoms**: Failed requests > threshold
**Investigation**:
```bash
# Query errors
az monitor app-insights query --app my-app \
  --analytics-query "requests | where success == false | top 100 by timestamp"

# Check exceptions
az monitor app-insights query --app my-app \
  --analytics-query "exceptions | top 100 by timestamp"
```

**Resolution**: Fix bugs, improve error handling, check dependencies

### High CPU/Memory

**Symptoms**: Resource utilization > 80%
**Investigation**:
```bash
# Check metrics
az monitor metrics list --resource $RESOURCE_ID \
  --metric "Percentage CPU" "Memory Percentage" \
  --start-time 2024-01-01T00:00:00Z --interval PT1M
```

**Resolution**: Scale up/out, optimize code, find memory leaks

## Resources

- **[Master Prompt](PROMPT.md)**: AI-ready prompt for this stage
- **[Monitoring Template](TEMPLATE-MONITORING.md)**: Complete monitoring setup
- **[Runbook Template](TEMPLATE-RUNBOOK.md)**: Operational procedures
- **[Framework Overview](../00-overview/README.md)**: Understand the full framework
- **[Stage 3 Output](../03-stage-deploy-deliver/README.md)**: Deployed systems

---

[Back to Documentation Index](../README.md) | [Stage 4 Prompt](PROMPT.md) | [Previous: Stage 3](../03-stage-deploy-deliver/README.md)
