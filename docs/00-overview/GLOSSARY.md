# Glossary

This glossary provides definitions for terms, acronyms, and concepts used throughout the Devviaai framework.

## Table of Contents

- [Framework Terms](#framework-terms)
- [Stage-Specific Terms](#stage-specific-terms)
- [Technology Terms](#technology-terms)
- [Azure Services](#azure-services)
- [Acronyms](#acronyms)

## Framework Terms

### Technical Blueprint Document (TBD)
The central artifact produced in Stage 1 that contains all design decisions, component specifications, and planning information. It serves as the single source of truth for the entire project.

### Context Chain
The explicit connection between stages where each stage declares its inputs (what it needs from previous stages) and outputs (what it provides to next stages). This ensures continuity and traceability.

### Traceability ID
A structured identifier (e.g., FR-001, COMP-001) that allows tracking from requirements through implementation to deployment and operations.

### Stage
One of the four major phases in the framework: Discover & Design, Develop & Build, Deploy & Deliver, Validate & Operate.

### Master Prompt
A carefully crafted, AI-ready prompt for each stage that guides the AI assistant in producing the required outputs with appropriate context and constraints.

## Stage-Specific Terms

### Stage 1: Discover & Design

**Requirements Registry**
A structured catalog of all functional and non-functional requirements, each with a unique identifier (FR-XXX or NFR-XXX).

**Component Catalog**
An inventory of all application and infrastructure components that will be built, each with specifications, dependencies, and interfaces.

**Resource Inventory**
A detailed list of all cloud resources (Azure services) that will be provisioned, including sizing, configuration, and cost estimates.

**Security Design**
Security architecture including authentication, authorization, network security, data protection, and compliance requirements.

**Task Backlog**
A prioritized list of development tasks derived from requirements and component specifications.

**Risk Matrix**
Identified risks with likelihood, impact, and mitigation strategies.

### Stage 2: Develop & Build

**Component Implementation**
The actual code and configuration for a component defined in the Component Catalog.

**Infrastructure as Code (IaC)**
Terraform/HCL modules that define and provision cloud resources declaratively.

**Unit Test**
Code that tests individual functions or methods in isolation.

**Integration Test**
Code that tests interactions between multiple components or services.

### Stage 3: Deploy & Deliver

**CI Pipeline (Continuous Integration)**
Automated workflow that builds, tests, and validates code on every commit or pull request.

**CD Pipeline (Continuous Deployment)**
Automated workflow that deploys validated code to target environments.

**Environment**
A complete deployment of the system (dev, staging, production) with all necessary resources and configurations.

**Secret Management**
Secure storage and injection of sensitive information like API keys, passwords, and certificates.

### Stage 4: Validate & Operate

**End-to-End Test**
Testing that validates complete user workflows across the entire system.

**Runbook**
Step-by-step operational procedures for common tasks (deployment, rollback, troubleshooting).

**Monitoring Configuration**
Setup for collecting metrics, logs, and traces from the running system.

**Dashboard**
Visual representation of system health, performance, and business metrics.

## Technology Terms

### HCL (HashiCorp Configuration Language)
The declarative language used by Terraform to define infrastructure resources.

### PLpgSQL (Procedural Language/PostgreSQL)
PostgreSQL's procedural language for writing stored procedures, functions, and triggers.

### Terraform
Infrastructure as Code tool for provisioning and managing cloud resources declaratively.

### GitHub Actions
CI/CD platform integrated with GitHub for automating workflows.

### Azure DevOps
Microsoft's DevOps platform offering CI/CD pipelines, repos, boards, and more.

### ARM Template (Azure Resource Manager)
Azure-native JSON-based infrastructure as code format (alternative to Terraform).

### Bicep
Domain-specific language (DSL) for deploying Azure resources declaratively (alternative to ARM/Terraform).

## Azure Services

### Azure App Service
Fully managed platform for building, deploying, and scaling web apps.

### Azure Functions
Serverless compute service for running event-driven code.

### Azure Container Apps
Fully managed environment for running containerized applications.

### Azure Kubernetes Service (AKS)
Managed Kubernetes service for container orchestration.

### Azure Database for PostgreSQL
Fully managed PostgreSQL database service.

### Azure SQL Database
Fully managed relational database service based on SQL Server.

### Azure Key Vault
Service for securely storing and accessing secrets, keys, and certificates.

### Azure Monitor
Comprehensive monitoring solution for collecting, analyzing, and acting on telemetry.

### Application Insights
Application Performance Management (APM) service for monitoring live applications.

### Azure Log Analytics
Service for querying and analyzing log data from Azure Monitor.

### Azure DevOps Services
Cloud-hosted version of Azure DevOps for CI/CD, repos, and project management.

### Azure Active Directory (Azure AD / Entra ID)
Microsoft's cloud-based identity and access management service.

### Azure Virtual Network (VNet)
Isolated network environment in Azure for resources.

### Azure Load Balancer
Service for distributing network traffic across multiple resources.

### Azure Application Gateway
Web traffic load balancer with application-level routing and SSL termination.

### Azure Front Door
Global, scalable entry point for web applications with CDN capabilities.

### Azure Storage
Scalable cloud storage for blobs, files, queues, and tables.

### Azure Blob Storage
Object storage service for unstructured data.

### Azure API Management
Fully managed service for publishing, securing, and analyzing APIs.

## Acronyms

### General

- **ADR**: Architecture Decision Record
- **API**: Application Programming Interface
- **ARM**: Azure Resource Manager
- **CI**: Continuous Integration
- **CD**: Continuous Deployment/Delivery
- **CQRS**: Command Query Responsibility Segregation
- **CRUD**: Create, Read, Update, Delete
- **CSV**: Comma-Separated Values
- **DTO**: Data Transfer Object
- **REST**: Representational State Transfer
- **JSON**: JavaScript Object Notation
- **YAML**: YAML Ain't Markup Language
- **XML**: eXtensible Markup Language

### Framework-Specific

- **TBD**: Technical Blueprint Document
- **FR**: Functional Requirement (e.g., FR-001)
- **NFR**: Non-Functional Requirement (e.g., NFR-001)
- **COMP**: Component (e.g., COMP-APP-001)
- **RES**: Resource (e.g., RES-AZ-001)
- **SEC**: Security Item (e.g., SEC-AUTH-001)
- **TASK**: Development Task (e.g., TASK-001)
- **RISK**: Risk Item (e.g., RISK-001)

### Architecture & Design

- **C4**: Context, Containers, Components, Code (architecture model)
- **SOA**: Service-Oriented Architecture
- **MSA**: Microservices Architecture
- **DDD**: Domain-Driven Design
- **TDD**: Test-Driven Development
- **BDD**: Behavior-Driven Development

### Cloud & DevOps

- **IaC**: Infrastructure as Code
- **PaaS**: Platform as a Service
- **SaaS**: Software as a Service
- **IaaS**: Infrastructure as a Service
- **VNet**: Virtual Network
- **NSG**: Network Security Group
- **RBAC**: Role-Based Access Control
- **SSO**: Single Sign-On
- **MFA**: Multi-Factor Authentication
- **WAF**: Web Application Firewall
- **CDN**: Content Delivery Network

### Quality & Operations

- **SLA**: Service Level Agreement
- **SLO**: Service Level Objective
- **SLI**: Service Level Indicator
- **RTO**: Recovery Time Objective
- **RPO**: Recovery Point Objective
- **MTTR**: Mean Time To Recovery
- **MTBF**: Mean Time Between Failures
- **APM**: Application Performance Management
- **AIOps**: Artificial Intelligence for IT Operations

### Azure-Specific

- **AAD**: Azure Active Directory (now Entra ID)
- **AKS**: Azure Kubernetes Service
- **ACR**: Azure Container Registry
- **ACA**: Azure Container Apps
- **APIM**: API Management
- **ASE**: App Service Environment
- **VMSS**: Virtual Machine Scale Set
- **NSG**: Network Security Group
- **UDR**: User Defined Route
- **VNET**: Virtual Network
- **KV**: Key Vault

### Databases

- **ACID**: Atomicity, Consistency, Isolation, Durability
- **BASE**: Basically Available, Soft state, Eventually consistent
- **OLTP**: Online Transaction Processing
- **OLAP**: Online Analytical Processing
- **ETL**: Extract, Transform, Load
- **ORM**: Object-Relational Mapping
- **SQL**: Structured Query Language
- **NoSQL**: Not Only SQL

### Security

- **TLS**: Transport Layer Security
- **SSL**: Secure Sockets Layer
- **HTTPS**: HTTP Secure
- **OAuth**: Open Authorization
- **OIDC**: OpenID Connect
- **JWT**: JSON Web Token
- **SAML**: Security Assertion Markup Language
- **DDoS**: Distributed Denial of Service
- **XSS**: Cross-Site Scripting
- **CSRF**: Cross-Site Request Forgery
- **CORS**: Cross-Origin Resource Sharing

## See Also

- [Framework Overview](README.md) - Understanding the framework
- [Architecture](ARCHITECTURE.md) - Technical architecture details
- [Documentation Index](../README.md) - Complete documentation

---

[Back to Documentation Index](../README.md)
