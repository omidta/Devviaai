# Quick Start Guide

Get up and running with the Devviaai Framework in 15 minutes. This guide will walk you through creating your first Technical Blueprint Document and understanding the framework basics.

## ⏱️ Time Required

**15 minutes** to complete this guide and generate your first TBD.

## 🎯 What You'll Learn

- How the 4-stage workflow works
- How to use AI assistants with the framework
- How to create a Technical Blueprint Document
- What to do next after this guide

## ✅ Prerequisites

Before starting, ensure you have:

- [ ] **AI Assistant Access**: GitHub Copilot, ChatGPT, Claude, or similar
- [ ] **Basic Requirements**: A project idea or requirements document
- [ ] **Text Editor**: Any editor for viewing/editing Markdown files
- [ ] **Git** (optional): For cloning the repository

### Optional but Recommended

- Azure subscription (for later stages)
- Familiarity with cloud concepts
- Understanding of software architecture basics

## 🚀 Step 1: Understand the Framework (3 minutes)

### The Four Stages

The Devviaai Framework follows a simple 4-stage flow:

```
Requirements → Stage 1: Design → TBD → Stage 2: Build → Code → Stage 3: Deploy → Live System → Stage 4: Operate
```

| Stage | You Act As | Input | Output | Duration |
|-------|-----------|-------|--------|----------|
| **1** | Solution Architect | Requirements | Technical Blueprint (TBD) | Hours to days |
| **2** | Software Engineer | TBD + Task | Code + Tests | Days to weeks |
| **3** | Platform Engineer | TBD + Code | CI/CD + Environments | Hours to days |
| **4** | QA/Ops Engineer | Deployed System | Validation + Monitoring | Days |

### Key Concept: The TBD

The **Technical Blueprint Document (TBD)** is your single source of truth:
- Created in Stage 1
- Used by all other stages
- Contains all design decisions
- Maintains full traceability

## 🏗️ Step 2: Access the Framework (2 minutes)

### Option A: Clone the Repository

```bash
git clone https://github.com/omidta/Devviaai.git
cd Devviaai
```

### Option B: Browse Online

Navigate to: https://github.com/omidta/Devviaai/tree/main/docs

### What You Need

For this quick start, you only need:
1. Stage 1 Master Prompt: `docs/01-stage-discover-design/PROMPT.md`
2. TBD Template: `docs/01-stage-discover-design/TEMPLATE-TBD.md`

## 📝 Step 3: Prepare Your Requirements (2 minutes)

Create a simple requirements document or use this example:

```markdown
# Project: Customer Portal

## Business Requirements
- Customers need to manage their profile information
- Customers need to view their order history
- Customers need to contact support

## Technical Requirements
- Must use Azure
- Must support 1,000 concurrent users
- Must be available 99.9% of the time
- Must integrate with existing customer database

## Constraints
- Budget: $500/month for Azure
- Timeline: 8 weeks
- Team: 2 developers
```

💡 **Tip**: Start small! Even 5-6 bullet points are enough for this quick start.

## 🤖 Step 4: Use AI to Generate Your TBD (5 minutes)

### 4.1 Open the Stage 1 Prompt

Open `docs/01-stage-discover-design/PROMPT.md` in your editor or view it online.

### 4.2 Customize the Prompt

Copy the prompt and replace the placeholders:

- `[PROJECT_NAME]` → Your project name
- `[PROJECT_DESCRIPTION]` → Brief description
- `[FUNCTIONAL_REQUIREMENTS]` → Your requirements
- `[CONSTRAINTS]` → Your constraints

### 4.3 Submit to Your AI Assistant

1. Open your AI assistant (ChatGPT, Claude, GitHub Copilot Chat)
2. Paste your customized prompt
3. Submit and wait for the response

### 4.4 Review the Output

Your AI will generate a structured TBD with:
- ✅ Requirements Registry (FR-XXX, NFR-XXX)
- ✅ Component Catalog (COMP-XXX)
- ✅ Resource Inventory (RES-XXX)
- ✅ Security Design (SEC-XXX)
- ✅ Task Backlog (TASK-XXX)
- ✅ Risk Matrix (RISK-XXX)

## ✅ Step 5: Verify Your TBD (2 minutes)

Check that your TBD includes:

- [ ] At least 3-5 functional requirements (FR-001, FR-002, ...)
- [ ] At least 2-3 non-functional requirements (NFR-001, ...)
- [ ] At least 2-3 components (COMP-APP-001, ...)
- [ ] At least 3-5 Azure resources (RES-AZ-001, ...)
- [ ] Security considerations (SEC-XXX)
- [ ] A task list (TASK-001, ...)
- [ ] Identified risks (RISK-001, ...)

✅ **Success Indicator**: You have a structured document with IDs and clear sections.

## 🎉 Step 6: What's Next? (1 minute)

Congratulations! You've created your first Technical Blueprint Document.

### Immediate Next Steps

1. **Save Your TBD**: Save it as `technical-blueprint.md` in your project
2. **Review Details**: Read through each section to understand the design
3. **Share with Team**: Get feedback from stakeholders

### Continue Your Journey

Choose your path:

#### Path A: Learn More About the Framework
→ Read the [Step-by-Step Guide](STEP-BY-STEP.md) for comprehensive coverage

#### Path B: Start Implementing
→ Move to [Stage 2: Develop & Build](../02-stage-develop-build/README.md)

#### Path C: See a Complete Example
→ Review the [Sample Project](../../examples/sample-project/README.md)

## 🔥 Common Pitfalls & Solutions

### Pitfall 1: Too Many Requirements
**Problem**: Including 50+ requirements in first iteration  
**Solution**: Start with 10-15 core requirements, iterate later  
**Why**: Keeps TBD manageable and speeds up initial implementation

### Pitfall 2: Vague Requirements
**Problem**: "System should be fast" without specifics  
**Solution**: Use measurable criteria: "API response < 200ms"  
**Why**: Enables validation and proper design decisions

### Pitfall 3: Skipping Security
**Problem**: No security considerations in initial TBD  
**Solution**: Always include authentication, authorization, encryption  
**Why**: Security is harder to add later than to design in

### Pitfall 4: Over-Engineering
**Problem**: Designing for 1M users when you have 100  
**Solution**: Design for current needs + 3x growth  
**Why**: Balance between scalability and cost/complexity

### Pitfall 5: Ignoring the TBD
**Problem**: Creating TBD but not referencing it in implementation  
**Solution**: Always reference TBD IDs in code comments and commits  
**Why**: Maintains traceability and context

### Pitfall 6: Not Using AI Effectively
**Problem**: One-shot prompting without iteration  
**Solution**: Ask AI to refine, expand, or clarify sections  
**Why**: AI assistants improve with iterative feedback

## 💡 Pro Tips

### Tip 1: Start Small, Iterate
Create a minimal TBD first, then enhance it. Don't aim for perfection on the first pass.

### Tip 2: Use the Sample Project
When stuck, refer to the sample Customer Portal project for examples of how to structure each section.

### Tip 3: AI Iteration
After generating the TBD, ask your AI assistant:
- "Can you expand the security design?"
- "Add more details to COMP-APP-001"
- "What risks am I missing?"

### Tip 4: Keep IDs Consistent
Once you assign an ID (e.g., FR-001), use it consistently across all documents and code.

### Tip 5: Document Decisions
When making architecture decisions, add them to the TBD or create Architecture Decision Records (ADRs).

### Tip 6: Version Control Your TBD
Commit the TBD to your repository. It should evolve with your project.

## 📚 Quick Reference

### Essential TBD IDs

| Prefix | What It Means | Example | Use Case |
|--------|---------------|---------|----------|
| FR | Functional Requirement | FR-001 | User can log in |
| NFR | Non-Functional Requirement | NFR-001 | 99.9% uptime |
| COMP | Component | COMP-APP-001 | Auth service |
| RES | Azure Resource | RES-AZ-001 | App Service |
| SEC | Security Item | SEC-AUTH-001 | Azure AD SSO |
| TASK | Development Task | TASK-001 | Build login API |
| RISK | Project Risk | RISK-001 | Database performance |

### Key Commands

```bash
# Clone framework
git clone https://github.com/omidta/Devviaai.git

# View Stage 1 prompt
cat docs/01-stage-discover-design/PROMPT.md

# View TBD template
cat docs/01-stage-discover-design/TEMPLATE-TBD.md

# View sample project
ls examples/sample-project/
```

## 🆘 Troubleshooting

### Issue: AI Output Is Too Generic
**Solution**: Provide more specific requirements and constraints in your prompt. Include technical details, business context, and examples.

### Issue: Missing Sections in TBD
**Solution**: Copy the TBD template structure and explicitly ask AI to fill each section.

### Issue: Can't Decide on Azure Services
**Solution**: Ask AI: "What Azure services are best for [your use case]?" Then discuss trade-offs.

### Issue: Too Complex for First Project
**Solution**: Simplify! Remove advanced features, reduce scope, focus on core functionality.

### Issue: Team Doesn't Understand TBD
**Solution**: Schedule a walkthrough session. Use the C4 diagrams to explain visually.

## 📖 Additional Resources

- **[Framework Overview](../00-overview/README.md)** - Deep dive into philosophy
- **[Stage 1 Full Documentation](../01-stage-discover-design/README.md)** - Complete Stage 1 guide
- **[Glossary](../00-overview/GLOSSARY.md)** - Terms and acronyms
- **[Step-by-Step Guide](STEP-BY-STEP.md)** - Comprehensive walkthrough

## ✨ What You've Accomplished

In just 15 minutes, you have:

- ✅ Understood the 4-stage framework
- ✅ Created a Technical Blueprint Document
- ✅ Learned to use AI with structured prompts
- ✅ Established traceability with TBD IDs
- ✅ Set the foundation for implementation

**You're now ready to move forward with Stages 2, 3, and 4!**

## 🎯 Your Next Action

Pick one:

1. **Go Deeper**: Read [Step-by-Step Guide](STEP-BY-STEP.md)
2. **Start Building**: Move to [Stage 2](../02-stage-develop-build/README.md)
3. **See Example**: Review [Sample Project](../../examples/sample-project/README.md)
4. **Customize**: Read [Customization Guide](CUSTOMIZATION.md)

---

**Questions?** Open an issue or discussion on GitHub.

**Found this helpful?** Star the repository and share with your team!

---

[Back to Guides Index](README.md) | [Documentation Index](../README.md) | [Step-by-Step Guide](STEP-BY-STEP.md)
