## Dt. 03-07-26 #DI

# 1. Introduce Yourself with Your Project Details

Hi, I'm **SR**, a DevOps Engineer with experience in **Azure Cloud, Terraform, Azure DevOps, Kubernetes (AKS), Docker, Azure Landing Zone, CI/CD, and Infrastructure as Code**.

Currently, I'm working on an enterprise project where my primary responsibility is to build and manage Azure infrastructure using **Terraform** following the **Azure Landing Zone** architecture.

### Project Details
- **Domain:** Enterprise Banking/Healthcare (change based on your project)
- **Cloud:** Microsoft Azure
- **IaC:** Terraform
- **CI/CD:** Azure DevOps
- **Containers:** Docker & AKS
- **Monitoring:** Azure Monitor, Log Analytics, Application Insights
- **Security:** Azure Policy, RBAC, Key Vault, NSG
- **Networking:** Hub & Spoke, VNet Peering, Private Endpoints

My responsibilities include provisioning infrastructure, automating deployments, implementing security and governance, supporting development teams, and ensuring high availability and scalability.

---

# 2. Explain Your Day-to-Day Activities and Team Contribution

My daily activities include:

- Attend daily stand-up meetings.
- Analyze new infrastructure requirements.
- Develop Terraform modules and deploy infrastructure.
- Review and merge pull requests.
- Maintain Azure Landing Zone standards.
- Configure Azure Policies and RBAC.
- Support AKS cluster deployments.
- Monitor infrastructure using Azure Monitor.
- Troubleshoot deployment failures.
- Coordinate with developers, security, and networking teams.
- Participate in release deployments and change management.
- Optimize infrastructure cost and performance.

---

# 3. Difference Between Infrastructure and Application

| Infrastructure | Application |
|---------------|-------------|
| Provides the platform for applications | Business logic used by end users |
| Managed by DevOps/Cloud Team | Managed by Developers |
| Includes VM, VNet, AKS, Storage, Load Balancer | Java, .NET, Node.js, Python applications |
| Focuses on availability, networking, security | Focuses on business functionality |
| Created using Terraform/Bicep | Developed using programming languages |

---

# 4. What is the Aim of Your Project?

The aim of our project is to provide a **secure, scalable, highly available, and automated cloud infrastructure** for application teams.

Our objective is to:

- Standardize Azure deployments
- Automate infrastructure provisioning
- Improve security using Azure Policies
- Reduce manual effort through Terraform
- Enable faster application deployments
- Ensure governance and compliance

---

# 5. Where Are You Using Azure Landing Zone in Your Project?

We use Azure Landing Zone as the **foundation of our Azure environment**.

It is used for:

- Organizing Management Groups
- Creating Subscriptions
- Centralized Networking
- Identity Management
- Security Governance
- Monitoring
- RBAC
- Azure Policies
- Shared Services

Every new application environment is deployed inside the Landing Zone.

---

# 6. What is Azure Landing Zone and Why Do We Need It?

Azure Landing Zone is a **predefined cloud architecture** that provides a secure, governed, scalable, and standardized Azure environment.

We use it because it provides:

- Governance
- Security
- Standardized architecture
- Networking
- Identity management
- Compliance
- Resource organization
- Automation
- Scalability

Instead of creating Azure resources randomly, Landing Zone gives a structured enterprise-ready environment.

---

# 7. Brief About Azure Landing Zone Architecture

Azure Landing Zone Architecture consists of:

- Root Management Group
- Platform Management Group
- Landing Zone Management Group
- Management Subscriptions
- Connectivity Subscription
- Identity Subscription
- Shared Services
- Hub Network
- Spoke VNets
- Azure Policies
- RBAC
- Monitoring
- Security Center
- Log Analytics
- Key Vault

Flow:

Root MG

→ Platform MG

→ Landing Zone MG

→ Production Subscription

→ Non-Production Subscription

Inside subscriptions:

Hub VNet

↓

Spoke VNets

↓

Application Resources

---

# 8. Difference Between Management Group and Subscription

| Management Group | Subscription |
|-----------------|-------------|
| Organizes multiple subscriptions | Container for Azure resources |
| Used for governance | Used for billing |
| Policies inherited by subscriptions | Resources inherit policies |
| No billing | Has billing |
| One Management Group contains many subscriptions | One subscription contains many resource groups |

---

# 9. Explain Hub and Spoke Architecture

Hub and Spoke is a centralized networking architecture.

### Hub contains

- Azure Firewall
- VPN Gateway
- ExpressRoute
- Bastion
- DNS
- Shared Services

### Spoke contains

- Application VNet
- AKS
- VM
- Database
- Storage

Benefits:

- Centralized security
- Reduced cost
- Easy management
- Better scalability
- Isolation between applications

---

# 10. What is Platform Landing Zone?

Platform Landing Zone hosts shared enterprise services.

It includes:

- Identity
- Networking
- Monitoring
- Logging
- Security
- Backup
- DNS
- Key Vault
- Firewall

Application teams consume these services instead of creating them separately.

---

# 11. What are Azure Policies? Explain Two Policies.

Azure Policy enforces organizational governance rules.

### Policy 1: Allowed Locations

Only approved Azure regions like East US or Central India can be used.

Purpose:
Prevent deployment in unauthorized regions.

---

### Policy 2: Allowed Resource Types

Only approved resource types such as VM, Storage, AKS can be created.

Purpose:
Prevent unauthorized resource creation.

---

# 12. Difference Between Azure Policy and RBAC

| Azure Policy | RBAC |
|--------------|------|
| Controls what can be created | Controls who can access |
| Governance | Authorization |
| Evaluates resources | Evaluates users |
| Can deny deployments | Can allow or deny access |
| Example: Only East US region | Example: Contributor role |

Simple answer:

**Policy controls Resources. RBAC controls Users.**

---

# 13. How Would You Design an Azure Landing Zone for Our Company?

I would follow Microsoft's Enterprise Scale Landing Zone.

### Step 1

Create Management Groups

- Root
- Platform
- Landing Zone
- Sandbox

### Step 2

Create Subscriptions

- Connectivity
- Identity
- Management
- Production
- Non-Production

### Step 3

Deploy Hub Network

- Azure Firewall
- VPN Gateway
- Bastion
- DNS

### Step 4

Create Spoke VNets

Separate VNets for each application.

### Step 5

Configure Identity

- Microsoft Entra ID
- Managed Identity
- PIM
- RBAC

### Step 6

Apply Azure Policies

- Allowed Locations
- Mandatory Tags
- Resource Naming
- Allowed SKUs
- Private Endpoints

### Step 7

Monitoring

- Azure Monitor
- Log Analytics
- Application Insights

### Step 8

Security

- Defender for Cloud
- NSG
- Key Vault
- Private Link

### Step 9

Automation

Provision everything using Terraform and Azure DevOps CI/CD.

---

# 14. How Do You Automate Landing Zone?

We automate Azure Landing Zone using:

- Terraform
- Azure DevOps Pipelines
- Git
- Remote Backend
- Terraform Modules
- Pull Requests
- Approval Gates

Deployment Flow:

Git

↓

Azure DevOps Pipeline

↓

Terraform Init

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Azure Landing Zone

---

# 15. Did You Work on Terraform? Why Do We Use Terraform?

Yes.

Terraform is an Infrastructure as Code (IaC) tool used to provision cloud infrastructure automatically.

Why Terraform?

- Infrastructure as Code
- Automation
- Multi-cloud support
- Version Control
- Reusability using Modules
- Consistency
- Faster deployment
- Easy rollback
- State management
- Reduced manual errors

---

# 16. How Do You Organize a Production Terraform Repository?

A production repository should be modular, scalable, and reusable.

```text
terraform-project/
│
├── modules/
│   ├── network/
│   ├── aks/
│   ├── storage/
│   ├── keyvault/
│   └── monitoring/
│
├── environments/
│   ├── dev/
│   ├── test/
│   ├── uat/
│   └── prod/
│
├── backend/
│   └── backend.tf
│
├── pipelines/
│   └── azure-pipelines.yml
│
├── scripts/
│
├── providers.tf
├── versions.tf
├── variables.tf
├── outputs.tf
├── locals.tf
├── README.md
```

### Best Practices

- Use reusable Terraform modules.
- Separate environments (Dev, Test, Prod).
- Store state remotely in Azure Storage Account.
- Enable state locking.
- Use Service Principal or Managed Identity.
- Store secrets in Azure Key Vault.
- Use Git branching and Pull Requests.
- Run `terraform fmt`, `validate`, and `plan` in CI/CD before `apply`.
- Use version pinning for Terraform and providers.
