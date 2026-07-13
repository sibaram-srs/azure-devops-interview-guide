## Dt. 13-07-26 #DI

# NAB & Quess DevOps Interview Questions – Interview Ready Answers

---

# 1. How would you design a production-grade CI/CD pipeline from scratch?

Pipeline Flow:

```text
Developer
   │
Git Push
   │
PR Validation
   │
Code Review
   │
Build
   │
Unit Test
   │
SonarQube Scan
   │
SAST Scan
   │
Dependency Scan
   │
Build Docker Image
   │
Trivy Scan
   │
Push Image to ACR/ECR
   │
Terraform Plan
   │
Approval
   │
Terraform Apply
   │
Deploy to AKS/EKS
   │
Smoke Test
   │
Synthetic Test
   │
Canary/Rolling Deployment
   │
Monitoring
   │
Notification
```

---

# 2. How would you design a CI/CD pipeline for Multi-Region AKS/EKS?

Deployment Order:

```text
Build Once

↓

Push Image

↓

Deploy Region-1

↓

Validation

↓

Approval (Optional)

↓

Deploy Region-2

↓

Validation

↓

Deploy Region-3

↓

Complete
```

Deploy one region at a time with automated validation before moving to the next.

---

# 3. How do you ensure deployment to the next region happens only after validation?

Configure stage dependencies in Azure DevOps/GitHub Actions/Jenkins.

```text
Region-1

↓

Smoke Test

↓

Health Check

↓

Synthetic Test

↓

If Success

↓

Deploy Region-2

Else

↓

Stop Pipeline
```

---

# 4. How would you automate deployment promotion?

- Stage Dependency
- Approval Gates
- Health Checks
- Synthetic Tests
- Canary Validation

Pipeline automatically promotes only after successful validation.

---

# 5. What is Synthetic Testing?

Synthetic Testing simulates real user requests after deployment.

Examples:

- Login API
- Payment API
- Home Page
- Search API

Purpose:

- Verify application health
- Validate user journey
- Detect failures before users

---

# 6. Production CI/CD Pipeline Stages

- Source Checkout
- Build
- Unit Test
- SonarQube
- SAST
- Dependency Scan
- Docker Build
- Image Scan
- Push Image
- Terraform Validate
- Terraform Plan
- Approval
- Terraform Apply
- Kubernetes Deployment
- Smoke Test
- Synthetic Test
- Monitoring Validation
- Notification

---

# 7. How do you automate deployment validation and rollback?

Validation:

- Health Probe
- Readiness Probe
- API Test
- Synthetic Test

If validation fails:

```text
Rollback Previous Version

↓

Notify Team

↓

Stop Deployment
```

---

# 8. How do you authenticate CI/CD Pipeline with Azure/AWS?

Azure

- Managed Identity
- Service Principal
- Workload Identity Federation

AWS

- IAM Role
- OIDC
- IAM User (Not Preferred)

---

# 9. How does authentication work between Git and Cloud?

```text
Git

↓

Pipeline

↓

OIDC / Service Principal

↓

Azure AD / IAM

↓

Temporary Token

↓

Deploy Resources
```

No passwords stored in the pipeline.

---

# 10. When do you use Managed Identity instead of Service Principal?

Managed Identity:

- Azure-hosted resources
- Automatic credential rotation
- No secret management

Service Principal:

- External CI/CD systems
- Cross-tenant access

Preferred: Managed Identity.

---

# 11. Kubernetes Deployment Strategies

- Rolling Update
- Recreate
- Blue-Green
- Canary
- A/B Testing

---

# 12. Rolling vs Canary vs Blue-Green

| Rolling | Canary | Blue-Green |
|----------|---------|------------|
| Gradual replacement | Small user percentage | Two environments |
| Low Cost | Medium Cost | High Cost |
| Easy rollback | Safe rollout | Instant rollback |

Most Cost Effective:

**Rolling Update**

Safest:

**Canary**

Fastest Rollback:

**Blue-Green**

---

# 13. Security Controls for Banking CI/CD

- MFA
- RBAC
- SonarQube
- Checkov
- tfsec
- Trivy
- Azure Key Vault
- Private Endpoints
- Manual Approval
- Signed Images
- HTTPS
- WAF
- Defender for Cloud

---

# 14. How do you secure secrets?

Never hardcode secrets.

Store them in:

- Azure Key Vault
- AWS Secrets Manager
- Kubernetes Secrets
- Managed Identity

---

# 15. Azure Key Vault Integration

```text
Pipeline

↓

Authenticate

↓

Read Secret

↓

Inject Runtime Variable

↓

Deployment
```

Secrets are never stored in code.

---

# 16. How do you prevent hardcoded secrets?

- Azure Key Vault
- Git Secret Scan
- Pre-Commit Hooks
- Gitleaks
- TruffleHog

---

# 17. How do you implement RBAC?

Assign minimum permissions.

Example:

Developer

↓

Contributor (Dev Only)

Pipeline

↓

Deployment Identity

Production

↓

Approval Required

---

# 18. Where do you integrate SonarQube?

After Build

Before Docker Build

Pipeline:

```text
Build

↓

SonarQube

↓

Quality Gate

↓

Continue
```

---

# 19. How do Private Endpoints improve security?

Private Endpoints:

- No Public Internet
- Private IP
- Secure Azure Backbone
- Reduced Attack Surface

---

# 20. Secure communication between Bastion and Production VM?

```text
User

↓

Azure Bastion

↓

Private IP

↓

Production VM
```

No Public IP.

Communication occurs over Azure private network.

---

# 21. Bastion Security Configuration

- MFA
- RBAC
- JIT Access
- NSG
- Private IP
- Logging
- Azure Monitor

---

# 22. Why should Production VMs not have Public IP?

- Reduces attack surface
- Prevents direct internet access
- Better compliance
- Secure through Bastion

---

# 23. Secure Bastion to Production Server

- Private IP
- NSG
- RBAC
- Bastion
- Azure Backbone Network

---

# 24. Networking Security for Production

- NSG
- Azure Firewall
- WAF
- Private Endpoint
- DDoS Protection
- Bastion
- VPN
- ExpressRoute

---

# 25. Migrate ClickOps to Terraform

Steps:

- Inventory Resources
- Import Resources
- Create Terraform Code
- Validate
- Plan
- Apply

---

# 26. Multi-Account Terraform Migration

Repository:

```text
modules/

environments/

dev/

qa/

prod/

account-1/

account-2/
```

Each account has its own backend and variables.

---

# 27. Reusable Terraform Modules

Modules:

- Network
- VM
- AKS
- Storage
- Key Vault
- Monitoring
- Load Balancer

Reusable using variables.

---

# 28. Root vs Child Modules

Root Module

Calls Child Modules.

Child Modules

Contain reusable resources.

```text
Root

↓

Network Module

↓

AKS Module

↓

Storage Module
```

---

# 29. Terraform Import Block

Purpose:

Manage existing resources with Terraform.

Command:

```bash
terraform import azurerm_resource_group.rg /subscriptions/...
```

Terraform Import Block (Terraform 1.5+) allows imports to be defined in code.

---

# 30. Infrastructure Migration with Minimal Downtime

- Blue-Green
- Rolling Deployment
- DNS Switch
- Load Balancer
- Data Replication

---

# 31. Terraform State Locking

Prevents multiple users from modifying state simultaneously.

Unlock:

```bash
terraform force-unlock LOCK_ID
```

---

# 32. Unlock State in Terraform Enterprise

Use:

Terraform Enterprise UI

or

Terraform Enterprise API

No local Terraform required.

---

# 33. Terraform State in Terraform Enterprise

- Remote State
- Versioned
- Locked
- Encrypted
- RBAC Protected

---

# 34. Production Observability Solution

Use:

- Azure Monitor
- Log Analytics
- Prometheus
- Grafana
- Application Insights

Monitor:

- Metrics
- Logs
- Traces

---

# 35. Monitoring Dashboards

- Infrastructure Dashboard
- Application Dashboard
- Latency Dashboard
- Error Dashboard
- Kubernetes Dashboard
- Database Dashboard

---

# 36. 30% Timeout Scenario

Dashboards:

- API Response Time
- HTTP Errors
- CPU
- Memory
- Pod Restart
- Network

Alerts:

- Latency > 500ms
- Error Rate > 5%
- Timeout >10%

---

# 37. Alerting Strategy

Critical

↓

PagerDuty

↓

Teams

↓

Email

↓

SMS

Different severity levels.

---

# 38. Threshold Monitoring

Examples:

CPU >80%

Memory >85%

Disk >75%

Latency >500ms

Availability <99%

---

# 39. KPIs

- CPU
- Memory
- Disk
- Latency
- Availability
- Error Rate
- Throughput
- Pod Restart
- Request Count

---

# 40. Latency Dashboard

Include:

- P50
- P95
- P99
- Response Time
- API Duration
- Database Query Time

---

# 41. Error Dashboard

Include:

- HTTP 4xx
- HTTP 5xx
- Exceptions
- Failed Requests
- Timeout Count

---

# 42. What is Latency?

Latency is the time taken for a request to travel from client to server and receive a response.

---

# 43. P50 vs P95 vs P99

| Metric | Meaning |
|----------|---------|
| P50 | Median response time |
| P95 | 95% requests complete within this time |
| P99 | 99% requests complete within this time |

Production Monitoring mainly uses **P95 and P99**.

---

# 44. AWS to Azure Migration

Steps:

- Assessment
- Resource Mapping
- Terraform
- Database Migration
- Application Migration
- Validation
- Cutover

---

# 45. Multi-Cloud Management

Tools:

- Terraform
- Kubernetes
- GitHub Actions
- Azure DevOps
- Prometheus
- Grafana

Standardize Infrastructure as Code across clouds.

---

# 46. Secure Banking Cloud Architecture

- Azure Landing Zone
- Hub-Spoke Network
- Private Endpoints
- Azure Firewall
- WAF
- DDoS
- Key Vault
- RBAC
- MFA
- Azure Policy
- Defender for Cloud

---

# 47. Highly Available Banking CI/CD Pipeline

Features:

- Multi-Region Deployment
- Manual Approval
- Rolling Deployment
- Canary Validation
- Automated Rollback
- Monitoring
- Notification

---

# 48. Zero Downtime Deployment

Use:

- Rolling Update
- Blue-Green
- Canary
- Readiness Probes
- Load Balancer

---

# 49. How do you verify deployment success?

System Validation:

- Pod Running
- Health Probe
- Logs
- Metrics

User Validation:

- Synthetic Test
- Login Test
- API Test
- UI Validation

---

# 50. Secure Communication Between User, Bastion, and Production

```text
User

↓

VPN / ExpressRoute

↓

Azure Bastion

↓

Private Network

↓

Production VM
```

Protected by RBAC, MFA, NSGs, and private IPs.

---

# 51. Standardize Terraform Across 20+ Accounts

- Reusable Modules
- Git Repository
- Version Control
- Terraform Enterprise
- Remote State
- Code Review
- CI/CD
- Policy as Code (Sentinel/Azure Policy)

---

# 52. Monitor Health and Notify Operations Team

Monitoring Stack:

- Azure Monitor
- Prometheus
- Grafana
- Application Insights

Alerts:

- Azure Monitor Alerts
- Action Groups
- Microsoft Teams
- Email
- PagerDuty
- ServiceNow

Flow:

```text
Application

↓

Metrics & Logs

↓

Azure Monitor / Prometheus

↓

Alert Rules

↓

Action Group

↓

Teams / Email / PagerDuty

↓

Operations Team
```
