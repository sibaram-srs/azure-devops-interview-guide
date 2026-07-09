# TDICS

# Senior Azure DevOps Engineer (10+ Years Experience) – Interview Ready Answers

---

# 1. Brief me about yourself, your skill set, and your current project.

"I have over 10 years of experience as a DevOps and Cloud Engineer, specializing in Azure Cloud, Azure DevOps, Terraform, Kubernetes, Docker, Azure Landing Zones, Infrastructure as Code, and CI/CD automation.

My expertise includes designing enterprise-scale Azure Landing Zones, automating infrastructure using Terraform, building multi-stage Azure DevOps pipelines, implementing GitOps with Helm and AKS, securing environments using Azure Key Vault, Managed Identity, RBAC, and Azure Policies.

In my current project, I'm responsible for designing and maintaining a multi-region Azure platform for multiple business applications. My responsibilities include Infrastructure as Code, CI/CD, Azure Networking, Monitoring, Disaster Recovery, Security, Cost Optimization, and Production Support."

---

# 2. Explain your recent project end-to-end.

"Our application is hosted on Azure using a Hub-Spoke Landing Zone architecture.

Developers push code to Azure Repos.

Azure DevOps pipeline performs:
- Build
- Unit Testing
- SonarQube Scan
- Security Scan
- Docker Image Build
- Push Image to Azure Container Registry

Terraform provisions infrastructure.

Helm deploys applications to AKS.

Secrets are fetched from Azure Key Vault.

Application Gateway routes external traffic.

Azure Firewall secures outbound traffic.

Monitoring is done using Azure Monitor, Log Analytics, Prometheus and Grafana.

Application Insights monitors application performance.

Production deployments follow Blue-Green deployment with approval gates."

---

# 3. How would you design an Azure Landing Zone from scratch?

I follow Microsoft's Cloud Adoption Framework.

Steps:

- Create Management Groups
- Create Separate Subscriptions
- Configure Azure AD
- Implement RBAC
- Create Hub VNet
- Create Spoke VNets
- Configure Azure Firewall
- Configure Private DNS
- Configure Azure Policies
- Configure Log Analytics
- Configure Defender for Cloud
- Configure Backup
- Configure Monitoring
- Enable Cost Management
- Configure Terraform Remote Backend

---

# 4. Explain Azure Landing Zone step by step.

1. Management Groups
2. Azure AD Integration
3. Subscription Design
4. Resource Organization
5. Hub Network
6. Spoke Networks
7. Firewall
8. Route Tables
9. NSGs
10. Private Endpoints
11. Monitoring
12. Security Policies
13. Backup
14. CI/CD Integration

---

# 5. Types of Azure Landing Zones

- Enterprise Scale Landing Zone
- Platform Landing Zone
- Application Landing Zone
- Sandbox Landing Zone

Enterprise Scale is recommended for large organizations.

---

# 6. Why is Azure Landing Zone important?

- Standardization
- Governance
- Security
- Scalability
- Compliance
- Cost Control
- Automation
- High Availability
- Disaster Recovery

---

# 7. Explain Hub-and-Spoke Architecture.

Hub contains shared services:

- Azure Firewall
- VPN Gateway
- ExpressRoute
- Bastion
- DNS
- Shared Services

Spokes contain application workloads.

Benefits:

- Centralized Security
- Lower Cost
- Better Isolation
- Easier Management

---

# 8. Multi-Region Azure Landing Zone

Deploy identical infrastructure in multiple Azure regions.

Example:

Primary:
East US

Secondary:
Central US

Components replicated:

- AKS
- App Gateway
- Key Vault
- SQL
- Storage
- Monitoring

Traffic Manager or Front Door handles failover.

---

# 9. Identity, Networking and DR in Multi-Region

Identity

- Azure AD
- Managed Identity
- RBAC

Networking

- Hub-Spoke
- VNet Peering
- Private Endpoints

DR

- Geo-redundant Storage
- Azure Site Recovery
- SQL Failover Groups
- Front Door Failover

---

# 10. Where do you associate UDRs?

UDRs are associated with Subnets.

Typically:

- AKS subnet
- VM subnet
- Application subnet

Never associate directly to VNet.

---

# 11. Azure Firewall

Centralized firewall deployed inside Hub VNet.

Features:

- DNAT
- SNAT
- Network Rules
- Application Rules
- Threat Intelligence
- Logging

---

# 12. How do you control traffic flow?

Using:

- NSG
- UDR
- Azure Firewall
- Application Gateway
- Private Endpoints

---

# 13. Ensure all traffic passes Azure Firewall

Create UDR.

Default Route:

```
0.0.0.0/0
```

Next Hop:

Azure Firewall Private IP

Associate Route Table to workload subnet.

---

# 14. Multi-region deployment experience

Yes.

Used Azure DevOps multi-stage pipelines.

Infrastructure deployed using Terraform modules.

Applications deployed using Helm.

Traffic switched using Azure Front Door.

---

# 15. Explain Multi-region deployment

Infrastructure

↓

Terraform

↓

AKS Cluster

↓

Helm Deployment

↓

Health Check

↓

Front Door Failover

---

# 16. Deployment successful but application unavailable

Check:

- Pods
- Logs
- App Gateway
- DNS
- NSG
- Firewall
- Health Probe
- Database Connectivity
- Key Vault Access

---

# 17. Rollback Strategy

Preferred:

Blue-Green

Alternative:

Helm Rollback

```bash
helm rollback
```

Terraform rollback only for infrastructure.

---

# 18. Communication during outage

- Create Incident Bridge
- Notify Stakeholders
- Provide ETA
- Update every 15 minutes
- RCA after restoration

---

# 19. Real-time rollback example

Deployment caused CrashLoopBackOff.

Executed:

```bash
helm rollback app 15
```

Application restored within 5 minutes.

Root cause fixed later.

---

# 20. Scripting experience

PowerShell

Bash

Python

Used for:

- VM Automation
- Storage
- AKS
- Backup
- Azure DevOps

---

# 21. Automation example

PowerShell script automatically:

- Creates Resource Group
- Creates Storage
- Creates Key Vault
- Assigns RBAC
- Uploads Secrets

Reduced provisioning from 30 minutes to 5 minutes.

---

# 22. Azure DevOps automation

PowerShell script:

- Reads Variable Group
- Validates Naming
- Creates Resources
- Stores Secrets
- Sends Teams Notification

---

# 23. PowerShell experience

Experience with:

- Az Module
- Azure CLI
- ARM
- REST API
- Azure DevOps API

---

# 24. RBAC scenario

Pipeline Service Principal:

Contributor

Application Team:

Reader

Security Team:

Key Vault Administrator

Least Privilege followed.

---

# 25. RBAC in Pipeline

Use:

Service Connection

Assign only required roles.

Never Owner.

---

# 26. RBAC prevents failures

Proper permissions avoid:

- Unauthorized deployments
- Manual changes
- Accidental deletion

---

# 27. RBAC Strategy

Separate access:

Developers

QA

Operations

Security

Production access only through approval.

---

# 28. Multi-stage pipeline

Stages:

Build

↓

Test

↓

Security Scan

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Deploy

↓

Smoke Test

↓

Production

---

# 29. SAST and DAST

SAST

- SonarQube
- Checkmarx

DAST

- OWASP ZAP

Integrated before Production deployment.

---

# 30. Monitoring tools

- Azure Monitor
- Log Analytics
- Grafana
- Prometheus
- Application Insights
- Azure Alerts

---

# 31. Current monitoring

Azure Monitor

Application Insights

Grafana

Prometheus

---

# 32. KPIs

- CPU >80%
- Memory >80%
- Disk >75%
- Pod Restart
- HTTP 5xx
- Availability
- Response Time
- Failed Deployments

---

# 33. Dev works QA fails

Check:

Variables

Key Vault

Firewall

DNS

Configuration

Terraform State

RBAC

---

# 34. Compare configurations

Terraform Variables

Git

Azure App Configuration

Variable Groups

---

# 35. Standardize environments

Reusable Terraform Modules

Separate tfvars

Same Pipeline

Different Variables

---

# 36. Monitor Terraform failures

Azure DevOps Logs

Terraform Exit Code

Azure Activity Logs

Log Analytics

Alerts

---

# 37. Terraform failure example

Issue:

Storage Account already existed.

Solution:

Imported existing resource.

```bash
terraform import
```

---

# 38. Terraform challenges

- State Conflict
- Provider Upgrade
- API Changes
- Drift
- Locking Issues

---

# 39. Terraform across environments

Separate:

Backend

tfvars

State File

Pipeline

Workspaces if required.

---

# 40. Terraform Drift

Manual Portal changes.

Detected using:

```bash
terraform plan
```

Resolved by:

Import or Apply.

---

# 41. Pipeline succeeded but resource missing

Possible reasons:

- Wrong Subscription
- Wrong Service Connection
- Wrong Resource Group
- Conditional Skip
- Terraform Target
- No Changes

---

# 42. Troubleshooting

Check:

Pipeline Logs

Terraform Output

Azure Activity Logs

Subscription

Provider Authentication

---

# 43. Two engineers run Apply

Without locking:

State corruption.

With Remote Backend:

Second deployment waits.

---

# 44. Terraform State Locking

Remote backend creates exclusive lock.

Only one Apply at a time.

---

# 45. terraform force-unlock

Use only when lock remains after unexpected failure.

Always verify no active deployment exists.

---

# 46. Azure Key Vault integration

Azure DevOps

↓

Service Connection

↓

Managed Identity / Service Principal

↓

Key Vault

↓

Pipeline Variables

---

# 47. Key Vault behind Private Endpoint

Use:

- Self-hosted Agent
- Private DNS
- VNet Integration

Microsoft Hosted Agent cannot access private resources.

---

# 48. Restrict App Service traffic

- Access Restrictions
- Private Endpoint
- NSG
- WAF
- App Gateway

---

# 49. Azure manages automatically in App Service

- OS
- Runtime
- Patch Management
- Scaling
- Load Balancer
- Infrastructure
- Availability

Customer manages:

Application

Configuration

Data

---

# 50. New Developer onboarding

Provide:

- Azure AD Group
- Repository Access
- Reader Role
- Dev Subscription Access
- Key Vault Read (if required)
- Azure DevOps Project Access

---

# 51. Configure developer tools

Install:

- VS Code
- Git
- Azure CLI
- Terraform
- kubectl
- Helm
- Docker Desktop
- PowerShell
- Azure DevOps Extension

---

# 52. Managed Identity vs Service Principal

| Managed Identity | Service Principal |
|-----------------|------------------|
| Azure Managed | User Managed |
| No Secret | Secret/Certificate Required |
| Automatic Rotation | Manual Rotation |
| More Secure | External Access Supported |

Use Managed Identity whenever Azure resources communicate with Azure resources.

---

# 53. docker build vs docker commit

docker build

- Uses Dockerfile
- Repeatable
- Version Controlled
- Recommended

docker commit

- Creates image from running container
- Manual
- Not Repeatable
- Not recommended for production

**Interview Answer:** "In production, we always use `docker build` with a version-controlled Dockerfile. `docker commit` is mainly useful for debugging or creating a quick snapshot and should not be part of a CI/CD pipeline."
