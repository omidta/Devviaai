# Stage 1: Master Prompt - Discover & Design

## Instructions for Use

1. Copy this entire prompt
2. Replace all `[PLACEHOLDER]` values with your project-specific information
3. Paste into your AI assistant (GitHub Copilot Chat, ChatGPT, Claude, etc.)
4. Review and refine the generated Technical Blueprint Document (TBD)
5. Save the output using the [TBD Template](TEMPLATE-TBD.md)

---

## Master Prompt

```
You are an expert Solution Architect specializing in Azure cloud solutions. Your task is to analyze the provided requirements and create a comprehensive Technical Blueprint Document (TBD) following the Devviaai framework.

### PROJECT CONTEXT

**Project Name**: [PROJECT_NAME]
**Project Description**: [BRIEF_DESCRIPTION]
**Target Platform**: Azure
**Timeline**: [PROJECT_TIMELINE]
**Budget Constraint**: [BUDGET_CONSTRAINT]

### INPUT DOCUMENTS

**Requirements**:
[PASTE_OR_DESCRIBE_REQUIREMENTS_HERE]

**Constraints**:
[PASTE_OR_DESCRIBE_CONSTRAINTS_HERE]

**Architecture Guidelines** (if any):
[PASTE_OR_DESCRIBE_ARCHITECTURE_GUIDELINES_HERE]

### YOUR TASK

Create a complete Technical Blueprint Document (TBD) with the following sections:

#### 1. REQUIREMENTS REGISTRY

Extract and document all requirements with unique IDs:

**Functional Requirements (FR-XXX)**:
- Identify all functional requirements from the input
- Assign sequential IDs starting with FR-001
- Write clear, testable requirement statements
- Include acceptance criteria where applicable

**Non-Functional Requirements (NFR-XXX)**:
- Identify performance requirements (response time, throughput)
- Identify availability/reliability requirements (SLA, uptime)
- Identify security requirements
- Identify compliance requirements
- Identify scalability requirements
- Assign sequential IDs starting with NFR-001

#### 2. COMPONENT CATALOG

Design the system architecture and identify all components:

**Application Components (COMP-APP-XXX)**:
For each application component, specify:
- Component ID and name
- Type (API, Service, Frontend, Backend, etc.)
- Technology/framework to be used
- Dependencies on other components
- Interfaces (REST API, message queue, database, etc.)
- Purpose and responsibilities

**Infrastructure Components (COMP-INF-XXX)**:
For each infrastructure component, specify:
- Component ID and name
- Type (Load Balancer, API Gateway, Cache, etc.)
- Azure service to be used
- Purpose and configuration overview

#### 3. RESOURCE INVENTORY

List all Azure resources needed:

For each resource (RES-AZ-XXX), provide:
- Resource ID and name
- Azure service type
- SKU/tier recommendation with justification
- Configuration details (cores, memory, storage, etc.)
- Purpose and which components use it
- Estimated monthly cost
- Scaling considerations

Include a cost summary table at the end.

#### 4. SECURITY DESIGN

Design comprehensive security architecture:

**Authentication & Authorization (SEC-AUTH-XXX)**:
- How will users authenticate?
- What authorization model will be used?
- Which components need authentication?

**Network Security (SEC-NET-XXX)**:
- Network isolation strategy (VNets, subnets)
- Network Security Groups (NSGs) configuration
- Private endpoints requirements
- Firewall rules

**Data Protection (SEC-DATA-XXX)**:
- Data encryption in transit (TLS/HTTPS)
- Data encryption at rest
- Key management strategy (Azure Key Vault)
- Backup and disaster recovery

**Compliance & Governance (SEC-COMP-XXX)**:
- Regulatory requirements (GDPR, HIPAA, etc.)
- Audit logging requirements
- Data residency requirements

#### 5. TASK BACKLOG

Break down the work into development tasks:

For each task (TASK-XXX), specify:
- Task ID and title
- Description of work to be done
- Which TBD items it implements (FR-XXX, COMP-XXX, RES-XXX references)
- Dependencies on other tasks
- Estimated effort (person-days)
- Priority (High, Medium, Low)
- Suggested assignment (Backend, Frontend, DevOps, Database, etc.)

Organize tasks by component or functional area.

#### 6. RISK MATRIX

Identify and assess project risks:

For each risk (RISK-XXX), provide:
- Risk ID and title
- Description of the risk
- Likelihood (Low, Medium, High)
- Impact (Low, Medium, High)
- Mitigation strategy
- Contingency plan
- Risk owner/responsible party

#### 7. ARCHITECTURE DIAGRAMS

Create the following diagrams using ASCII art or Mermaid syntax:

**C4 Context Diagram**:
Show the system in its environment with external actors and systems.

**C4 Container Diagram**:
Show the high-level technical building blocks (applications, databases, etc.).

#### 8. TECHNOLOGY STACK

Document the chosen technology stack:

**Backend**: [Languages, Frameworks]
**Frontend**: [Languages, Frameworks] (if applicable)
**Database**: [Database technology and rationale]
**Infrastructure as Code**: [Terraform/Bicep/ARM]
**CI/CD**: [GitHub Actions/Azure DevOps]
**Monitoring**: [Azure Monitor, Application Insights, etc.]

For each major technology choice, provide:
- Technology name
- Rationale for selection
- Alternatives considered

### OUTPUT FORMAT

Structure your response as a complete Technical Blueprint Document following the template in TEMPLATE-TBD.md. Use proper markdown formatting with:
- Clear section headers
- Tables for structured data
- Code blocks for examples
- Bullet points for lists
- Cross-references between related items (e.g., "implements FR-001, FR-003")

### QUALITY REQUIREMENTS

Ensure your TBD includes:
- ✅ Every requirement has a unique ID
- ✅ Every component references the requirements it fulfills
- ✅ Every resource references the components that use it
- ✅ Every task references the TBD items it implements
- ✅ Cost estimates for all Azure resources
- ✅ Complete traceability from requirements to tasks
- ✅ Security addressed at all levels
- ✅ NFRs are specific and measurable
- ✅ Architecture diagrams are clear and informative

### ADDITIONAL GUIDANCE

- Prefer Azure PaaS services over IaaS when possible
- Design for cloud-native patterns (stateless, scalable, resilient)
- Follow Azure Well-Architected Framework principles
- Consider cost optimization (right-sizing, reserved instances)
- Think about DevOps from the start (CI/CD, IaC)
- Include monitoring and observability requirements
- Plan for multiple environments (dev, staging, prod)
- Document assumptions clearly

Begin your analysis and produce the complete Technical Blueprint Document now.
```

---

## Example Usage

### Step 1: Prepare Your Inputs

Gather your project information:
- Requirements document
- Constraints (budget, timeline, technology)
- Any existing architecture guidelines

### Step 2: Fill in the Placeholders

Replace:
- `[PROJECT_NAME]`: "Customer Portal Modernization"
- `[BRIEF_DESCRIPTION]`: "Migrate legacy customer portal to Azure cloud with modern architecture"
- `[PROJECT_TIMELINE]`: "6 months"
- `[BUDGET_CONSTRAINT]`: "$2,000/month operational costs"
- `[PASTE_OR_DESCRIBE_REQUIREMENTS_HERE]`: Paste your full requirements
- etc.

### Step 3: Use with Your AI Assistant

**GitHub Copilot**:
- Open Copilot Chat
- Paste the customized prompt
- Review the generated TBD

**ChatGPT/Claude**:
- Start a new conversation
- Paste the customized prompt
- Download or copy the generated TBD

### Step 4: Refine and Iterate

After initial generation:
- Review each section for completeness
- Ask follow-up questions to clarify areas
- Request expansions on specific components
- Validate cost estimates
- Ensure all cross-references are correct

### Step 5: Save and Proceed

- Save the TBD as `technical-blueprint.md` in your project
- Share with stakeholders for review
- Use as input for Stage 2 (Develop & Build)

## Tips for Better Results

### Be Specific in Your Requirements

❌ **Vague**: "Users should be able to log in"

✅ **Specific**: "Users must authenticate using their corporate Azure AD credentials with multi-factor authentication. The system must support single sign-on (SSO) across all components."

### Provide Context

Include information about:
- Current state (if migrating)
- Business objectives
- User personas
- Expected load/usage patterns
- Regulatory requirements

### Iterate

The first output might not be perfect:
1. Generate initial TBD
2. Review and identify gaps
3. Ask clarifying questions
4. Request refinements
5. Repeat until satisfied

### Use Follow-Up Prompts

After the initial TBD:

```
"Can you expand on COMP-APP-001 with more details about the API endpoints?"

"Please provide more detailed cost estimates with different Azure regions."

"Add error handling and resilience requirements to the NFRs."

"Create a more detailed task breakdown for the authentication component."
```

## Validation Checklist

Before moving to Stage 2, verify:

- [ ] All functional requirements are captured with FR-XXX IDs
- [ ] All non-functional requirements are captured with NFR-XXX IDs
- [ ] Every component has a specification
- [ ] All Azure resources are listed with SKUs and costs
- [ ] Security is comprehensively addressed
- [ ] Tasks are broken down to appropriate granularity (3-10 days each)
- [ ] Dependencies between tasks are identified
- [ ] Risks are assessed with mitigation strategies
- [ ] Architecture diagrams are included
- [ ] Technology stack is documented with rationale
- [ ] Total estimated cost is within budget
- [ ] TBD is reviewed by stakeholders

## Common Pitfalls to Avoid

1. **Incomplete Requirements**: Don't skip NFRs; they're as important as functional requirements
2. **Vague Components**: Each component needs specific technology and interface definitions
3. **Missing Costs**: Always include cost estimates; budget surprises are bad surprises
4. **No Security**: Don't defer security design; include it from the start
5. **No Traceability**: Every task should reference TBD items (FR, COMP, RES)
6. **Over-Engineering**: Keep it as simple as possible while meeting requirements
7. **Under-Estimating**: Be realistic with effort estimates; add buffer for unknowns

## Next Steps

After completing the TBD:

1. **Review**: Get stakeholder approval
2. **Store**: Commit to version control
3. **Share**: Make accessible to the team
4. **Proceed**: Move to [Stage 2: Develop & Build](../02-stage-develop-build/README.md)

---

[Back to Stage 1 Overview](README.md) | [TBD Template](TEMPLATE-TBD.md) | [Documentation Index](../README.md)
