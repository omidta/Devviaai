# Stage 1: Discover & Design

## Overview

Stage 1 is where requirements transform into a comprehensive technical design. Acting as a **Solution Architect**, you'll analyze requirements, design the system architecture, and produce the Technical Blueprint Document (TBD) that guides all subsequent stages.

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
Convert business and technical requirements into a detailed, implementable technical design that serves as the single source of truth for the entire project.

### Key Objectives

1. **Requirements Clarity**: Extract, classify, and document all functional and non-functional requirements
2. **Architecture Definition**: Design the system using industry-standard models (C4, Arc42)
3. **Component Specification**: Identify and specify all application and infrastructure components
4. **Resource Planning**: Define all Azure resources with sizing and configuration
5. **Security Design**: Establish comprehensive security architecture
6. **Task Decomposition**: Break down work into manageable, prioritized tasks
7. **Risk Management**: Identify and plan mitigation for project risks

## Role Definition

### Solution Architect Responsibilities

As a Solution Architect in Stage 1, you are responsible for:

- **Strategic Thinking**: Understanding business goals and constraints
- **Technical Design**: Creating scalable, maintainable architecture
- **Technology Selection**: Choosing appropriate Azure services and technologies
- **Documentation**: Producing clear, comprehensive design documentation
- **Risk Assessment**: Identifying potential issues early
- **Stakeholder Communication**: Translating technical decisions for non-technical audiences

### Skills Required

- Cloud architecture (Azure preferred)
- System design patterns
- Security best practices
- Cost optimization
- Requirements engineering
- Technical documentation

## Input Requirements

### Required Inputs

1. **Requirements Document**
   - Business objectives
   - User stories or use cases
   - Functional requirements
   - Non-functional requirements (performance, security, etc.)

2. **Architecture Guidelines** (if available)
   - Organization architecture standards
   - Technology preferences
   - Security requirements
   - Compliance requirements

3. **Constraints Document**
   - Budget constraints
   - Timeline constraints
   - Technology constraints
   - Regulatory constraints

### Optional Inputs

- Existing system documentation (for migrations)
- Competitor analysis
- User research findings
- Proof of concept results

## Output Specifications

### Primary Output: Technical Blueprint Document (TBD)

The TBD is a comprehensive document with six major sections:

#### 1. Requirements Registry

**Format**: Structured list with unique IDs

**Functional Requirements (FR-XXX)**:
- **FR-001**: User authentication via Azure AD
- **FR-002**: CRUD operations for entities
- **FR-003**: Export data to CSV

**Non-Functional Requirements (NFR-XXX)**:
- **NFR-001**: 99.9% availability SLA
- **NFR-002**: Response time < 200ms for API calls
- **NFR-003**: Support 10,000 concurrent users

#### 2. Component Catalog

**Application Components (COMP-APP-XXX)**:
```
COMP-APP-001: Authentication Service
  Type: REST API
  Technology: Node.js + Express
  Dependencies: COMP-APP-002 (User Service)
  Interfaces: REST API, Azure AD
  
COMP-APP-002: User Service
  Type: Backend Service
  Technology: Python + FastAPI
  Dependencies: RES-AZ-002 (PostgreSQL)
  Interfaces: REST API
```

**Infrastructure Components (COMP-INF-XXX)**:
```
COMP-INF-001: API Gateway
  Type: Azure API Management
  Purpose: API routing, rate limiting, monitoring
  Dependencies: COMP-APP-001, COMP-APP-002
```

#### 3. Resource Inventory

```
RES-AZ-001: Web App Service
  Service: Azure App Service
  SKU: P1v2 (2 cores, 3.5 GB RAM)
  Purpose: Host Node.js API
  Estimated Cost: $146/month
  
RES-AZ-002: PostgreSQL Database
  Service: Azure Database for PostgreSQL
  SKU: General Purpose, 2 vCores
  Storage: 100 GB
  Purpose: Application data store
  Estimated Cost: $164/month
```

#### 4. Security Design

```
SEC-AUTH-001: Azure AD Integration
  Type: Authentication
  Implementation: OAuth 2.0 + OpenID Connect
  Components: All APIs
  
SEC-NET-001: Network Isolation
  Type: Network Security
  Implementation: VNet + NSGs
  Components: All Azure resources
  
SEC-DATA-001: Data Encryption
  Type: Data Protection
  Implementation: TLS in transit, AES-256 at rest
  Components: All data stores
```

#### 5. Task Backlog

```
TASK-001: Implement Authentication API
  Priority: High
  Dependencies: None
  Implements: FR-001, COMP-APP-001
  Estimated Effort: 5 days
  
TASK-002: Set up PostgreSQL Schema
  Priority: High
  Dependencies: None
  Implements: COMP-APP-002, RES-AZ-002
  Estimated Effort: 3 days
```

#### 6. Risk Matrix

```
RISK-001: Database Performance Bottleneck
  Likelihood: Medium
  Impact: High
  Mitigation: Implement caching, optimize queries, monitor performance
  Owner: Backend team
  
RISK-002: Azure AD Integration Complexity
  Likelihood: Low
  Impact: Medium
  Mitigation: Early POC, detailed documentation, expert consultation
  Owner: Security team
```

### Supporting Outputs

- **C4 Context Diagram**: System in its environment
- **C4 Container Diagram**: High-level component view
- **Technology Stack Document**: Chosen technologies with rationale
- **Architecture Decision Records (ADRs)**: Key decisions documented

## Process Flow

### Step-by-Step Process

```
1. Requirements Analysis
   │
   ├─→ Read all input documents
   ├─→ Extract functional requirements (FR-XXX)
   ├─→ Extract non-functional requirements (NFR-XXX)
   └─→ Clarify ambiguities with stakeholders
   │
   ▼
2. Architecture Design
   │
   ├─→ Create C4 Context Diagram
   ├─→ Create C4 Container Diagram
   ├─→ Select Azure services
   └─→ Define technology stack
   │
   ▼
3. Component Design
   │
   ├─→ Identify application components
   ├─→ Define component interfaces
   ├─→ Map component dependencies
   └─→ Assign technologies to components
   │
   ▼
4. Resource Planning
   │
   ├─→ Map components to Azure resources
   ├─→ Size resources based on NFRs
   ├─→ Estimate costs
   └─→ Plan for scalability
   │
   ▼
5. Security Design
   │
   ├─→ Design authentication/authorization
   ├─→ Plan network security
   ├─→ Define data protection measures
   └─→ Address compliance requirements
   │
   ▼
6. Task Planning
   │
   ├─→ Break down components into tasks
   ├─→ Identify dependencies
   ├─→ Estimate effort
   └─→ Prioritize tasks
   │
   ▼
7. Risk Assessment
   │
   ├─→ Identify technical risks
   ├─→ Assess likelihood and impact
   ├─→ Define mitigation strategies
   └─→ Assign risk owners
   │
   ▼
8. TBD Assembly & Review
   │
   ├─→ Compile all sections
   ├─→ Cross-reference IDs
   ├─→ Review for completeness
   └─→ Get stakeholder approval
```

## Integration with Other Stages

### Provides to Stage 2 (Develop & Build)

- **TBD Document**: Complete technical specification
- **Task IDs**: Specific work items to implement
- **Component Specifications**: What to build and how
- **Technology Choices**: Languages, frameworks, services to use

### Provides to Stage 3 (Deploy & Deliver)

- **Resource Inventory**: What infrastructure to provision
- **Environment Requirements**: Dev, staging, prod configurations
- **Security Requirements**: Network, secrets, access controls

### Provides to Stage 4 (Validate & Operate)

- **NFRs**: Success criteria for validation
- **Monitoring Requirements**: What to observe
- **SLAs**: Service level agreements to track

### Receives Feedback From

- **Stage 2**: Feasibility issues, technical constraints discovered during implementation
- **Stage 3**: Deployment challenges, resource constraints
- **Stage 4**: Operational issues, performance problems

## Quality Checklist

### Before Completing Stage 1

- [ ] All requirements have unique IDs (FR-XXX, NFR-XXX)
- [ ] Every component is specified with technology and dependencies
- [ ] All Azure resources are listed with SKUs and estimated costs
- [ ] Security is addressed for authentication, network, and data
- [ ] All tasks reference at least one TBD item (FR, COMP, RES)
- [ ] Risks are identified with mitigation strategies
- [ ] C4 diagrams are created (Context and Container minimum)
- [ ] Technology stack is documented with rationale
- [ ] Dependencies between components are mapped
- [ ] TBD is reviewed by technical stakeholders
- [ ] Traceability matrix is complete (Requirements → Components → Resources → Tasks)

### Quality Attributes

- **Completeness**: All aspects of the system are covered
- **Clarity**: Non-technical stakeholders can understand high-level design
- **Traceability**: Every implementation item traces back to a requirement
- **Feasibility**: Design is implementable within constraints
- **Maintainability**: Design supports future changes and growth

## Best Practices

### Do's ✅

1. **Start with Requirements**: Don't jump to solutions; understand needs first
2. **Use Standard Models**: Leverage C4, Arc42 for consistency
3. **Think Cloud-Native**: Design for scalability, resilience, observability
4. **Document Decisions**: Use ADRs for significant choices
5. **Consider Cost**: Balance functionality with budget constraints
6. **Plan for Security**: Security by design, not as an afterthought
7. **Be Specific**: Provide enough detail for implementation
8. **Maintain Traceability**: Every item should link to requirements
9. **Validate Early**: Review with stakeholders before moving forward
10. **Keep It Updated**: TBD is a living document throughout the project

### Don'ts ❌

1. **Don't Over-Design**: Avoid unnecessary complexity
2. **Don't Ignore NFRs**: Performance, security, availability matter
3. **Don't Skip Risk Assessment**: Identify problems early
4. **Don't Assume**: Document assumptions explicitly
5. **Don't Design in Isolation**: Involve stakeholders and implementers
6. **Don't Forget Operations**: Consider monitoring, maintenance from the start
7. **Don't Lock In**: Allow flexibility for changes during implementation
8. **Don't Duplicate**: Reuse existing components and patterns
9. **Don't Ignore Costs**: Cloud resources can be expensive
10. **Don't Skimp on Documentation**: Future you (and your team) will thank you

## Getting Started

### Using This Stage

1. **Review the Master Prompt**: See [PROMPT.md](PROMPT.md) for AI-assisted workflow
2. **Use the TBD Template**: See [TEMPLATE-TBD.md](TEMPLATE-TBD.md) for structure
3. **Follow the Process**: Work through each step systematically
4. **Leverage AI**: Use the prompt with your AI assistant for faster results
5. **Review and Refine**: Iterate until the TBD is complete and approved

### Example Usage

```bash
# 1. Gather your inputs
# - requirements.md
# - constraints.md
# - architecture-guidelines.md

# 2. Copy the master prompt from PROMPT.md

# 3. Customize the prompt with your project details

# 4. Use with your AI assistant (GitHub Copilot, ChatGPT, Claude)

# 5. Review and refine the generated TBD

# 6. Save as technical-blueprint.md

# 7. Proceed to Stage 2
```

## Resources

- **[Master Prompt](PROMPT.md)**: AI-ready prompt for this stage
- **[TBD Template](TEMPLATE-TBD.md)**: Structure for the Technical Blueprint Document
- **[Framework Overview](../00-overview/README.md)**: Understand the full framework
- **[Example Project](../../examples/sample-project/)**: See a complete TBD example

---

[Back to Documentation Index](../README.md) | [Stage 1 Prompt](PROMPT.md) | [Next: Stage 2](../02-stage-develop-build/README.md)
