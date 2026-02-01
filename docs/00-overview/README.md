# Framework Overview

## Introduction

The Devviaai Unified Workflow Framework is a comprehensive, systematic approach to building and deploying cloud-based solutions with full AI assistance. It bridges the gap between requirements and production-ready systems through a structured 4-stage process that ensures quality, traceability, and operational excellence.

## Core Philosophy

### Design Principles

1. **Context Continuity**: Every stage builds on the previous one with explicit input/output contracts
2. **Traceability First**: All artifacts link back to requirements and forward to implementation
3. **AI-Optimized**: Prompts and templates designed for maximum AI assistant effectiveness
4. **Cloud-Native**: Built for modern cloud platforms with Azure as the primary target
5. **Standards-Based**: Leverages industry best practices (Arc42, C4, Well-Architected Framework)
6. **Practical Over Perfect**: Focus on delivering working solutions over theoretical perfection

### The Four-Stage Model

The framework divides the software delivery lifecycle into four distinct, interconnected stages:

```
┌────────────────────────────────────────────────────────────────┐
│                     WORKFLOW STAGES                            │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  STAGE 1: DISCOVER & DESIGN                                    │
│  ┌──────────────────────────────────────────────────────┐     │
│  │ Role: Solution Architect                             │     │
│  │ Activity: Requirements analysis, architecture design │     │
│  │ Output: Technical Blueprint Document (TBD)           │     │
│  └──────────────────────────────────────────────────────┘     │
│                            │                                   │
│                            ▼                                   │
│  STAGE 2: DEVELOP & BUILD                                      │
│  ┌──────────────────────────────────────────────────────┐     │
│  │ Role: Full-Stack Engineer                            │     │
│  │ Activity: Code implementation, IaC development       │     │
│  │ Output: Application code, Infrastructure code, Tests │     │
│  └──────────────────────────────────────────────────────┘     │
│                            │                                   │
│                            ▼                                   │
│  STAGE 3: DEPLOY & DELIVER                                     │
│  ┌──────────────────────────────────────────────────────┐     │
│  │ Role: Platform Engineer                              │     │
│  │ Activity: CI/CD setup, environment provisioning      │     │
│  │ Output: Pipelines, deployed environments             │     │
│  └──────────────────────────────────────────────────────┘     │
│                            │                                   │
│                            ▼                                   │
│  STAGE 4: VALIDATE & OPERATE                                   │
│  ┌──────────────────────────────────────────────────────┐     │
│  │ Role: Quality & Reliability Engineer                 │     │
│  │ Activity: Validation, monitoring, documentation      │     │
│  │ Output: Reports, dashboards, runbooks                │     │
│  └──────────────────────────────────────────────────────┘     │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

## Stage Details

### Stage 1: Discover & Design

**Purpose**: Transform requirements into a comprehensive technical design

**Key Activities**:
- Requirements analysis and documentation
- Architecture design using C4 model
- Component identification and cataloging
- Resource planning and sizing
- Security and compliance design
- Risk assessment and mitigation planning

**Primary Artifact**: Technical Blueprint Document (TBD)

**TBD Sections**:
- Requirements Registry (FR-XXX, NFR-XXX)
- Component Catalog (COMP-XXX)
- Resource Inventory (RES-XXX)
- Security Design (SEC-XXX)
- Task Backlog (TASK-XXX)
- Risk Matrix (RISK-XXX)

### Stage 2: Develop & Build

**Purpose**: Implement the designed solution as code and infrastructure

**Key Activities**:
- Application code development
- Infrastructure as Code (Terraform/HCL) creation
- Unit and integration test implementation
- Database schema and stored procedures
- API and service implementation
- Documentation updates

**Primary Artifacts**:
- Application source code
- Infrastructure modules
- Test suites
- Component documentation

**Technology Focus**:
- **Database**: PLpgSQL (PostgreSQL)
- **Scripting**: PowerShell, Bash
- **Infrastructure**: Terraform/HCL
- **Application**: Python, Node.js, .NET

### Stage 3: Deploy & Deliver

**Purpose**: Automate deployment and establish environments

**Key Activities**:
- CI/CD pipeline creation (GitHub Actions or Azure DevOps)
- Environment provisioning (dev, staging, prod)
- Deployment automation
- Secret management setup
- Network and security configuration
- Environment validation

**Primary Artifacts**:
- CI pipelines (build, test, scan)
- CD pipelines (deploy, promote)
- Environment configurations
- Deployment documentation

**Environments**:
- **Development**: Rapid iteration, latest code
- **Staging**: Pre-production validation
- **Production**: Live system, stable releases

### Stage 4: Validate & Operate

**Purpose**: Ensure quality and establish operational practices

**Key Activities**:
- End-to-end testing
- Performance and load testing
- Security validation
- Monitoring and alerting setup
- Dashboard creation
- Runbook documentation
- Incident response planning

**Primary Artifacts**:
- Validation reports
- Monitoring configurations
- Dashboards
- Runbooks and playbooks
- Operational documentation

## The Technical Blueprint Document (TBD)

The TBD is the central artifact that flows through all stages. It serves as:

- **Single Source of Truth**: All design decisions documented
- **Traceability Matrix**: Links requirements to implementation
- **Communication Tool**: Bridges stakeholders and implementers
- **Change Log**: Tracks evolution of the solution

### TBD Structure

```
Technical Blueprint Document (TBD)
├── 1. Requirements Registry
│   ├── Functional Requirements (FR-001, FR-002, ...)
│   └── Non-Functional Requirements (NFR-001, NFR-002, ...)
├── 2. Component Catalog
│   ├── Application Components (COMP-APP-001, ...)
│   └── Infrastructure Components (COMP-INF-001, ...)
├── 3. Resource Inventory
│   ├── Azure Resources (RES-AZ-001, ...)
│   └── External Resources (RES-EXT-001, ...)
├── 4. Security Design
│   ├── Authentication/Authorization (SEC-AUTH-001, ...)
│   ├── Network Security (SEC-NET-001, ...)
│   └── Data Protection (SEC-DATA-001, ...)
├── 5. Task Backlog
│   └── Development Tasks (TASK-001, TASK-002, ...)
└── 6. Risk Matrix
    └── Identified Risks (RISK-001, RISK-002, ...)
```

## Traceability System

The framework uses structured identifiers to maintain traceability:

### ID Prefixes

| Prefix | Meaning | Example | Usage |
|--------|---------|---------|-------|
| FR | Functional Requirement | FR-001 | User authentication requirement |
| NFR | Non-Functional Requirement | NFR-001 | 99.9% availability requirement |
| COMP | Component | COMP-APP-001 | User service component |
| RES | Resource | RES-AZ-001 | Azure App Service instance |
| SEC | Security Item | SEC-AUTH-001 | Azure AD integration |
| TASK | Development Task | TASK-001 | Implement login API |
| RISK | Risk Item | RISK-001 | Database performance risk |

### Traceability Example

```
FR-001 (User Login Requirement)
  ├── COMP-APP-001 (Authentication Service)
  │   ├── TASK-001 (Implement Auth API)
  │   └── RES-AZ-001 (App Service for Auth)
  ├── SEC-AUTH-001 (Azure AD Integration)
  │   └── TASK-002 (Configure Azure AD)
  └── RISK-001 (Auth Service Availability)
      └── TASK-003 (Implement retry logic)
```

## Integration with Industry Standards

### Arc42

- Section 1-4: Covered in Stage 1 (TBD)
- Section 5-7: Covered in Stage 2 (Implementation)
- Section 8-9: Covered in Stage 3 (Deployment)
- Section 10-11: Covered in Stage 4 (Operations)

### C4 Model

- **Context Diagram**: Created in Stage 1
- **Container Diagram**: Created in Stage 1
- **Component Diagram**: Refined in Stage 2
- **Code Diagram**: Generated in Stage 2

### Azure Well-Architected Framework

- **Reliability**: Addressed in NFRs and Stage 4
- **Security**: SEC-XXX items across all stages
- **Cost Optimization**: Resource sizing in Stage 1
- **Operational Excellence**: Stage 4 focus
- **Performance Efficiency**: NFRs and validation

## AI Assistant Integration

### How AI Assistants Are Used

1. **Stage Prompts**: Each stage has a master prompt designed for AI tools
2. **Template Population**: AI helps fill out templates with project-specific content
3. **Code Generation**: AI generates boilerplate and implementation code
4. **Review and Refinement**: AI assists in reviewing and improving artifacts
5. **Documentation**: AI helps maintain comprehensive documentation

### Best Practices for AI Usage

- **Be Specific**: Provide clear context and requirements in prompts
- **Iterate**: Refine AI outputs through multiple interactions
- **Validate**: Always review AI-generated content critically
- **Customize**: Adapt AI suggestions to your specific needs
- **Learn**: Use AI outputs to understand patterns and best practices

## Workflow Flexibility

While the framework defines a sequential flow, it supports:

- **Iterative Refinement**: Return to earlier stages as needed
- **Parallel Work**: Multiple tasks in Stage 2 can proceed simultaneously
- **Incremental Delivery**: Deploy subsets of functionality progressively
- **Customization**: Adapt stages to project-specific needs

## Success Metrics

Track framework effectiveness through:

- **Traceability**: % of code linked to requirements
- **Documentation Coverage**: % of components documented
- **Deployment Frequency**: How often you deploy
- **Change Fail Rate**: % of deployments requiring rollback
- **Time to Production**: Days from requirements to deployment
- **Incident Response Time**: MTTR for production issues

## Next Steps

- **New Users**: Start with the [Quick Start Guide](../guides/QUICK-START.md)
- **Detailed Learning**: Read the [Step-by-Step Guide](../guides/STEP-BY-STEP.md)
- **Stage 1**: Begin with [Discover & Design](../01-stage-discover-design/README.md)
- **Examples**: Review the [Sample Project](../../examples/sample-project/README.md)

---

[Back to Documentation Index](../README.md) | [Architecture Details](ARCHITECTURE.md) | [Glossary](GLOSSARY.md)
