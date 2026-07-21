# Azure DevOps Interview – Expert DevOps Answers

---

# 1. Introduce yourself and explain your DevOps journey.

I am a DevOps Engineer with around **4+ years of experience** in cloud infrastructure, CI/CD automation, Infrastructure as Code, and DevSecOps.

I started my career working on infrastructure and system administration activities like Linux, networking, VM management, and deployments. Gradually, I moved towards automation by learning scripting, CI/CD tools, cloud platforms, and Infrastructure as Code.

Currently, I work on:

- Azure Cloud
- Azure DevOps
- Terraform
- Kubernetes
- Docker
- Jenkins/GitHub Actions
- CI/CD automation
- Security scanning
- Monitoring and troubleshooting

My focus is on building scalable, secure, and automated cloud environments.

---

# 2. How did you move from infrastructure to DevOps?

Initially, my role was more infrastructure-focused:

- Server provisioning
- OS configuration
- Network troubleshooting
- Application deployments

I started automating repetitive tasks using:

- Shell scripting
- Python
- Ansible

Then I moved into:

- CI/CD pipeline creation
- Cloud automation
- Terraform
- Containerization
- Kubernetes

This transition helped me move from manual infrastructure management to automated DevOps practices.

---

# 3. Explain one Azure DevOps CI/CD pipeline from code commit till production deployment.

Flow:

```
Developer
   |
   |
Git Repository
   |
   |
Azure DevOps Pipeline Trigger
   |
   |
Build Pipeline
   |
   |
Checkout Code
   |
   |
Restore Dependencies
   |
   |
Compile Application
   |
   |
Unit Testing
   |
   |
SonarQube Scan
   |
   |
Create Artifact
   |
   |
Publish Artifact
   |
   |
Release Pipeline
   |
   |
Deploy to Azure PaaS/AKS
   |
   |
Smoke Testing
   |
   |
Production
```

Stages:

1. Developer pushes code.
2. Pipeline triggers automatically.
3. Application is built and tested.
4. Security scans are executed.
5. Artifact is generated.
6. Artifact is deployed to Dev/Test/Prod.
7. Approval gates are applied before production.

---

# 4. Production deployment failed. How will you identify build vs release issue?

## Identify Build Issue

Check:

- Build logs
- Compilation errors
- Dependency failures
- Unit test failures
- SonarQube failures
- Artifact generation

Example:

```
Build Stage Failed
```

means issue is before deployment.

---

## Identify Release Issue

Check:

- Deployment logs
- Azure activity logs
- Service connection
- Permissions
- Configuration
- Environment variables
- Infrastructure status

Example:

```
Artifact Created Successfully
Deployment Failed
```

means release issue.

---

## How do you check logs?

Sources:

- Azure DevOps Pipeline Logs
- Application Logs
- Azure Monitor
- Log Analytics
- Application Insights
- Kubernetes Logs

Commands:

```bash
kubectl logs pod-name

az webapp log tail

terraform show
```

---

## How do you perform rollback?

Options:

### Application Rollback

Deploy previous stable artifact.

### Azure App Service

Use deployment slots:

```
Production Slot
        |
        Swap Back
        |
Previous Version
```

### Kubernetes

```bash
kubectl rollout undo deployment app-name
```

### Infrastructure

Restore previous Terraform configuration/state.

---

# 5. Explain one Infrastructure Pipeline using Azure DevOps + Terraform.

Flow:

```
Developer
    |
Terraform Code Commit
    |
Azure DevOps Pipeline
    |
Terraform Init
    |
Terraform Validate
    |
Terraform Plan
    |
Manual Approval
    |
Terraform Apply
    |
Azure Resources Created
```

Pipeline stages:

### Init

Downloads providers and initializes backend.

```bash
terraform init
```

### Validate

Checks syntax.

```bash
terraform validate
```

### Plan

Shows changes.

```bash
terraform plan
```

### Apply

Creates/updates infrastructure.

```bash
terraform apply
```

---

# 6. Explain Service Connection.

Azure DevOps Service Connection provides secure authentication between Azure DevOps and external services.

Example:

Azure DevOps → Azure Subscription

It stores:

- Tenant ID
- Subscription ID
- Service Principal details

Used by pipelines to:

- Deploy Azure resources
- Access Azure Container Registry
- Deploy applications

---

# 7. Managed Identity vs Service Principal.

| Managed Identity | Service Principal |
|-|-|
| Azure-managed identity | Application identity |
| No secret management | Requires secret/certificate |
| Used mainly inside Azure | Used by external systems |
| More secure | Requires credential rotation |

Example:

VM accessing Key Vault:

Use Managed Identity.

External CI/CD pipeline:

Use Service Principal or OIDC.

---

# 8. Azure Hosted Agent vs Self Hosted Agent.

## Azure Hosted Agent

- Managed by Microsoft
- Fresh VM for every job
- No maintenance
- Limited customization

Example:

```yaml
pool:
 vmImage: ubuntu-latest
```

---

## Self Hosted Agent

- Managed by organization
- Installed on own VM
- Custom tools available
- Access private network

Example:

```yaml
pool:
 name: MyAgentPool
```

---

# 9. Developer manually changes Azure resource after Terraform deployment. How detect Terraform Drift?

Terraform drift occurs when actual infrastructure differs from Terraform configuration.

Detection:

Run:

```bash
terraform plan
```

Terraform compares:

```
Terraform Code
      |
      |
Terraform State
      |
      |
Actual Azure Infrastructure
```

Example:

Terraform:

```
VM Size = Standard_B2s
```

Portal:

```
VM Size = Standard_D4s
```

Terraform detects the difference.

---

# 10. Explain Terraform State File.

Terraform State File:

```
terraform.tfstate
```

maintains mapping between Terraform code and real infrastructure.

It stores:

- Resource IDs
- Dependencies
- Current configuration
- Metadata

Example:

Terraform knows:

```
azurerm_virtual_machine.vm1
=
Azure VM ID
```

State enables Terraform to perform updates efficiently.

---

# 11. Explain Refresh.

Terraform Refresh updates the state file with the actual infrastructure status.

Command:

```bash
terraform refresh
```

Modern Terraform:

```bash
terraform plan -refresh-only
```

It detects external changes without modifying infrastructure.

---

# 12. How does Terraform Plan detect deleted resources?

Terraform compares:

```
Terraform Configuration

+

State File

+

Actual Cloud Resources
```

Example:

State:

```
Storage Account exists
```

Azure:

```
Storage Account deleted
```

Terraform Plan shows:

```
-/+ Resource will be recreated
```

---

# 13. How do you restore deleted resources?

Depends on resource type.

Options:

- Run terraform apply to recreate resource.
- Restore from backup.
- Restore database backup.
- Recover storage snapshots.
- Import existing resource.

Example:

```bash
terraform apply
```

---

# 14. What is Zero Drift State / Equilibrium State?

Zero Drift means:

```
Terraform Code
       =
Terraform State
       =
Actual Infrastructure
```

No manual changes exist.

Running:

```bash
terraform plan
```

shows:

```
No changes. Infrastructure matches configuration.
```

---

# 15. How multiple engineers work on same Terraform project?

Use:

- Remote Backend
- Shared State
- State Locking
- Git Branching Strategy
- Code Review
- Pull Requests

Example:

Azure Backend:

```
Azure Storage Account
        |
Terraform State File
        |
State Lock
```

---

# 16. Explain State Locking.

State locking prevents multiple engineers from modifying infrastructure simultaneously.

Example:

Engineer A:

```
terraform apply
```

locks state.

Engineer B:

```
terraform apply
```

waits until lock is released.

Azure backend provides locking using Blob Lease.

---

# 17. Explain Application CI/CD Pipeline.

Application Pipeline:

```
Developer Commit

↓

Source Control

↓

Build

↓

Unit Test

↓

Code Quality Scan

↓

Security Scan

↓

Package Application

↓

Create Artifact

↓

Deploy

↓

Testing

↓

Production
```

---

# 18. Developer pushes code to Git. Explain every step till Production.

1. Developer commits code.
2. Pushes code to Git.
3. Pipeline trigger starts.
4. Agent checks out code.
5. Dependencies installed.
6. Application build starts.
7. Unit tests execute.
8. SonarQube scan runs.
9. Security scans execute.
10. Artifact created.
11. Artifact stored.
12. Deployment pipeline starts.
13. Application deployed to Dev.
14. Testing performed.
15. Approval received.
16. Deploy to Production.
17. Monitoring enabled.

---

# 19. Which Azure PaaS services have you worked on?

I have worked with:

- Azure App Service
- Azure Functions
- Azure SQL Database
- Azure Storage Account
- Azure Key Vault
- Azure Container Registry
- Azure Kubernetes Service
- Application Insights
- Azure API Management

---

# 20. Explain one project where you deployed application using Azure PaaS.

I worked on an application hosted on Azure App Service.

Architecture:

```
Developer
    |
Azure DevOps Pipeline
    |
Build Application
    |
Create Artifact
    |
Deploy to App Service
    |
Application Insights Monitoring
    |
Azure SQL Database
```

Responsibilities:

- CI/CD automation
- Infrastructure provisioning using Terraform
- App deployment
- Monitoring
- Security configuration

---

# 21. Application load becomes 10x. Which Azure PaaS service will you scale?

For Azure App Service:

Enable:

- Autoscaling
- Increase App Service Plan SKU
- Add more instances

For databases:

- Increase compute tier
- Enable read replicas
- Optimize queries

For AKS:

- Increase node count
- Enable cluster autoscaler
- Scale pods using HPA

---

# 22. How will you achieve High Availability?

Implement:

### Application Layer

- Multiple instances
- Load Balancer/Application Gateway
- Availability Zones

### Infrastructure Layer

- Multi-zone deployment
- Auto Scaling

### Database Layer

- Geo-replication
- Backup
- Failover groups

### Deployment Layer

- Blue-Green Deployment
- Canary Deployment
- Rolling Deployment

### Monitoring

- Azure Monitor
- Application Insights
- Alerts

Goal:

No single point of failure and automatic recovery during failures.
