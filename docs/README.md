# Devviaai Documentation Index

Welcome to the complete documentation for the Devviaai Unified Workflow Framework. This documentation will guide you through implementing Azure solutions using a structured, AI-assisted 4-stage approach.

## 📖 Table of Contents

### Getting Started

- **[Quick Start Guide](guides/QUICK-START.md)** - Get up and running in 15 minutes
- **[Step-by-Step Guide](guides/STEP-BY-STEP.md)** - Comprehensive walkthrough of the framework
- **[Customization Guide](guides/CUSTOMIZATION.md)** - Adapt the framework to your project needs

### Framework Overview

- **[Framework Overview](00-overview/README.md)** - Core concepts and architecture
- **[Architecture](00-overview/ARCHITECTURE.md)** - Detailed framework architecture
- **[Glossary](00-overview/GLOSSARY.md)** - Terms, definitions, and acronyms

### Stage Documentation

Each stage has comprehensive documentation including overview, master prompts, and templates:

#### Stage 1: Discover & Design
- **[Overview](01-stage-discover-design/README.md)** - Architecture and design phase
- **[Master Prompt](01-stage-discover-design/PROMPT.md)** - AI-ready prompt for Stage 1
- **[TBD Template](01-stage-discover-design/TEMPLATE-TBD.md)** - Technical Blueprint Document template

#### Stage 2: Develop & Build
- **[Overview](02-stage-develop-build/README.md)** - Implementation phase
- **[Master Prompt](02-stage-develop-build/PROMPT.md)** - AI-ready prompt for Stage 2
- **[Component Template](02-stage-develop-build/TEMPLATE-COMPONENT.md)** - Application component template
- **[Infrastructure Template](02-stage-develop-build/TEMPLATE-INFRASTRUCTURE.md)** - IaC module template

#### Stage 3: Deploy & Deliver
- **[Overview](03-stage-deploy-deliver/README.md)** - Deployment and delivery phase
- **[Master Prompt](03-stage-deploy-deliver/PROMPT.md)** - AI-ready prompt for Stage 3
- **[CI Pipeline Template](03-stage-deploy-deliver/TEMPLATE-PIPELINE-CI.md)** - Continuous Integration template
- **[CD Pipeline Template](03-stage-deploy-deliver/TEMPLATE-PIPELINE-CD.md)** - Continuous Deployment template

#### Stage 4: Validate & Operate
- **[Overview](04-stage-validate-operate/README.md)** - Validation and operations phase
- **[Master Prompt](04-stage-validate-operate/PROMPT.md)** - AI-ready prompt for Stage 4
- **[Monitoring Template](04-stage-validate-operate/TEMPLATE-MONITORING.md)** - Monitoring strategy template
- **[Runbook Template](04-stage-validate-operate/TEMPLATE-RUNBOOK.md)** - Operations runbook template

### Templates & Resources

- **[Templates Index](templates/README.md)** - All available templates
- **[ADR Template](templates/adr-template.md)** - Architecture Decision Record
- **[Project Kickoff Checklist](templates/project-kickoff-checklist.md)** - Project initialization checklist

### Examples

- **[Sample Project](../examples/sample-project/README.md)** - Complete end-to-end example

## 🎯 How to Navigate This Documentation

### For First-Time Users

1. Start with the **[Quick Start Guide](guides/QUICK-START.md)** to understand the basics
2. Review the **[Framework Overview](00-overview/README.md)** to understand core concepts
3. Follow the **[Step-by-Step Guide](guides/STEP-BY-STEP.md)** for your first project
4. Reference the **[Sample Project](../examples/sample-project/README.md)** as needed

### For Experienced Users

1. Jump to the relevant **Stage Documentation** for the phase you're working on
2. Use the **Master Prompts** with your AI assistant
3. Copy and customize the **Templates** for your deliverables
4. Reference the **[Glossary](00-overview/GLOSSARY.md)** for terminology

### For Team Leads

1. Review the **[Framework Overview](00-overview/README.md)** to understand the complete workflow
2. Use the **[Project Kickoff Checklist](templates/project-kickoff-checklist.md)** to start new projects
3. Share the **[Quick Start Guide](guides/QUICK-START.md)** with team members
4. Customize the framework using the **[Customization Guide](guides/CUSTOMIZATION.md)**

## 🔍 Document Conventions

Throughout this documentation, you'll encounter:

- **`[PLACEHOLDER]`** - Values you need to replace with project-specific information
- **TBD IDs** - Structured identifiers like FR-001, COMP-001, RES-001
- **Code Blocks** - Copy-paste ready examples and templates
- **Diagrams** - ASCII art for visualizing concepts and flows

## 📋 Quick Reference

### Stage Flow

```
Requirements → Stage 1 → TBD → Stage 2 → Code/IaC → Stage 3 → Deployed System → Stage 4 → Operations
```

### Key Artifacts by Stage

| Stage | Key Input | Key Output |
|-------|-----------|------------|
| 1 | Requirements, Constraints | Technical Blueprint Document (TBD) |
| 2 | TBD, Task ID | Application Code, Infrastructure Code, Tests |
| 3 | TBD, Stage 2 Artifacts | CI/CD Pipelines, Running Environments |
| 4 | TBD, Deployed System | Validation Reports, Monitoring, Runbooks |

### Common Patterns

- **Iterative Refinement**: Stages can be revisited as requirements evolve
- **Context Preservation**: Each stage explicitly references inputs and outputs
- **Traceability**: All artifacts link back to TBD identifiers
- **AI Collaboration**: Master prompts are optimized for AI assistants

## 🆘 Getting Help

- **Troubleshooting**: Check the [Step-by-Step Guide](guides/STEP-BY-STEP.md) for common issues
- **Questions**: Review the [Glossary](00-overview/GLOSSARY.md) for terminology
- **Examples**: See the [Sample Project](../examples/sample-project/README.md) for reference implementations
- **Issues**: Report problems at [GitHub Issues](https://github.com/omidta/Devviaai/issues)

## 🔄 Documentation Updates

This documentation is actively maintained. Check the repository for the latest updates and improvements.

**Last Updated**: 2026-01-31
**Version**: 1.0.0

---

[Back to Main README](../README.md) | [Quick Start](guides/QUICK-START.md) | [Framework Overview](00-overview/README.md)
