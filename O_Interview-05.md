# Azure DevOps / Terraform / Kubernetes / Monitoring Interview Questions & Expert Answers

---

# 1. What is your understanding of an Azure Landing Zone? How is it implemented, and what is its role?

An **Azure Landing Zone (ALZ)** is a **predefined, secure, scalable, and governed Azure environment** that provides the foundation for hosting enterprise workloads. It follows Microsoft's Cloud Adoption Framework (CAF) and establishes standardized governance, networking, security, identity, and operational practices.

## Role of a Landing Zone

- Standardizes Azure deployments
- Provides governance and compliance
- Implements enterprise networking
- Enforces security policies
- Supports multiple subscriptions
- Simplifies onboarding of new applications
- Enables scalability and automation

## High-Level Architecture

```
Management Group
       │
       ▼
Subscriptions
       │
       ▼
Landing Zone
       │
 ┌───────────────┐
 │ Governance    │
 │ Identity      │
 │ Networking    │
 │ Security      │
 │ Monitoring    │
 └───────────────┘
       │
       ▼
Application Workloads
```

---

# 2. What services, tools, and technologies are provisioned through a Landing Zone?

A typical Azure Landing Zone provisions:

## Governance

- Management Groups
- Azure Policy
- RBAC
- Resource Locks
- Resource Tagging

## Identity

- Microsoft Entra ID
- Managed Identities
- Privileged Identity Management (PIM)

## Networking

- Hub-and-Spoke Network
- Virtual Networks
- Subnets
- Azure Firewall
- NSGs
- Route Tables
- VPN Gateway
- ExpressRoute
- Private DNS Zones

## Security

- Microsoft Defender for Cloud
- Azure Key Vault
- Private Endpoints
- Azure Bastion
- DDoS Protection

## Monitoring

- Azure Monitor
- Log Analytics Workspace
- Application Insights
- Azure Alerts

## Shared Services

- Azure Container Registry
- Backup Vault
- Recovery Services Vault
- Storage Accounts

---

# 3. Describe the Landing Zone used in your current architecture.

Our Landing Zone follows a **Hub-and-Spoke architecture**.

```
Management Group
        │
        ▼
Production Subscription
        │
        ▼
Hub VNet
│
├── Azure Firewall
├── Azure Bastion
├── VPN Gateway
├── Private DNS
└── Log Analytics
        │
────────┼────────
        │
   Spoke VNets
        │
 ├── AKS
 ├── App Service
 ├── Azure SQL
 ├── Storage
 └── Key Vault
```

Key characteristics:

- Separate subscriptions for Dev, UAT, and Production.
- Centralized networking and security.
- Private Endpoints for PaaS services.
- Azure Policy enforcement.
- Monitoring through Azure Monitor and New Relic.
- Infrastructure provisioned using Terraform.

---

# 4. What do you understand by Terraform workflows?

A Terraform workflow defines the lifecycle of infrastructure deployment.

Typical workflow:

```
Write Code
      │
terraform fmt
      │
terraform validate
      │
terraform init
      │
terraform plan
      │
Approval
      │
terraform apply
      │
Verification
```

In CI/CD:

```
Git Commit
      │
Pipeline Trigger
      │
Terraform Validation
      │
Security Scan
      │
Terraform Plan
      │
Approval
      │
Terraform Apply
```

---

# 5. What is a Terraform state file?

The Terraform state file (`terraform.tfstate`) records the current state of infrastructure.

It stores:

- Resource IDs
- Resource attributes
- Resource dependencies
- Outputs
- Metadata

Terraform compares:

```
Desired State (Terraform Code)

vs

Current State (State File)
```

This comparison determines which resources need to be created, modified, or deleted.

### Best Practices

- Store state remotely.
- Enable state locking.
- Enable versioning.
- Encrypt the backend.
- Restrict access using RBAC.

---

# 6. Difference between Root Module and Child Module

| Root Module | Child Module |
|--------------|--------------|
| Entry point of the Terraform configuration | Reusable module called by the root module |
| Executes `terraform apply` | Cannot be executed independently |
| References child modules | Contains reusable infrastructure logic |

Example:

```
Root Module
│
├── Calls VNet Module
├── Calls AKS Module
└── Calls SQL Module
```

---

# 7. Have you used Terraform Cloud or Terraform OSS (Vanilla)?

In my projects, I primarily used **Terraform OSS (Vanilla)**.

Features used:

- Azure Storage backend
- Remote state
- State locking
- Azure DevOps integration
- Git-based version control

I also have knowledge of Terraform Cloud, including:

- Remote execution
- Workspaces
- Policy as Code (Sentinel)
- Variable management
- Team collaboration

---

# 8. Where do you maintain the Terraform state file?

We store Terraform state in an **Azure Storage Account**.

Configuration includes:

- Dedicated Storage Account
- Blob Container
- Versioning enabled
- Soft Delete enabled
- State locking
- RBAC-restricted access

Benefits:

- Shared state
- Secure storage
- Locking during deployments
- Version history

---

# 9. Have you created private Terraform modules?

Yes.

Examples of reusable modules:

- Virtual Network
- NSG
- AKS
- Azure SQL
- Key Vault
- Storage Account
- Application Gateway
- Azure Firewall
- Log Analytics
- Azure Monitor

Benefits:

- Code reuse
- Standardization
- Easier maintenance
- Consistent deployments

---

# 10. What major Azure resources have you provisioned using Terraform?

Resources include:

- Resource Groups
- Virtual Networks
- Subnets
- NSGs
- Route Tables
- Azure Firewall
- VPN Gateway
- Azure Bastion
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Azure SQL Database
- Azure Storage Accounts
- Azure Key Vault
- Azure Monitor
- Log Analytics Workspace
- Application Gateway
- Azure Front Door
- Recovery Services Vault
- Managed Identities
- Private Endpoints
- Private DNS Zones

---

# 11. What best practices do you follow while configuring Azure Firewall?

Best practices:

- Default deny inbound traffic.
- Allow only required ports and protocols.
- Use application rules where possible.
- Use network rules for non-HTTP traffic.
- Implement DNAT only when necessary.
- Enable threat intelligence mode.
- Centralize logging.
- Integrate with Log Analytics.
- Enable diagnostics.
- Follow least-privilege principles.
- Separate production and non-production rules.
- Review firewall policies regularly.

---

# 12. What is a Lifecycle Block in Terraform?

A lifecycle block controls how Terraform manages resources.

Example:

```terraform
lifecycle {
  prevent_destroy = true
}
```

Common options:

- `prevent_destroy`
- `ignore_changes`
- `create_before_destroy`
- `replace_triggered_by`

Example:

```terraform
lifecycle {
  ignore_changes = [
    tags
  ]
}
```

---

# 13. What are Terraform Meta-Arguments?

Meta-arguments control Terraform resource behavior.

Common meta-arguments:

- `count`
- `for_each`
- `depends_on`
- `provider`
- `lifecycle`

Example:

```terraform
resource "azurerm_resource_group" "rg" {
  for_each = var.resource_groups

  name     = each.key
  location = each.value
}
```

---

# 14. What Kubernetes experience do you have?

I have worked extensively with **Azure Kubernetes Service (AKS)**.

Responsibilities:

- Cluster provisioning
- Node Pools
- Deployments
- StatefulSets
- Services
- Ingress
- Helm Charts
- ConfigMaps
- Secrets
- Persistent Volumes
- HPA
- Cluster Autoscaler
- RBAC
- Network Policies
- Rolling and Blue-Green deployments
- Monitoring using Prometheus and Grafana

---

# 15. Users are receiving 503 errors from an application hosted on Kubernetes. How would you troubleshoot?

Troubleshooting steps:

### Step 1

Check pod status:

```bash
kubectl get pods
```

### Step 2

Check application logs:

```bash
kubectl logs <pod-name>
```

### Step 3

Verify services:

```bash
kubectl get svc
```

### Step 4

Check ingress configuration:

```bash
kubectl get ingress
```

### Step 5

Verify endpoints:

```bash
kubectl get endpoints
```

### Step 6

Check deployment status:

```bash
kubectl rollout status deployment/app
```

If Kubernetes resources appear healthy, investigate the Application Gateway.

---

## What should you check on Application Gateway?

- Backend Health
- Health Probe status
- Backend Pool configuration
- Listener configuration
- HTTP Settings
- SSL certificates
- Routing Rules
- WAF logs
- Access logs
- Diagnostics logs

A common cause of HTTP 503 is that the backend health probe reports all backend instances as **Unhealthy**.

---

# 16. What backend checks should you perform for a 503 error?

Verify:

- Pod readiness
- Liveness probes
- Service endpoints
- Application startup
- Database connectivity
- CPU and memory utilization
- Port configuration
- Firewall or NSG rules
- TLS configuration
- DNS resolution
- Container logs

---

# 17. Which CI/CD tool did you use?

Primary tool:

- Azure DevOps Pipelines

Also familiar with:

- GitHub Actions
- Jenkins

Azure DevOps handled:

- Build
- Testing
- Security scans
- Infrastructure deployment
- Application deployment
- Release approvals

---

# 18. Have you worked with GitHub Actions?

Yes.

Typical workflow:

```
Git Push
      │
GitHub Actions
      │
Build
      │
Unit Tests
      │
SonarQube
      │
Docker Build
      │
Trivy Scan
      │
Push to ACR
      │
Deploy to AKS
```

Benefits:

- Native GitHub integration
- YAML-based workflows
- Marketplace actions
- Easy automation

---

# 19. Explain your CI/CD pipeline with security checks.

```
Developer Commit
        │
Pull Request
        │
Build
        │
Unit Tests
        │
SonarQube (SAST)
        │
Dependency Scan
        │
Terraform Validate
        │
tfsec / Checkov
        │
Docker Build
        │
Trivy Scan
        │
Push Image to ACR
        │
Deploy to AKS
        │
Smoke Tests
        │
OWASP ZAP (DAST)
        │
Approval
        │
Production Deployment
```

Security checks include:

- SAST
- Dependency Scanning
- IaC Scanning
- Container Scanning
- DAST
- Quality Gates

---

# 20. Have you worked with monitoring tools?

Yes.

Monitoring tools used:

- New Relic
- Azure Monitor
- Log Analytics Workspace
- Prometheus
- Grafana
- Azure Application Insights

These tools monitor:

- Infrastructure
- Applications
- Kubernetes
- Logs
- Alerts
- Performance
- Availability

---

# 21. Do you know how to implement a monitoring solution end-to-end?

Yes.

Typical implementation:

```
Application
      │
Monitoring Agent
      │
Metrics
Logs
Traces
      │
Monitoring Platform
      │
Dashboards
Alerts
Reports
```

Steps:

1. Install monitoring agent.
2. Configure application instrumentation.
3. Enable log forwarding.
4. Create dashboards.
5. Configure alerts.
6. Validate metrics.
7. Integrate with notification channels.

---

# 22. How does New Relic (or another monitoring tool) access application logs?

Logs are collected using agents.

Common methods:

- New Relic Infrastructure Agent
- Fluent Bit
- Azure Monitor Agent
- Log Forwarder

Flow:

```
Application
      │
Application Log
      │
Agent
      │
New Relic
```

For Kubernetes:

```
Pods
      │
Fluent Bit DaemonSet
      │
New Relic Log API
```

---

# 23. Where are logs stored so that someone can view logs from the past three days?

Logs are indexed and stored within the monitoring platform.

Examples:

- New Relic Logs
- Azure Log Analytics Workspace
- Elasticsearch (ELK)

Retention depends on the configured policy (e.g., 30, 90, or 365 days).

When a user searches for logs from three days ago, the monitoring platform queries its indexed log storage.

---

# 24. How are logs stored, and what is that storage mechanism called?

The log lifecycle typically looks like this:

```
Application
      │
Generates Logs
      │
Collection Agent
      │
Log Pipeline
      │
Log Parsing
      │
Indexing
      │
Storage
      │
Search & Dashboards
```

Key concepts:

- **Log Collection** – Gathering logs from applications and infrastructure.
- **Log Parsing** – Structuring raw log data.
- **Log Indexing** – Creating searchable indexes for fast retrieval.
- **Log Retention** – Defining how long logs are stored.
- **Log Archiving** – Moving older logs to lower-cost storage.

In New Relic, Azure Monitor, and ELK, logs are **indexed** and stored in a searchable backend. The indexed data enables fast querying, dashboards, alerts, and historical analysis.
