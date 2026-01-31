# Framework Architecture

This document provides detailed technical architecture of the Devviaai Unified Workflow Framework.

## Table of Contents

- [High-Level Architecture](#high-level-architecture)
- [Stage Architecture](#stage-architecture)
- [Data Flow](#data-flow)
- [Integration Points](#integration-points)
- [Technology Stack](#technology-stack)
- [Design Patterns](#design-patterns)

## High-Level Architecture

### Conceptual Model

```
┌─────────────────────────────────────────────────────────────────────┐
│                         DEVVIAAI FRAMEWORK                          │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                        INPUT LAYER                           │  │
│  │  • Requirements Documents                                    │  │
│  │  • Business Constraints                                      │  │
│  │  • Technical Constraints                                     │  │
│  │  • Stakeholder Input                                         │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                │                                   │
│                                ▼                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                   STAGE 1: DISCOVER & DESIGN                 │  │
│  │  ┌─────────────┐  ┌──────────────┐  ┌─────────────────┐     │  │
│  │  │ Requirements│  │ Architecture │  │ Resource        │     │  │
│  │  │ Analysis    │  │ Design       │  │ Planning        │     │  │
│  │  └─────────────┘  └──────────────┘  └─────────────────┘     │  │
│  │  Output: Technical Blueprint Document (TBD)                  │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                │                                   │
│                                ▼                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                   STAGE 2: DEVELOP & BUILD                   │  │
│  │  ┌─────────────┐  ┌──────────────┐  ┌─────────────────┐     │  │
│  │  │ Application │  │ Infrastructure│  │ Testing         │     │  │
│  │  │ Code        │  │ as Code      │  │ Implementation  │     │  │
│  │  └─────────────┘  └──────────────┘  └─────────────────┘     │  │
│  │  Output: Source Code, IaC Modules, Tests                     │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                │                                   │
│                                ▼                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                  STAGE 3: DEPLOY & DELIVER                   │  │
│  │  ┌─────────────┐  ┌──────────────┐  ┌─────────────────┐     │  │
│  │  │ CI Pipeline │  │ CD Pipeline  │  │ Environment     │     │  │
│  │  │ Setup       │  │ Setup        │  │ Provisioning    │     │  │
│  │  └─────────────┘  └──────────────┘  └─────────────────┘     │  │
│  │  Output: Automated Pipelines, Running Environments           │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                │                                   │
│                                ▼                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                 STAGE 4: VALIDATE & OPERATE                  │  │
│  │  ┌─────────────┐  ┌──────────────┐  ┌─────────────────┐     │  │
│  │  │ Validation  │  │ Monitoring   │  │ Operations      │     │  │
│  │  │ & Testing   │  │ Setup        │  │ Documentation   │     │  │
│  │  └─────────────┘  └──────────────┘  └─────────────────┘     │  │
│  │  Output: Reports, Dashboards, Runbooks                       │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                │                                   │
│                                ▼                                   │
│  ┌─────────────────────────────────────────────────────────────┐  │
│  │                       OUTPUT LAYER                           │  │
│  │  • Production System                                         │  │
│  │  • Operational Documentation                                 │  │
│  │  • Monitoring & Alerting                                     │  │
│  │  • Continuous Improvement Feedback                           │  │
│  └─────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## Stage Architecture

### Stage 1: Discover & Design - Detailed Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                    STAGE 1: DISCOVER & DESIGN                  │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  INPUT PROCESSING                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │
│  │ Requirements │  │ Architecture │  │ Constraints  │        │
│  │ Document     │  │ Guidelines   │  │ Document     │        │
│  └──────────────┘  └──────────────┘  └──────────────┘        │
│         │                 │                 │                 │
│         └─────────────────┴─────────────────┘                 │
│                           │                                    │
│                           ▼                                    │
│  ANALYSIS & DESIGN ACTIVITIES                                  │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ 1. Requirements Extraction & Classification             │  │
│  │    → Functional Requirements (FR-XXX)                   │  │
│  │    → Non-Functional Requirements (NFR-XXX)              │  │
│  │                                                          │  │
│  │ 2. Architecture Design                                  │  │
│  │    → C4 Context Diagram                                 │  │
│  │    → C4 Container Diagram                               │  │
│  │    → Technology Stack Selection                         │  │
│  │                                                          │  │
│  │ 3. Component Identification                             │  │
│  │    → Application Components (COMP-APP-XXX)              │  │
│  │    → Infrastructure Components (COMP-INF-XXX)           │  │
│  │    → Component Dependencies & Interfaces                │  │
│  │                                                          │  │
│  │ 4. Resource Planning                                    │  │
│  │    → Azure Service Selection (RES-AZ-XXX)               │  │
│  │    → Resource Sizing & Configuration                    │  │
│  │    → Cost Estimation                                    │  │
│  │                                                          │  │
│  │ 5. Security Design                                      │  │
│  │    → Authentication/Authorization (SEC-AUTH-XXX)        │  │
│  │    → Network Security (SEC-NET-XXX)                     │  │
│  │    → Data Protection (SEC-DATA-XXX)                     │  │
│  │                                                          │  │
│  │ 6. Task Decomposition                                   │  │
│  │    → Development Tasks (TASK-XXX)                       │  │
│  │    → Task Dependencies & Priorities                     │  │
│  │                                                          │  │
│  │ 7. Risk Assessment                                      │  │
│  │    → Risk Identification (RISK-XXX)                     │  │
│  │    → Impact & Likelihood Analysis                       │  │
│  │    → Mitigation Strategies                              │  │
│  └─────────────────────────────────────────────────────────┘  │
│                           │                                    │
│                           ▼                                    │
│  OUTPUT: Technical Blueprint Document (TBD)                    │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ • Requirements Registry                                 │  │
│  │ • Component Catalog                                     │  │
│  │ • Resource Inventory                                    │  │
│  │ • Security Design                                       │  │
│  │ • Task Backlog                                          │  │
│  │ • Risk Matrix                                           │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Stage 2: Develop & Build - Detailed Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                    STAGE 2: DEVELOP & BUILD                    │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  INPUT: TBD + Specific Task ID                                 │
│                                                                │
│  IMPLEMENTATION ACTIVITIES                                     │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ Application Layer                                       │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │ │ API/Services│  │ Business    │  │ Data Access │      │  │
│  │ │ Layer       │  │ Logic       │  │ Layer       │      │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  │                                                          │  │
│  │ Database Layer                                          │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │ │ Schema      │  │ Stored      │  │ Migrations  │      │  │
│  │ │ Design      │  │ Procedures  │  │ Scripts     │      │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  │                                                          │  │
│  │ Infrastructure Layer (Terraform/HCL)                    │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │ │ Compute     │  │ Networking  │  │ Storage/    │      │  │
│  │ │ Resources   │  │ Resources   │  │ Data        │      │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  │                                                          │  │
│  │ Testing Layer                                           │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │ │ Unit Tests  │  │ Integration │  │ IaC Tests   │      │  │
│  │ │             │  │ Tests       │  │             │      │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  └─────────────────────────────────────────────────────────┘  │
│                           │                                    │
│                           ▼                                    │
│  OUTPUT: Artifacts                                             │
│  • Application Source Code                                     │
│  • Infrastructure Modules                                      │
│  • Database Scripts                                            │
│  • Test Suites                                                 │
│  • Component Documentation                                     │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Stage 3: Deploy & Deliver - Detailed Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                   STAGE 3: DEPLOY & DELIVER                    │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  CI PIPELINE (Continuous Integration)                          │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ Trigger: Code Push/PR                                   │  │
│  │    ↓                                                     │  │
│  │ Build → Test → Security Scan → Quality Gates            │  │
│  │    ↓                                                     │  │
│  │ Publish Artifacts                                       │  │
│  └─────────────────────────────────────────────────────────┘  │
│                           │                                    │
│                           ▼                                    │
│  CD PIPELINE (Continuous Deployment)                           │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ Artifact Retrieval                                      │  │
│  │    ↓                                                     │  │
│  │ Deploy to DEV                                           │  │
│  │    ↓                                                     │  │
│  │ Integration Tests                                       │  │
│  │    ↓                                                     │  │
│  │ Deploy to STAGING (Manual Approval)                     │  │
│  │    ↓                                                     │  │
│  │ E2E Tests, Performance Tests                            │  │
│  │    ↓                                                     │  │
│  │ Deploy to PRODUCTION (Manual Approval)                  │  │
│  │    ↓                                                     │  │
│  │ Smoke Tests, Health Checks                              │  │
│  └─────────────────────────────────────────────────────────┘  │
│                           │                                    │
│                           ▼                                    │
│  ENVIRONMENTS                                                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                    │
│  │   DEV    │  │ STAGING  │  │   PROD   │                    │
│  │          │  │          │  │          │                    │
│  │ • Latest │  │ • Pre-   │  │ • Stable │                    │
│  │   Code   │  │   Prod   │  │   Release│                    │
│  │ • Rapid  │  │ • Full   │  │ • HA     │                    │
│  │   Iter.  │  │   Testing│  │ • Monitor│                    │
│  └──────────┘  └──────────┘  └──────────┘                    │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Stage 4: Validate & Operate - Detailed Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                  STAGE 4: VALIDATE & OPERATE                   │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  VALIDATION LAYER                                              │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ • End-to-End Testing                                    │  │
│  │ • Performance Testing                                   │  │
│  │ • Security Testing                                      │  │
│  │ • Compliance Validation                                 │  │
│  └─────────────────────────────────────────────────────────┘  │
│                           │                                    │
│                           ▼                                    │
│  OBSERVABILITY STACK                                           │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ Metrics Layer (Azure Monitor)                           │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │ │ Application │  │ Infrastructure │ Business    │      │  │
│  │ │ Metrics     │  │ Metrics      │  │ Metrics     │      │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  │                                                          │  │
│  │ Logging Layer (Log Analytics)                           │  │
│  │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │  │
│  │ │ Application │  │ System      │  │ Audit       │      │  │
│  │ │ Logs        │  │ Logs        │  │ Logs        │      │  │
│  │ └─────────────┘  └─────────────┘  └─────────────┘      │  │
│  │                                                          │  │
│  │ Tracing Layer (Application Insights)                    │  │
│  │ ┌─────────────┐  ┌─────────────┐                        │  │
│  │ │ Distributed │  │ Performance │                        │  │
│  │ │ Tracing     │  │ Profiling   │                        │  │
│  │ └─────────────┘  └─────────────┘                        │  │
│  └─────────────────────────────────────────────────────────┘  │
│                           │                                    │
│                           ▼                                    │
│  OPERATIONAL CAPABILITIES                                      │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ • Dashboards (Azure Portal, Grafana)                    │  │
│  │ • Alerts (Email, SMS, Teams, PagerDuty)                 │  │
│  │ • Runbooks (Deployment, Rollback, Incident Response)    │  │
│  │ • SLA Monitoring & Reporting                            │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

## Data Flow

### Artifact Flow Through Stages

```
Requirements Doc
      │
      ▼
┌─────────────┐
│   Stage 1   │ → TBD (Technical Blueprint Document)
└─────────────┘
      │
      ▼
┌─────────────┐     ┌──────────────────────────────┐
│   Stage 2   │ → │ • Application Code          │
└─────────────┘     │ • Infrastructure Modules    │
      │             │ • Tests                     │
      │             │ • Documentation             │
      ▼             └──────────────────────────────┘
┌─────────────┐     ┌──────────────────────────────┐
│   Stage 3   │ →  │ • CI/CD Pipelines           │
└─────────────┘     │ • Environment Configs       │
      │             │ • Deployed Environments     │
      │             └──────────────────────────────┘
      ▼
┌─────────────┐     ┌──────────────────────────────┐
│   Stage 4   │ →  │ • Validation Reports        │
└─────────────┘     │ • Monitoring Configuration  │
                    │ • Runbooks                  │
                    │ • Operational Dashboards    │
                    └──────────────────────────────┘
```

### Traceability Flow

```
FR-001: User Authentication Requirement
    │
    ├─→ COMP-APP-001: Authentication Service
    │       │
    │       ├─→ TASK-001: Implement Auth API
    │       │       │
    │       │       └─→ src/auth/api.py (Stage 2)
    │       │               │
    │       │               └─→ CI Pipeline Test (Stage 3)
    │       │                       │
    │       │                       └─→ E2E Validation (Stage 4)
    │       │
    │       └─→ RES-AZ-001: App Service
    │               │
    │               └─→ terraform/app-service.tf (Stage 2)
    │                       │
    │                       └─→ CD Pipeline Deploy (Stage 3)
    │                               │
    │                               └─→ Health Monitoring (Stage 4)
    │
    └─→ SEC-AUTH-001: Azure AD Integration
            │
            └─→ TASK-002: Configure Azure AD
                    │
                    └─→ terraform/aad.tf (Stage 2)
```

## Integration Points

### AI Assistant Integration

```
┌───────────────────┐
│   Developer       │
│   (Human)         │
└─────────┬─────────┘
          │
          │ Uses framework prompts
          │
          ▼
┌───────────────────┐
│   AI Assistant    │
│ (Copilot/GPT/etc) │
└─────────┬─────────┘
          │
          │ Generates artifacts
          │
          ▼
┌───────────────────┐
│   Framework       │
│   Artifacts       │
│ (TBD, Code, etc)  │
└───────────────────┘
```

### External Tools Integration

```
Framework Artifacts
        │
        ├─→ GitHub/Azure DevOps (Version Control)
        │
        ├─→ GitHub Actions/Azure Pipelines (CI/CD)
        │
        ├─→ Terraform Cloud/Azure (Resource Provisioning)
        │
        ├─→ Azure Monitor (Observability)
        │
        └─→ Azure Key Vault (Secret Management)
```

## Technology Stack

### Development Stack

| Layer | Technologies |
|-------|-------------|
| Backend | Python, Node.js, .NET, PowerShell |
| Database | PostgreSQL (PLpgSQL), Azure SQL |
| IaC | Terraform/HCL, Bicep (alternative) |
| Testing | pytest, Jest, xUnit, Terratest |

### Platform Stack

| Component | Technologies |
|-----------|-------------|
| Compute | Azure App Service, Azure Functions, AKS |
| Storage | Azure Blob Storage, Azure Files |
| Database | Azure Database for PostgreSQL, Azure SQL |
| Networking | Azure VNet, Application Gateway, Front Door |
| Security | Azure AD, Key Vault, Network Security Groups |

### DevOps Stack

| Function | Technologies |
|----------|-------------|
| Source Control | Git, GitHub, Azure Repos |
| CI/CD | GitHub Actions, Azure Pipelines |
| Containers | Docker, Azure Container Registry |
| Orchestration | Kubernetes (AKS), Container Apps |
| Monitoring | Azure Monitor, Application Insights |

## Design Patterns

### Context Chain Pattern

Each stage explicitly declares:
- **Input Contract**: What it expects from previous stage
- **Processing**: What transformations it performs
- **Output Contract**: What it provides to next stage

### Traceability Pattern

Every artifact includes:
- **Source Reference**: Which TBD item it implements
- **Identifier**: Unique ID for tracking
- **Dependencies**: Links to related items

### Template Pattern

All deliverables follow structured templates:
- **Consistency**: Same structure across projects
- **Completeness**: All required sections covered
- **AI-Friendly**: Easy for AI to populate

### Modular Composition Pattern

Components are:
- **Self-Contained**: Can be developed independently
- **Well-Interfaced**: Clear contracts with dependencies
- **Reusable**: Can be used across projects

## Next Steps

- **Implementation**: See stage-specific documentation
- **Examples**: Review the [Sample Project](../../examples/sample-project/README.md)
- **Customization**: Read the [Customization Guide](../guides/CUSTOMIZATION.md)

---

[Back to Documentation Index](../README.md) | [Framework Overview](README.md) | [Glossary](GLOSSARY.md)
