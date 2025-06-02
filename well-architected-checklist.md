# Well-Architected Microservices Checklist

## Operational Excellence

This section focuses on the areas that can help to improve the operational excellence of microservices. Continuous improvement of the operations maturity itself is considered out of scope.

### Ownership

- [ ] Mechanism to identify ownership of the service (development and runtime) (For ex: Using tags, labels, etc.)
- [ ] Point of contacts for updates and changes
- [ ] Escalation process and matrix
- [ ] Does not apply

### Logging & Monitoring

- [ ] Application telemetry
- [ ] User activity telemetry
- [ ] Dependency telemetry
- [ ] Transaction traceability
- [ ] Does not apply

### Observability

- [ ] Workload health 
  - [ ] Identify KPIs and metrics
  - [ ] Metrics Baseline available
  - [ ] Expected traffic and metrics patterns
  - [ ] Iterate KPI and Metrics review and adjustments
  - [ ] Dashboard creation and on-boarding
  - [ ] HTTP request/transaction status, time etc. would be ingested to NewRelic.
  - [ ] Alert policies and notification channels would be configured in NewRelic.
- [ ] Tracing mechanism implemented

### Deployment/Rollback Automation

- [ ] Use of version control
- [ ] Testing strategies defined and implemented
  - [ ] Load tests successful
- [ ] Deployment and rollback automation
  - [ ] Ability to rollback
    - [ ] Automated
    - [ ] Manual Process
- [ ] Deployment strategies - Canary, Blue/Green, etc.
- [ ] Security updates strategy
- [ ] Immutable infra
- [ ] Required documentation
- [ ] Any environment specific considerations

### Operational Readiness

- [ ] Architecture Approval
- [ ] Security Approval
- [ ] Published in service directory
- [ ] API documentation available
- [ ] Runbook available
- [ ] Playbook available
- [ ] Operations team capacity
- [ ] Game day plan and readiness

&nbsp;

---

## Security

This section covers the architectural and runtime security aspects of a given service.

Assumption: Shift-left of code related security control and scan enforcement is part of the best practices.

### Exposure

- [ ] External service
  - [ ] Is it part of API Gateway?
- [ ] Internal service
  - [ ] Is it part of a Service Mesh?
- [ ] Meets Governance and Compliance requirements.
- [ ] Risks identified.

### Secure Operations

- [ ] Identify/Establish process for continuous security vulnerability monitoring, updates and remediation.
- [ ] Identify Threat Model.
- [ ] Identify any trade-offs, risks vs. benefits.
- [ ] Use InfoSec approved hardened container images.
- [ ] Install any required security tools agents/sidecars.

### IAM, Authentication and Authorization

- [ ] Identify service owners and level of required accesses.
- [ ] Use least-privilege permissions in access roles and policies.
- [ ] Isolate resources by namespaces (In Kubernetes terms) and allow least privilege access to service accounts and roles.
- [ ] Implement authentication methods like Mutual TLS, JWT.
- [ ] Use ACLs as necessary.
- [ ] Use short-lived credentials and certificates.
- [ ] Implement credentials and certificate rotation.

### Data-In-Transit

- [ ] Use encryption protocols and methods for synchronous service-to-service communication.
- [ ] Use encryption protocols and methods for asynchronous communication via middleware, event hubs, etc.
- [ ] Use network level policies to allow/deny service-to-service communication.

### Data at rest

- [ ] Use server-side encryption of storage media/service, filesystem.
- [ ] Use client side data/field level encryption if needed.
- [ ] Enable encryption for all the cloud services being used. For ex. - Database, Queue, Topics, Cache.
- [ ] Implement additional controls to handle sensitive data as per organizational requirements.
- [ ] Store secrets and sensitive configuration data in a separate secrets store. And, never in a code repository.
- [ ] Enforce strict least-privilege policies and processes to allow access to the secrets store.
- [ ] Ensure that the data backup and restore mechanism follows the data encryption and access policies as per organizational requirements.

&nbsp;

---

## Reliability

Focus of this area is to ensure better runtime reliability of the services and their dependencies. Platform-level reliability is beyond the scope of this framework.

### Service Quotas

- [ ] Review cloud service limits. Request soft-limit quota increase if needed. Hard limits need to be considered as part of the architecture.
- [ ] Know and/or build the cloud service limit monitoring, alerting, and remediation mechanism.

### Network Reliability

- [ ] Leverage highly available network connectivity. Use cloud provider network backbone wherever possible.
- [ ] All required infrastructure-level network policies/rules are configured (DNS, Firewalls, etc.)
- [ ] All required service-level network policies/rules are configured (Service Mesh, Kubernetes Networking, etc.)
- [ ] Service communication failure handling mechanism is implemented. For ex: Throttling, controlled retries, timeouts, exponential backoffs, jitters.
- [ ] Tests conducted for network failures.

### Request Processing

- [ ] Sync vs. Async/Event-based processing.

### Reliable Deployment

Refer ["Deployment/Rollback Automation"](#Deployment/Rollback-Automation) section under "Operational Excellence".

### Scaling

- [ ] Service level auto-scaling policies implemented.
- [ ] No or Least service-to-service coupling by avoiding sync call dependencies.

### Data Backup and Recovery, Disaster Recovery

- [ ] Data backup and recovery plan and process in place
- [ ] Backups are encrypted
- [ ] Plan to validate the restore process
- [ ] RTO and RPO defined.
- [ ] DR strategy in place. (Note: This will also impact the architecture and deployment requirements.)

### Blast Radius

- [ ] Multi location deployment (Regions, Availability Zones, etc.).
- [ ] Services to run on multiple nodes/machines/servers spread across multiple locations.
- [ ] Automated deployment process to quickly deploy across regions/datacenters as needed.
- [ ] Graceful handling of component level failures. For ex: A storage service or a queue becomes temporarily unavailable.
- [ ] Provision capacity for fault tolerance.
- [ ] Multiple replicas.

### Testing

- [ ] Unit tests in place.
- [ ] Integration tests in place.
- [ ] Benchmark tests in place.
- [ ] Smoke tests in place.
- [ ] Load/Performance tests in place.

&nbsp;

---

## Performance Efficiency

### Identify Traffic/Load Patterns

_The data points collected here can help to decide the right architecture, resource and scaling configurations._

- [ ] Monthly/Daily Active Users
- [ ] Transactions/Requests per sec/min/hour/day etc.
- [ ] Any other traffic patterns
- [ ] Request/Response sizes
- [ ] Data volume requirements

### Resource Configuration Requirements

- [ ] Optimal CPU, Memory, Storage configurations
- [ ] Choose appropriate storage type
- [ ] Choose appropriate databases
- [ ] Is any particular Node Affinity (in Kubernetes terms) required to meet any underlying hardware requirements?

### Performance Monitoring

- [ ] Configure APM metrics and traces (Observability).
- [ ] Configure Synthetics.
- [ ] Configure alarms for error rates, p95, p99 etc.

### Granularity

- [ ] Is the service granular enough to meet performance and scalability requirements based on the traffic patterns (For ex: Adjustable to read vs. write load requirements.)
- [ ] Does the service need to be more granular?

### Testing and Benchmarks

- [ ] Any benchmark comparisons required/done against existing services to determine possible performance results.
- [ ] Network Latency considered.
- [ ] Benchmark testing meets requirements.
- [ ] Load/Performance testing meets requirements.

&nbsp;

---

## Cost Optimization

Microservices by themselves may not seem to incur a lot of runtime cost in the beginning, but, it can quickly add up at scale if some of the below aspects are overlooked.

### Consider The Dependencies

- [ ] Choose cost efficient services and architecture.
- [ ] Calculate runtime cost of all dependent services (including data transfer cost) specific to the service in context.
- [ ] Consider license cost.

### Resource Identification

- [ ] Add required Tags and Labels.
- [ ] Meet requirements for Showback/Chargeback.
- [ ] Plan to continuously identify and optimize resource utilization.
- [ ] Process to identify inactive/abandoned services and decommission them.

### Guardrails

- [ ] Cluster level policies enforced to restrict resource usage beyond permissible limits.
- [ ] Identify and enforce min/max resource limits.
- [ ] Exception process in place.

### Development & Build Environments

- [ ] Utilize local development and testing processes wherever possible.

&nbsp;

---

## Sustainability

Sustainability in terms of microservices can be achieved through continuous effort to make the services run efficiently and sufficiently to meet the SLA, SLO requirements.

Continuous cost optimization and resource utilization improvement plays a key role as part of this effort.

- [ ] Configure CPU/Memory/Storage capacity to start with the minimum numbers to meet the request/response/processing requirements.
- [ ] Using lifecycle policies and automation to decommission unwanted resources and data.
- [ ] Avoid over-provisioning beyond the fault-tolerance requirements.
- [ ] Use shared and managed services wherever possible.

---

## Testing Strategy

A strong testing foundation ensures services are reliable, maintainable, and can evolve safely over time.

### General Guidelines
- [ ] Is there a testing strategy document that outlines the scope and approach?
- [ ] Are testing responsibilities clearly defined (developers, QA, DevOps)?
- [ ] Is test automation integrated into the CI/CD pipeline?

### Unit Testing
- [ ] Are all business logic units covered with automated unit tests?
- [ ] Is code coverage tracked and maintained at a healthy threshold?
- [ ] Are mocks/stubs used where appropriate to isolate units?

### Integration Testing
- [ ] Are key interactions between services tested through integration tests?
- [ ] Is a dedicated test environment available that mirrors production setup?
- [ ] Are dependencies (e.g., databases, queues) tested with real or containerized instances?

### Contract Testing
- [ ] Are consumer-driven contract tests in place for inter-service APIs (e.g., using Pact)?
- [ ] Are versioning and backward compatibility tested for all APIs?
- [ ] Is contract testing automated and part of the CI/CD process?

### End-to-End (E2E) Testing
- [ ] Are critical business workflows covered by E2E tests?
- [ ] Are synthetic transactions used to validate production health?
- [ ] Are E2E tests run on realistic test data?

### Regression & Load Testing
- [ ] Are regression test suites updated regularly?
- [ ] Are load tests conducted to validate performance under stress?
- [ ] Are tools like JMeter, k6, or Locust used for load/performance tests?

### Chaos & Resilience Testing
- [ ] Is chaos engineering practiced to test system resilience (e.g., using Chaos Mesh, Gremlin)?
- [ ] Are failure injection mechanisms in place to simulate network or service failures?

### Test Observability
- [ ] Are test results visible in CI dashboards?
- [ ] Are flaky tests tracked and resolved promptly?
- [ ] Are logs from failed tests stored for post-mortem analysis?

---

## Data Management & Consistency

Proper handling of data is critical in a distributed architecture. This section ensures each microservice owns its data and consistency is thoughtfully addressed.

### General Principles
- [ ] Does each microservice own its own database (Database per Service pattern)?
- [ ] Are direct cross-service database calls avoided?
- [ ] Is data modeling aligned with the service’s Bounded Context?

### Data Consistency
- [ ] Is eventual consistency clearly acknowledged and handled where needed?
- [ ] Are strategies like **SAGA Pattern** or **Outbox Pattern** used to manage distributed transactions?
- [ ] Are compensating actions defined for long-running or failed transactions?

### Data Sharing
- [ ] Are services using asynchronous communication (e.g., events) for sharing state changes?
- [ ] Is there a strategy for publishing domain events (e.g., using message brokers like Kafka, RabbitMQ)?
- [ ] Are subscribers idempotent and resilient to duplicate events?

### Read Models & Querying
- [ ] Is **CQRS** (Command Query Responsibility Segregation) applied when needed?
- [ ] Are read models updated using event-driven mechanisms?
- [ ] Are materialized views or projections used to optimize reads?

### Data Ownership & Retention
- [ ] Are data retention policies defined and enforced for each service?
- [ ] Are archival and deletion strategies compliant with business and legal requirements (e.g., GDPR)?

### Data Security & Access
- [ ] Are sensitive fields encrypted at rest and in transit?
- [ ] Are row- or field-level access controls implemented where necessary?
- [ ] Are secrets (e.g., DB credentials, keys) stored securely (e.g., HashiCorp Vault, AWS Secrets Manager)?

### Schema Management
- [ ] Are schema migrations managed using version-controlled tools (e.g., Flyway, Liquibase)?
- [ ] Are schema changes backward-compatible and tested?
- [ ] Is there a rollback plan in place for failed data migrations?
