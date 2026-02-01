# ADR-NNN: [Short Title of Decision]

**Status**: [Proposed | Accepted | Deprecated | Superseded]  
**Date**: [YYYY-MM-DD]  
**Decision Makers**: [Names or roles of people involved]  
**Tags**: [Relevant tags: architecture, security, infrastructure, etc.]

## Context and Problem Statement

[Describe the context and the problem that needs to be solved. Be clear about why a decision is needed.]

**Example**:
> We need to select a database solution for our customer management system. The system must handle 1M+ customer records, support complex queries, provide strong consistency, and integrate well with our Azure infrastructure.

## Decision Drivers

[List the factors that influence the decision. These help explain the priorities and constraints.]

* [Driver 1: e.g., Performance requirements]
* [Driver 2: e.g., Budget constraints]
* [Driver 3: e.g., Team expertise]
* [Driver 4: e.g., Regulatory compliance]
* [Driver 5: e.g., Operational simplicity]

**Example**:
* Need for ACID transactions and strong consistency
* Must support 10,000+ concurrent users
* Team has PostgreSQL experience
* Monthly budget of $500 for database services
* Prefer managed service to minimize operational overhead
* Must integrate with Azure ecosystem

## Considered Options

### Option 1: [Option Name]

**Description**: [Brief description of this option]

**Pros**:
* [Advantage 1]
* [Advantage 2]
* [Advantage 3]

**Cons**:
* [Disadvantage 1]
* [Disadvantage 2]
* [Disadvantage 3]

**Cost**: [Estimated monthly cost]

**Example**:
### Option 1: Azure Database for PostgreSQL (Flexible Server)

**Description**: Managed PostgreSQL service in Azure with flexible server option.

**Pros**:
* Fully managed (automated backups, patching, high availability)
* Strong ACID compliance and data consistency
* Team already knows PostgreSQL well
* Excellent integration with Azure services
* Good performance for transactional workloads
* Built-in monitoring and alerting

**Cons**:
* Higher cost compared to single-server option
* PostgreSQL-specific features may differ from vanilla PostgreSQL
* Limited to Azure ecosystem

**Cost**: ~$164/month for General Purpose 2 vCores

---

### Option 2: [Option Name]

[Follow same structure as Option 1]

**Example**:
### Option 2: Azure Cosmos DB (PostgreSQL API)

**Description**: Globally distributed NoSQL database with PostgreSQL wire protocol compatibility.

**Pros**:
* Global distribution and multi-region replication
* Horizontal scalability
* Multiple consistency models
* Fully managed service
* Good for write-heavy workloads

**Cons**:
* Higher cost (RU-based pricing)
* Not true PostgreSQL (compatibility layer)
* Learning curve for team
* Might be overkill for single-region app
* More complex operational model

**Cost**: ~$500+/month depending on throughput

---

### Option 3: [Option Name]

[Follow same structure]

**Example**:
### Option 3: Azure SQL Database

**Description**: Microsoft's managed relational database service.

**Pros**:
* Highly mature and stable
* Excellent Azure integration
* Advanced features (temporal tables, columnstore indexes)
* Good management tools
* Strong performance

**Cons**:
* Team lacks SQL Server experience
* Different SQL dialect from PostgreSQL
* Migration from existing PostgreSQL databases would be complex
* Licensing considerations

**Cost**: ~$150/month for Standard S2

---

### Option 4: [Option Name]

[Add more options as needed]

## Decision Outcome

**Chosen option**: [Option X: Name]

**Rationale**: [Explain why this option was chosen over the others. Reference decision drivers.]

**Example**:
**Chosen option**: Option 1 - Azure Database for PostgreSQL (Flexible Server)

**Rationale**: 
We selected Azure Database for PostgreSQL (Flexible Server) because it best satisfies our decision drivers:

1. **Team Expertise**: Our team has 3+ years of PostgreSQL experience, eliminating learning curve and reducing risk
2. **ACID Compliance**: PostgreSQL provides the strong consistency we need for customer data
3. **Managed Service**: Flexible Server is fully managed, aligning with our goal of operational simplicity
4. **Cost**: At $164/month, it fits comfortably within our $500 budget
5. **Azure Integration**: Native Azure service with excellent integration with App Service, Monitor, and Key Vault
6. **Performance**: Provides sufficient performance for our 10K concurrent user requirement based on benchmarks

While Cosmos DB offers global distribution, we don't currently need multi-region support. Azure SQL Database would require team retraining and migration effort. PostgreSQL Flexible Server provides the right balance of capabilities, cost, and team fit.

## Implementation

**Required Actions**:
- [ ] [Action item 1]
- [ ] [Action item 2]
- [ ] [Action item 3]

**Timeline**: [Expected implementation timeframe]

**Responsible**: [Person/Team responsible for implementation]

**Example**:
**Required Actions**:
- [ ] Provision Azure Database for PostgreSQL (Flexible Server) in dev environment
- [ ] Configure backup retention and high availability settings
- [ ] Set up Azure Private Link for secure connectivity
- [ ] Configure connection pooling in application
- [ ] Update infrastructure-as-code (Terraform) to include database
- [ ] Document connection strings and configuration in Key Vault
- [ ] Update application configuration to use managed database
- [ ] Run migration scripts to import existing data
- [ ] Perform load testing to validate performance
- [ ] Set up monitoring and alerting in Azure Monitor

**Timeline**: Week 1-2 of Sprint 3

**Responsible**: Backend team (Lead: Jane Doe)

## Consequences

### Positive Consequences

* [Positive outcome 1]
* [Positive outcome 2]
* [Positive outcome 3]

**Example**:
* Team can leverage existing PostgreSQL knowledge, reducing development time
* Managed service reduces operational burden (no manual patching, backups, etc.)
* Strong consistency model ensures data integrity for financial transactions
* Native Azure integration simplifies architecture
* Automated backups and point-in-time restore provide data safety
* Cost-effective solution within budget constraints

### Negative Consequences

* [Negative outcome 1]
* [Negative outcome 2]
* [Negative outcome 3]

**Example**:
* Locked into Azure ecosystem (multi-cloud strategy not possible without re-architecture)
* Some PostgreSQL extensions may not be available in managed service
* Vertical scaling requires downtime (though minimal with Flexible Server)
* If we need global distribution later, migration to Cosmos DB would be required

### Risks and Mitigations

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| [Risk description] | [High/Med/Low] | [High/Med/Low] | [How to mitigate] |

**Example**:
| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Database becomes a bottleneck at scale | High | Medium | Implement caching layer (Redis), optimize queries, monitor performance metrics |
| Vendor lock-in limits future flexibility | Medium | High | Abstract database access through repository pattern, document migration path to self-hosted PostgreSQL |
| Cost increases beyond budget as data grows | Medium | Medium | Set up cost alerts, implement data archival strategy, monitor storage growth |
| Connection pool exhaustion under load | Medium | Low | Configure appropriate connection limits, implement circuit breakers, use connection pooling best practices |

## Links

**Related Decisions**:
* [ADR-XXX: Title of related decision]
* [ADR-YYY: Title of another related decision]

**Related Documents**:
* [TBD Section X: Database Design]
* [Architecture Diagram: System Context]
* [Performance Requirements: NFR-001]

**References**:
* [External link 1: e.g., Azure documentation]
* [External link 2: e.g., benchmark results]
* [External link 3: e.g., blog post or article]

**Example**:
**Related Decisions**:
* ADR-002: Caching Strategy - Uses Redis with PostgreSQL
* ADR-005: Data Retention Policy - Affects database storage planning

**Related Documents**:
* TBD Section 3.1: Azure Resources - RES-AZ-002 (PostgreSQL Database)
* TBD Section 1.2: Non-Functional Requirements - NFR-002 (Database query performance)
* Architecture Diagram: C4 Container Diagram shows database placement

**References**:
* [Azure Database for PostgreSQL documentation](https://docs.microsoft.com/azure/postgresql/)
* [PostgreSQL connection pooling best practices](https://www.postgresql.org/docs/current/runtime-config-connection.html)
* [Cost calculator for Azure Database for PostgreSQL](https://azure.microsoft.com/pricing/calculator/)

## Review and Update History

| Date | Reviewer | Changes | Version |
|------|----------|---------|---------|
| [YYYY-MM-DD] | [Name] | [Description of changes] | [1.0] |

**Example**:
| Date | Reviewer | Changes | Version |
|------|----------|---------|---------|
| 2026-01-15 | Architecture Team | Initial decision documented | 1.0 |
| 2026-02-10 | Jane Doe | Updated cost estimates after implementation | 1.1 |
| 2026-03-20 | Security Team | Added encryption requirements to implementation | 1.2 |

## Notes

[Any additional notes, assumptions, or context that doesn't fit in other sections]

**Example**:
* We may reconsider this decision if we expand to other cloud providers
* Performance benchmarks were conducted with 100GB dataset and 5000 concurrent connections
* Decision made before Azure Cosmos DB for PostgreSQL reached general availability
* This ADR supersedes verbal decision made in planning meeting on 2026-01-10

---

## How to Use This Template

1. **Copy this template** to your project's ADR directory
2. **Rename the file** following the pattern: `ADR-NNN-short-title.md`
   - Example: `ADR-001-database-selection.md`
3. **Fill in all sections**, removing example content
4. **Update status** as the decision progresses:
   - **Proposed**: Decision is under discussion
   - **Accepted**: Decision has been made and approved
   - **Deprecated**: Decision is no longer relevant
   - **Superseded**: Decision has been replaced by another ADR
5. **Link from TBD** and other relevant documents
6. **Keep updated** as implementation progresses or circumstances change

## Tips for Writing Good ADRs

✅ **Do:**
- Write ADRs for significant decisions that affect architecture, not trivial choices
- Be concise but complete - aim for clarity, not length
- Document the decision process, not just the outcome
- Include concrete pros/cons for each option
- Link to related documents and decisions
- Update ADRs when circumstances change

❌ **Don't:**
- Create ADRs for every minor decision - focus on significant choices
- Write novels - keep it focused and scannable
- Skip the alternatives - document what you *didn't* choose and why
- Forget to update status as decisions evolve
- Ignore consequences - both positive and negative matter

## ADR Best Practices

1. **Number Sequentially**: Use ADR-001, ADR-002, etc. Never reuse numbers.
2. **Write Early**: Document decisions when they're made, not months later.
3. **Be Honest**: Include real constraints (budget, time, team skills).
4. **Update Status**: Mark as Deprecated or Superseded when replaced.
5. **Link Extensively**: Connect ADRs to TBD, code, and other ADRs.
6. **Review Periodically**: Revisit ADRs to validate decisions are still valid.

---

[Back to Templates Index](README.md) | [Documentation Index](../README.md) | [Project Kickoff Checklist](project-kickoff-checklist.md)
