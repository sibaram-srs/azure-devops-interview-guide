# CreoDIspan

# Senior DevOps Engineer Interview Questions & Answers

---

# 1. What is Terraform Drift?

## Definition

Terraform Drift occurs when the actual infrastructure differs from the infrastructure defined in the Terraform state file and configuration.

This usually happens when someone manually modifies resources outside Terraform (Azure Portal, AWS Console, CLI, PowerShell, etc.).

Example:

```text
Terraform Code
        ↓
Terraform State
        ↓
Azure Infrastructure

Someone changes VM Size from Azure Portal
        ↓
Infrastructure != State
        ↓
Terraform Drift
```

## How to Detect Drift

```bash
terraform plan
```

Terraform compares:

- Terraform configuration
- Terraform state
- Actual infrastructure

If differences exist, Terraform reports drift.

## How to Fix Drift

### Option 1 (Recommended)

Update Terraform code.

```bash
terraform apply
```

### Option 2

Import manual changes into Terraform.

```bash
terraform import
```

### Option 3

If changes are intentional, update the Terraform configuration.

## Best Practices

- Never modify infrastructure manually.
- Enable RBAC.
- Use CI/CD for deployments.
- Store state remotely.
- Run regular `terraform plan`.

---

# 2. What is Terraform Import?

## Definition

Terraform Import allows existing infrastructure to be managed by Terraform without recreating it.

Example:

A Storage Account already exists in Azure.

Instead of deleting and recreating it:

```bash
terraform import azurerm_storage_account.storage /subscriptions/xxxxx/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/devstorage001
```

After import:

```bash
terraform plan
```

Terraform detects missing configuration.

Write the resource block:

```hcl
resource "azurerm_storage_account" "storage" {
  name                     = "devstorage001"
  location                 = "East US"
  resource_group_name      = "rg-demo"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

## Benefits

- No downtime
- Existing infrastructure becomes managed by Terraform
- Avoid recreation

---

# 3. Explain the Deployment Process

A typical production deployment process is:

```text
Developer
     │
     ▼
Git Push
     │
     ▼
Pull Request
     │
     ▼
Code Review
     │
     ▼
Merge to Main
     │
     ▼
CI Pipeline
     │
     ├── Build
     ├── Unit Test
     ├── Security Scan
     ├── SonarQube Analysis
     ├── Docker Build
     ├── Push Image
     ▼
Artifact Registry (ACR)
     ▼
CD Pipeline
     │
Terraform Apply
     │
Deploy Application
     │
Helm Upgrade
     │
Smoke Test
     │
Approval
     ▼
Production
```

## Best Practices

- Infrastructure as Code
- Automated testing
- Security scanning
- Manual approval for Production
- Rollback strategy

---

# 4. Role and Responsibilities of a Senior DevOps Engineer

## Infrastructure

- Design cloud architecture
- Terraform modules
- Landing Zone implementation

## CI/CD

- Jenkins
- Azure DevOps
- GitHub Actions

## Containers

- Docker
- Kubernetes
- Helm

## Monitoring

- Prometheus
- Grafana
- Azure Monitor
- Log Analytics

## Security

- Key Vault
- IAM
- RBAC
- Secrets Management

## Automation

- Bash
- PowerShell
- Python

## Reliability

- High Availability
- Disaster Recovery
- Backup Strategy

## Collaboration

- Developers
- QA
- Security
- Cloud Team

---

# 5. What is an Azure Landing Zone?

## Definition

A Landing Zone is a pre-configured Azure environment that follows Microsoft's Cloud Adoption Framework.

It provides:

- Networking
- Security
- Identity
- Governance
- Policies
- Monitoring

Example Architecture

```text
Management Group
      │
Subscriptions
      │
Hub VNet
      │
Spoke VNets
      │
Applications
```

Components

- Azure Policy
- RBAC
- Log Analytics
- Defender
- Firewall
- Key Vault
- Networking
- Resource Groups

Benefits

- Standardization
- Governance
- Security
- Scalability

---

# 6. What is PaaS (Platform as a Service)?

PaaS provides a platform to develop, deploy, and manage applications without managing servers.

Examples

- Azure App Service
- Azure SQL Database
- Azure Functions
- Azure Container Apps

Provider Manages

- OS
- Patch Management
- Networking
- Runtime
- Scaling

Customer Manages

- Application
- Configuration
- Data

Advantages

- Less maintenance
- Auto Scaling
- High Availability
- Faster deployment

---

# 7. Application Gateway vs Load Balancer

| Feature | Azure Load Balancer | Azure Application Gateway |
|----------|---------------------|---------------------------|
| Layer | Layer 4 | Layer 7 |
| Protocol | TCP/UDP | HTTP/HTTPS |
| SSL Termination | No | Yes |
| URL Routing | No | Yes |
| Cookie Affinity | No | Yes |
| WAF | No | Yes |
| Path-based Routing | No | Yes |

## Load Balancer

Used for:

- VM traffic
- TCP applications
- Database traffic

## Application Gateway

Used for:

- Web applications
- APIs
- HTTPS
- WAF

---

# 8. How Does Backend Communicate with Database?

Typical Architecture

```text
User
   │
Application Gateway
   │
Backend API
   │
Private Endpoint
   │
Azure SQL Database
```

Communication Process

1. Client sends request.
2. Backend authenticates.
3. Backend retrieves secrets from Key Vault.
4. Backend opens secure connection.
5. Executes SQL query.
6. Returns response.

Security Best Practices

- Private Endpoint
- Managed Identity
- Key Vault
- TLS Encryption
- NSG Rules

Never expose database publicly.

---

# 9. What is NSG (Network Security Group)?

NSG controls inbound and outbound traffic.

Example

```text
Internet
     │
NSG
     │
VM
```

Rules

- Source
- Destination
- Port
- Protocol
- Allow/Deny

Example

Allow

```
443 HTTPS
```

Deny

```
3389 RDP from Internet
```

Priority

Lower number = Higher priority

Example

```
100 Allow HTTPS

200 Allow SSH

300 Deny All
```

---

# 10. What is Azure Key Vault?

Key Vault securely stores:

- Secrets
- Passwords
- Certificates
- Encryption Keys

Example

Application

↓

Managed Identity

↓

Key Vault

↓

Database Password

Benefits

- Secret Rotation
- RBAC
- Encryption
- Audit Logs

Never store secrets in:

- Git
- Terraform variables
- Docker images

---

# 11. How Do You Manage Terraform State File?

Use Remote Backend.

Azure Storage Account

↓

Blob Container

↓

Terraform State

Example

```text
Terraform
      │
Azure Storage Account
      │
Blob Container
      │
terraform.tfstate
```

Benefits

- State Locking
- Team Collaboration
- Versioning
- Backup

Never commit:

```
terraform.tfstate
```

Use

```
.gitignore
```

Enable

- Blob Versioning
- Soft Delete
- Encryption

---

# 12. Explain the CI/CD Pipeline Process

CI Pipeline

```text
Developer
     │
Git Push
     │
Build
     │
Unit Test
     │
Security Scan
     │
Docker Build
     │
Push Image
```

CD Pipeline

```text
Terraform
      │
Infrastructure
      │
Deploy
      │
Helm Upgrade
      │
Smoke Test
      │
Approval
      │
Production
```

Tools

- Azure DevOps
- GitHub Actions
- Jenkins
- ArgoCD
- Helm
- Terraform

Best Practices

- Separate CI and CD.
- Immutable artifacts.
- Environment promotion (Dev → QA → UAT → Prod).
- Automated rollback.
- Approval gates for Production.

---

# 13. Create a Storage Account and Configure Terraform Backend for Blob Storage

## Step 1: Create Resource Group

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "rg-terraform-state"
  location = "East US"
}
```

---

## Step 2: Create Storage Account

```hcl
resource "azurerm_storage_account" "storage" {
  name                     = "tfstateprod001"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

---

## Step 3: Create Blob Container

```hcl
resource "azurerm_storage_container" "container" {
  name                  = "tfstate"
  storage_account_id    = azurerm_storage_account.storage.id
  container_access_type = "private"
}
```

---

## Step 4: Configure Terraform Backend

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-terraform-state"
    storage_account_name = "tfstateprod001"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
}
```

---

## Initialize Terraform

```bash
terraform init
```

Terraform migrates the local state to Azure Blob Storage.

---

# Production Best Practices

- Use remote backend for all environments.
- Enable Blob Versioning and Soft Delete.
- Store secrets in Azure Key Vault.
- Use Managed Identity instead of access keys where possible.
- Use separate state files for Dev, Test, and Prod.
- Implement reusable Terraform modules.
- Enforce Azure Policy and RBAC.
- Protect production deployments with approval gates.
- Enable monitoring, logging, and alerting for infrastructure and applications.
- Avoid manual changes to infrastructure to prevent configuration drift.
