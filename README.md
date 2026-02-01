# Devviaai - Unified AI-Assisted Development & DevOps Workflow Framework

[![Documentation](https://img.shields.io/badge/docs-latest-blue.svg)](docs/README.md)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Azure](https://img.shields.io/badge/cloud-Azure-0078D4.svg)](https://azure.microsoft.com)

## 🎯 Executive Summary

Devviaai is a comprehensive, reusable 4-stage workflow framework designed to streamline the implementation of code and infrastructure in Azure environments. By combining software engineering best practices with modern DevOps methodologies, this framework provides AI-assisted guidance through the entire development lifecycle—from initial design to production operations.

## ✨ Key Features

- **🔄 4-Stage Integrated Workflow**: Seamlessly moves from design through deployment to operations
- **🤖 AI-Assisted Prompts**: Ready-to-use master prompts for each stage with AI tools like GitHub Copilot
- **📋 Production-Ready Templates**: Comprehensive templates for all artifacts and deliverables
- **☁️ Azure-Native**: Optimized for Azure cloud services with adaptability to other platforms
- **📊 Full Traceability**: Complete lineage from requirements to deployed resources
- **🏗️ Industry Standards**: Based on Arc42, C4 Model, and Azure Well-Architected Framework
- **📖 Comprehensive Documentation**: Step-by-step guides, examples, and best practices

## 🏛️ Framework Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVVIAAI WORKFLOW FRAMEWORK                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌───────────────┐      ┌───────────────┐                      │
│  │   Stage 1:    │      │   Stage 2:    │                      │
│  │   DISCOVER &  │─────▶│   DEVELOP &   │                      │
│  │    DESIGN     │      │     BUILD     │                      │
│  └───────────────┘      └───────────────┘                      │
│         │                      │                                │
│         │                      │                                │
│         ▼                      ▼                                │
│  ┌───────────────┐      ┌───────────────┐                      │
│  │   Stage 4:    │◀─────│   Stage 3:    │                      │
│  │   VALIDATE &  │      │   DEPLOY &    │                      │
│  │    OPERATE    │      │    DELIVER    │                      │
│  └───────────────┘      └───────────────┘                      │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│  INPUT: Requirements & Constraints                              │
│  OUTPUT: Production System with Monitoring & Operations         │
└─────────────────────────────────────────────────────────────────┘
```

### The Four Stages

| Stage | Role | Input | Output | Purpose |
|-------|------|-------|--------|---------|
| **1. Discover & Design** | Solution Architect | Requirements, Constraints | Technical Blueprint Document (TBD) | Define architecture and create comprehensive design |
| **2. Develop & Build** | Full-Stack Engineer | TBD, Task ID | Code, Infrastructure as Code, Tests | Implement components and infrastructure |
| **3. Deploy & Deliver** | Platform Engineer | TBD, Artifacts | CI/CD Pipelines, Environments | Automate deployment and delivery |
| **4. Validate & Operate** | Quality & Reliability Engineer | TBD, Deployed System | Validation Reports, Monitoring, Runbooks | Ensure quality and operational readiness |

## 🚀 Quick Start

### Prerequisites

- Azure subscription (or another cloud provider)
- Git installed
- Access to an AI assistant (GitHub Copilot, ChatGPT, Claude, etc.)
- Basic knowledge of:
  - Infrastructure as Code (Terraform/HCL)
  - CI/CD concepts
  - Cloud services (Azure preferred)

### 15-Minute Quick Start

1. **Clone this repository**
   ```bash
   git clone https://github.com/omidta/Devviaai.git
   cd Devviaai
   ```

2. **Review the framework overview**
   ```bash
   cat docs/README.md
   cat docs/guides/QUICK-START.md
   ```

3. **Start with Stage 1**
   - Review the Stage 1 prompt: `docs/01-stage-discover-design/PROMPT.md`
   - Use your AI assistant with the prompt and your requirements
   - Generate your Technical Blueprint Document using the template

4. **Continue through the stages**
   - Each stage builds on the previous one
   - Use the provided prompts and templates
   - Follow the step-by-step guide for detailed instructions

For a complete walkthrough, see [Quick Start Guide](docs/guides/QUICK-START.md).

## 📚 Documentation

### Core Documentation

- **[Documentation Index](docs/README.md)** - Start here for complete documentation
- **[Quick Start Guide](docs/guides/QUICK-START.md)** - Get started in 15 minutes
- **[Step-by-Step Guide](docs/guides/STEP-BY-STEP.md)** - Detailed implementation guide
- **[Customization Guide](docs/guides/CUSTOMIZATION.md)** - Adapt the framework to your needs
- **[Innovation Workflow](docs/guides/INNOVATION-WORKFLOW.md)** - Manage ideas using Design Thinking, Lean Startup, or Agile

### Stage Documentation

- **[Stage 1: Discover & Design](docs/01-stage-discover-design/README.md)** - Architecture and design
- **[Stage 2: Develop & Build](docs/02-stage-develop-build/README.md)** - Implementation
- **[Stage 3: Deploy & Deliver](docs/03-stage-deploy-deliver/README.md)** - CI/CD and deployment
- **[Stage 4: Validate & Operate](docs/04-stage-validate-operate/README.md)** - Quality and operations

### Reference Materials

- **[Framework Overview](docs/00-overview/README.md)** - Architecture and principles
- **[Glossary](docs/00-overview/GLOSSARY.md)** - Terms and definitions
- **[Templates](docs/templates/README.md)** - All document templates
- **[Examples](examples/sample-project/README.md)** - Working example project

## 💡 Use Cases

This framework is ideal for:

- **Greenfield Projects**: Starting new Azure-based applications from scratch
- **Migration Projects**: Moving existing applications to Azure with proper documentation
- **Modernization Initiatives**: Updating legacy systems with modern DevOps practices
- **Multi-Team Projects**: Ensuring consistent practices across distributed teams
- **AI-Assisted Development**: Leveraging AI tools for rapid, high-quality delivery
- **Training & Onboarding**: Teaching teams structured software delivery practices

## 🎓 Key Framework Principles

1. **Context Chain**: Each stage explicitly references previous outputs and declares what it provides next
2. **Full Traceability**: All artifacts reference TBD IDs (FR-XXX, NFR-XXX, COMP-XXX, RES-XXX)
3. **Modularity**: Prompts are self-contained but interconnected
4. **Azure-Native**: Designed for Azure but adaptable to other platforms
5. **Industry Standards**: Based on Arc42, C4 Model, Azure Well-Architected Framework
6. **AI-Optimized**: Prompts designed for effective use with AI assistants

## 🛠️ Technology Stack

The framework supports and recommends:

- **Infrastructure**: Terraform/HCL for Azure
- **Backend**: PLpgSQL (PostgreSQL), PowerShell, Python, Node.js
- **CI/CD**: GitHub Actions, Azure DevOps Pipelines
- **Monitoring**: Azure Monitor, Application Insights
- **Documentation**: Markdown, Architecture Decision Records (ADRs)

## 🤝 Contributing

We welcome contributions! Whether it's:

- 🐛 Bug reports and fixes
- 📝 Documentation improvements
- ✨ New templates or examples
- 💡 Framework enhancements

Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

This framework is built on industry best practices and standards:

- **Arc42** - Architecture documentation template
- **C4 Model** - Software architecture diagrams
- **Azure Well-Architected Framework** - Cloud architecture principles
- **The Twelve-Factor App** - Cloud-native application methodology

## 📬 Support & Contact

- **Documentation Issues**: [Open an issue](https://github.com/omidta/Devviaai/issues)
- **Questions**: Use [GitHub Discussions](https://github.com/omidta/Devviaai/discussions)
- **Project Repository**: [https://github.com/omidta/Devviaai](https://github.com/omidta/Devviaai)

---

**Made with ❤️ for developers who value structure, quality, and AI-assisted productivity**
