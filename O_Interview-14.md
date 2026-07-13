#  Platform Engineering Interview Questions – Interview Ready Answers

---

# 1. How will you migrate a Monolithic Application to Microservices?

### Approach

1. Analyze the monolithic application and identify business domains.
2. Break the application into independent microservices using Domain-Driven Design (DDD).
3. Containerize each service using Docker.
4. Deploy services to AKS/Kubernetes.
5. Implement API Gateway for routing.
6. Use asynchronous communication (Kafka/Service Bus) where required.
7. Create independent CI/CD pipelines for each service.
8. Monitor all services using Prometheus and Grafana.
9. Gradually migrate modules using the Strangler Fig Pattern.
10. Decommission the monolith after complete migration.

**Architecture**

```text
Client
   │
API Gateway
   │
──────────────────────────
│ User Service           │
│ Order Service          │
│ Payment Service        │
│ Notification Service   │
──────────────────────────
        │
 AKS / Kubernetes
        │
 Azure SQL / Cosmos DB
```

---

# 2. How will you design a Platform for 50 Applications?

### My Approach

- Azure Landing Zone
- Hub-Spoke Network
- Shared AKS Cluster (or multiple clusters based on workload)
- Internal Developer Platform (IDP)
- Shared CI/CD Templates
- Shared Terraform Modules
- Centralized Monitoring
- Azure Key Vault
- RBAC
- GitOps Deployment

### Shared Platform Services

- AKS
- Azure Container Registry
- Azure Key Vault
- Azure Monitor
- Log Analytics
- Prometheus
- Grafana
- Azure DevOps
- Terraform Modules

Developers only push code; the platform handles builds, deployments, security, and monitoring.

---

# 3. Have you designed an isolated environment with security controls?

**Yes.**

### Architecture

```text
Management Group

↓

Subscription

↓

Resource Group

↓

Virtual Network

↓

Frontend Subnet

↓

Backend Subnet

↓

Database Subnet

↓

Private Endpoint

↓

Azure Firewall

↓

Azure Bastion
```

### Security Controls

- Private Endpoints
- No Public IPs
- Azure Firewall
- NSGs
- Azure Policy
- RBAC
- Key Vault
- Managed Identity
- Defender for Cloud
- DDoS Protection
- Bastion Host
- Azure Monitor & Logging

---

# 4. How will you manage multiple environments using Terraform and CI/CD?

### Repository Structure

```text
terraform/

modules/

network/

aks/

storage/

environments/

dev/

qa/

uat/

prod/
```

Each environment has:

- Separate backend
- Separate tfvars
- Separate state file
- Separate pipeline

Pipeline Flow

```text
Git

↓

Build

↓

Terraform Validate

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Deploy Application
```

---

# 5. How will you set up an Internal Developer Platform (IDP)?

### Components

- Azure DevOps
- GitHub
- AKS
- Terraform
- Azure Key Vault
- Azure Container Registry
- Prometheus
- Grafana
- Backstage (Developer Portal)

### Developer Workflow

```text
Developer

↓

Backstage Portal

↓

Create Service

↓

Pipeline Trigger

↓

Infrastructure Provisioned

↓

Application Deployed

↓

Monitoring Enabled
```

Benefits

- Self-Service
- Standardization
- Faster Deployments
- Governance

---

# 6. How will you create a common dashboard for 50 applications?

Use:

- Prometheus
- Grafana
- Azure Monitor
- Application Insights

Dashboard Categories

- Infrastructure
- Application
- Kubernetes
- Database
- Business Metrics

Use Grafana variables to filter by:

- Application
- Namespace
- Environment
- Region

---

# 7. What metrics will you monitor?

### Infrastructure

- CPU
- Memory
- Disk
- Network
- VM Health

### Kubernetes

- Pod Status
- Node Status
- Pod Restart Count
- HPA
- Deployment Status

### Application

- Request Count
- Error Rate
- Response Time
- Availability
- Throughput

### Database

- Connections
- Query Time
- CPU
- Storage

### Tools

Metrics

- Prometheus
- Azure Monitor

Logs

- Log Analytics

Visualization

- Grafana

Tracing

- Application Insights / OpenTelemetry

---

# 8. Best Practices for Multiple Environments & CI/CD

- Separate subscriptions
- Separate state files
- Separate pipelines
- Reusable templates
- Terraform modules
- Approval gates
- Git branching strategy
- RBAC
- Key Vault
- Automated testing
- Canary/Rolling deployments
- Monitoring after deployment

---

# 9. How will you create a CI/CD pipeline from scratch?

Pipeline

```text
Developer Commit

↓

Build

↓

Unit Test

↓

SonarQube

↓

SAST Scan

↓

Docker Build

↓

Trivy Scan

↓

Push to ACR

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Deploy to AKS

↓

Smoke Test

↓

Synthetic Test

↓

Notification
```

---

# 10. How will you troubleshoot CI/CD pipeline logs?

### Steps

1. Check pipeline execution logs.
2. Identify failed stage.
3. Verify build logs.
4. Check Terraform logs.
5. Review Docker build logs.
6. Check Kubernetes deployment events.
7. Validate application logs.
8. Verify monitoring alerts.
9. Fix the issue and rerun the pipeline.

Useful Commands

```bash
kubectl logs <pod-name>
```

```bash
kubectl describe pod <pod-name>
```

```bash
terraform plan
```

---

# 11. How will you design and implement a centralized monitoring solution?

### Architecture

```text
Applications

↓

Prometheus

↓

Grafana

↓

Azure Monitor

↓

Log Analytics

↓

Application Insights

↓

Alert Manager

↓

Email

↓

Teams

↓

PagerDuty
```

### Components

Metrics

- Prometheus

Logs

- Log Analytics

Tracing

- Application Insights

Visualization

- Grafana

Alerting

- Azure Monitor Alerts
- Alertmanager

### Dashboards

- Infrastructure Dashboard
- Kubernetes Dashboard
- Application Dashboard
- Error Dashboard
- Latency Dashboard
- Database Dashboard
- Business Dashboard

---

# 12. How will you implement security for an AKS Cluster?

### Cluster Security

- Private AKS Cluster
- Azure AD Integration
- Kubernetes RBAC
- Azure RBAC
- Managed Identity
- Network Policies
- Private Endpoints
- Azure Firewall
- NSGs
- Pod Security Standards
- Image Scanning (Trivy)
- Azure Policy for AKS
- Secrets in Azure Key Vault (CSI Driver)
- Defender for Containers
- HTTPS/TLS
- Ingress with WAF
- Regular Patch Management
- Audit Logs enabled

### Secure Architecture

```text
Internet

↓

Azure Application Gateway (WAF)

↓

Private AKS Cluster

↓

Ingress Controller

↓

Microservices

↓

Azure Key Vault

↓

Azure SQL (Private Endpoint)

↓

Azure Monitor

↓

Microsoft Defender for Cloud
```

### Security Best Practices

- No Public API Server
- Disable Anonymous Access
- Least Privilege RBAC
- Use Managed Identity
- Scan Images Before Deployment
- Enable Network Policies
- Encrypt Secrets
- Enable Audit Logging
- Continuous Monitoring
- Regular Vulnerability Assessment

**Interview Closing Line:**

> "My approach is to build a secure, automated, reusable, and scalable platform where developers only focus on writing code, while the platform automatically handles infrastructure provisioning, security, deployments, monitoring, governance, and compliance."
