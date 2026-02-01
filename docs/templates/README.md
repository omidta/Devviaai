# Templates Index

This directory contains reusable templates for common documentation artifacts used throughout the Devviaai Framework.

## 📚 Available Templates

### Core Templates

#### [Architecture Decision Record (ADR)](adr-template.md)
**Purpose**: Document significant architecture decisions  
**When to Use**: Whenever making important technical decisions  
**Stage**: Primarily Stage 1, but can be used throughout

**Key Sections**:
- Context and problem statement
- Decision drivers
- Considered options
- Decision outcome
- Consequences

---

#### [Project Kickoff Checklist](project-kickoff-checklist.md)
**Purpose**: Comprehensive checklist for starting new projects  
**When to Use**: At project initiation  
**Stage**: Before Stage 1

**Key Sections**:
- Prerequisites verification
- Team setup
- Environment setup
- Initial planning
- Stage 1 preparation

---

### Stage-Specific Templates

#### Stage 1 Templates

- **[Technical Blueprint Document (TBD)](../01-stage-discover-design/TEMPLATE-TBD.md)**
  - Complete design document template
  - Requirements registry
  - Component catalog
  - Resource inventory
  - Security design
  - Task backlog
  - Risk matrix

#### Stage 2 Templates

- **[Component Template](../02-stage-develop-build/TEMPLATE-COMPONENT.md)**
  - Application component documentation
  - API specifications
  - Configuration details
  
- **[Infrastructure Template](../02-stage-develop-build/TEMPLATE-INFRASTRUCTURE.md)**
  - Terraform module structure
  - Resource definitions
  - Variables and outputs

#### Stage 3 Templates

- **[CI Pipeline Template](../03-stage-deploy-deliver/TEMPLATE-PIPELINE-CI.md)**
  - Continuous Integration workflow
  - Build and test automation
  
- **[CD Pipeline Template](../03-stage-deploy-deliver/TEMPLATE-PIPELINE-CD.md)**
  - Continuous Deployment workflow
  - Environment management

#### Stage 4 Templates

- **[Monitoring Template](../04-stage-validate-operate/TEMPLATE-MONITORING.md)**
  - Monitoring strategy
  - Alert definitions
  - Dashboard specifications
  
- **[Runbook Template](../04-stage-validate-operate/TEMPLATE-RUNBOOK.md)**
  - Operational procedures
  - Incident response
  - Health checks

---

## 🎯 How to Use These Templates

### Step 1: Choose the Right Template

Match your need to the appropriate template:
- **Decision making** → ADR
- **Starting project** → Project Kickoff Checklist  
- **Design phase** → TBD Template
- **Building component** → Component Template
- **Documenting operations** → Runbook Template

### Step 2: Copy the Template

```bash
# Copy template to your project
cp docs/templates/adr-template.md docs/adr/ADR-001-database-choice.md

# Or for TBD
cp docs/01-stage-discover-design/TEMPLATE-TBD.md docs/technical-blueprint.md
```

### Step 3: Fill in the Placeholders

Look for these markers and replace them:
- `[PROJECT_NAME]` - Your project name
- `[DATE]` - Current date
- `[AUTHOR_NAME]` - Your name
- `[DESCRIPTION]` - Specific descriptions
- `[XXX]` - Sequential numbering

### Step 4: Customize as Needed

Templates are starting points. Adapt them:
- Add sections relevant to your project
- Remove sections that don't apply
- Adjust level of detail
- Modify format to match your standards

### Step 5: Maintain and Version

- Keep templates in version control
- Update as requirements change
- Version control all filled templates
- Link templates to requirements

---

## 📋 Template Usage by Stage

### Before Stage 1: Project Initiation

**Use**:
- [Project Kickoff Checklist](project-kickoff-checklist.md)

**Purpose**: Ensure readiness to start the framework workflow

---

### Stage 1: Discover & Design

**Use**:
- [TBD Template](../01-stage-discover-design/TEMPLATE-TBD.md)
- [ADR Template](adr-template.md) (for key decisions)

**Purpose**: Document complete technical design

---

### Stage 2: Develop & Build

**Use**:
- [Component Template](../02-stage-develop-build/TEMPLATE-COMPONENT.md)
- [Infrastructure Template](../02-stage-develop-build/TEMPLATE-INFRASTRUCTURE.md)
- [ADR Template](adr-template.md) (for implementation decisions)

**Purpose**: Document components and infrastructure as code

---

### Stage 3: Deploy & Deliver

**Use**:
- [CI Pipeline Template](../03-stage-deploy-deliver/TEMPLATE-PIPELINE-CI.md)
- [CD Pipeline Template](../03-stage-deploy-deliver/TEMPLATE-PIPELINE-CD.md)

**Purpose**: Define deployment automation

---

### Stage 4: Validate & Operate

**Use**:
- [Monitoring Template](../04-stage-validate-operate/TEMPLATE-MONITORING.md)
- [Runbook Template](../04-stage-validate-operate/TEMPLATE-RUNBOOK.md)

**Purpose**: Establish operational procedures

---

## 🔍 Template Details

### Architecture Decision Record (ADR)

**File Naming Convention**: `ADR-NNN-short-title.md`

**Examples**:
- `ADR-001-database-selection.md`
- `ADR-002-authentication-strategy.md`
- `ADR-003-deployment-approach.md`

**Best Practices**:
- Write ADRs for significant decisions
- Be concise but complete
- Include alternatives considered
- Document consequences (positive and negative)
- Link to relevant TBD sections

**Status Values**:
- Proposed
- Accepted
- Deprecated
- Superseded

---

### Project Kickoff Checklist

**When to Use**: 
- New project start
- New phase of existing project
- Team onboarding

**Best Practices**:
- Complete checklist before Stage 1
- Verify all prerequisites
- Document any deviations
- Update based on learnings

---

## 🛠️ Creating Custom Templates

### When to Create Custom Templates

Create custom templates when:
- You need a document type multiple times
- Organization requires specific formats
- Domain requires specialized documentation
- Team requests standardization

### Custom Template Structure

```markdown
# [Template Name]

**Purpose**: [What this template is for]  
**When to Use**: [Situations for using this template]  
**Stage**: [Which stage(s) this applies to]

## Instructions

[How to fill out this template]

---

## Section 1: [Section Name]

[Description of what goes here]

[PLACEHOLDER for content]

## Section 2: [Section Name]

[Description of what goes here]

[PLACEHOLDER for content]

---

## Validation Checklist

- [ ] Checklist item 1
- [ ] Checklist item 2

---

[Footer with links]
```

### Example: Custom Security Review Template

```markdown
# Security Review Template

**Purpose**: Document security analysis of components  
**When to Use**: Before deploying any component to production  
**Stage**: Stage 4 (Validate & Operate)

## Instructions

Complete this security review for each component before production deployment.

---

## 1. Component Information

**Component ID**: [COMP-XXX]  
**Component Name**: [Name]  
**Reviewer**: [Name]  
**Review Date**: [Date]

## 2. Authentication & Authorization

**Authentication Method**: [Azure AD / API Key / etc.]

- [ ] MFA enabled for admin access
- [ ] Secure credential storage
- [ ] Token expiration configured

**Authorization Model**: [RBAC / ABAC / etc.]

- [ ] Least privilege principle applied
- [ ] Role definitions documented
- [ ] Access control tested

## 3. Data Protection

- [ ] Data encrypted at rest
- [ ] Data encrypted in transit
- [ ] PII identified and protected
- [ ] Data retention policy defined

## 4. Network Security

- [ ] Firewall rules configured
- [ ] Network segmentation applied
- [ ] DDoS protection enabled
- [ ] VPN for sensitive access

## 5. Security Testing

- [ ] OWASP top 10 tested
- [ ] Dependency vulnerabilities scanned
- [ ] Penetration testing completed
- [ ] Security scan passed

## 6. Compliance

- [ ] GDPR compliance verified
- [ ] Audit logging enabled
- [ ] Data residency requirements met
- [ ] Compliance documentation complete

## 7. Sign-Off

**Security Team Approval**: [Name, Date]  
**Findings**: [Any issues found]  
**Status**: Approved / Approved with conditions / Rejected

---

[Back to Templates Index](README.md)
```

---

## 📚 Template Library Organization

### Directory Structure

```
docs/
├── templates/
│   ├── README.md (this file)
│   ├── adr-template.md
│   ├── project-kickoff-checklist.md
│   └── custom/
│       ├── security-review-template.md
│       └── [your-custom-templates].md
├── 01-stage-discover-design/
│   └── TEMPLATE-TBD.md
├── 02-stage-develop-build/
│   ├── TEMPLATE-COMPONENT.md
│   └── TEMPLATE-INFRASTRUCTURE.md
├── 03-stage-deploy-deliver/
│   ├── TEMPLATE-PIPELINE-CI.md
│   └── TEMPLATE-PIPELINE-CD.md
└── 04-stage-validate-operate/
    ├── TEMPLATE-MONITORING.md
    └── TEMPLATE-RUNBOOK.md
```

### Naming Conventions

**Template Files**: `[purpose]-template.md`
- Use lowercase
- Hyphen-separated
- Suffix with `-template`

**Filled Templates**: `[ID]-[short-description].md`
- Use ID when applicable
- Short, descriptive name
- No `-template` suffix

---

## 🔄 Template Maintenance

### Version Control

All templates should be:
- Stored in version control
- Versioned with semantic versioning
- Updated based on feedback
- Reviewed periodically

### Template Updates

**When to Update**:
- Feedback from users
- Framework evolution
- Best practices change
- Tool updates

**How to Update**:
1. Create branch for template update
2. Make changes
3. Get review from team
4. Update changelog
5. Merge and tag version

### Template Changelog

Track changes to templates:

```markdown
# Template Changelog

## ADR Template

### v1.1.0 (2026-01-31)
- Added "Links" section for cross-references
- Enhanced "Consequences" section with positive/negative split

### v1.0.0 (2026-01-15)
- Initial version based on Michael Nygard's ADR format
```

---

## 📊 Template Usage Examples

### Example 1: Starting New Project

```bash
# 1. Use kickoff checklist
cp docs/templates/project-kickoff-checklist.md project-kickoff.md
# Fill out and verify all items

# 2. Create TBD from template
cp docs/01-stage-discover-design/TEMPLATE-TBD.md docs/technical-blueprint.md
# Work through Stage 1 to complete

# 3. Document key decisions
cp docs/templates/adr-template.md docs/adr/ADR-001-cloud-provider.md
# Document decision to use Azure
```

### Example 2: Documenting Component

```bash
# Create component documentation
cp docs/02-stage-develop-build/TEMPLATE-COMPONENT.md docs/components/auth-service.md
# Fill in component details

# Document API
# Add OpenAPI spec or API documentation
```

### Example 3: Setting Up Operations

```bash
# Create runbook
cp docs/04-stage-validate-operate/TEMPLATE-RUNBOOK.md docs/runbooks/api-operations.md
# Fill in operational procedures

# Create monitoring setup
cp docs/04-stage-validate-operate/TEMPLATE-MONITORING.md docs/monitoring/monitoring-setup.md
# Define metrics, alerts, dashboards
```

---

## 🆘 Getting Help

### Template Questions

- **Missing template?** Open an issue to request a new template
- **Template unclear?** Open an issue for clarification
- **Template improvements?** Submit a pull request

### Contributing Templates

We welcome template contributions:

1. Create your custom template
2. Test with real project
3. Document purpose and usage
4. Submit PR with template and examples

See [CONTRIBUTING.md](../../CONTRIBUTING.md) for guidelines.

---

## 📖 Additional Resources

- **[Quick Start Guide](../guides/QUICK-START.md)** - Get started quickly
- **[Step-by-Step Guide](../guides/STEP-BY-STEP.md)** - Complete walkthrough
- **[Customization Guide](../guides/CUSTOMIZATION.md)** - Adapt templates
- **[Sample Project](../../examples/sample-project/README.md)** - See templates in use

---

**Last Updated**: 2026-01-31  
**Version**: 1.0.0

---

[Back to Documentation Index](../README.md) | [ADR Template](adr-template.md) | [Project Kickoff](project-kickoff-checklist.md)
