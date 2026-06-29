
## Interview Ready Answers (Azure + Terraform + Azure DevOps + Security)

This round is heavily focused on **Azure Architecture, Networking, Security, Terraform, Azure DevOps, and Git**. Most questions are scenario-based, so answer like an Azure Architect rather than just a DevOps Engineer.

---

# 1. Tell Me About Yourself

### Interview Ready Answer

"Hi, my name is SR. I have around 8 years of experience in Cloud and DevOps Engineering with expertise in Azure Cloud, Azure DevOps, Terraform, Docker, Kubernetes (AKS), CI/CD, Infrastructure as Code, Monitoring, and Cloud Security.

Currently, I work on Azure-based enterprise applications where I manage infrastructure provisioning, CI/CD automation, AKS deployments, security implementation, monitoring, and production support. My focus is on automation, scalability, reliability, and secure cloud architecture."

---

# 2. Which Azure Services Are You Working With?

### Interview Answer

* Resource Groups
* VNETs
* Subnets
* NSGs
* Route Tables
* Azure Firewall
* Application Gateway
* Azure Front Door
* AKS
* ACR
* Key Vault
* Azure Monitor
* Log Analytics
* Storage Accounts
* VMSS
* Azure SQL
* Managed Identity
* Azure DevOps

---

# 3. What is Azure Landing Zone?

### Interview Answer

"Azure Landing Zone is a pre-configured Azure environment that provides governance, security, networking, identity, monitoring, and compliance foundations for deploying workloads.

It includes:

* Management Groups
* Subscriptions
* Policies
* RBAC
* Networking
* Monitoring
* Security Controls

Its purpose is to provide a secure and scalable cloud foundation."

---

# 4. Explain Hub-and-Spoke Architecture

### Interview Answer

```text
Hub VNET
│
├── Firewall
├── VPN Gateway
├── Shared Services
│
├── Spoke VNET 1 (App)
├── Spoke VNET 2 (DB)
├── Spoke VNET 3 (Dev)
```

Hub contains shared services.

Spokes host workloads.

Benefits:

* Centralized Security
* Lower Cost
* Better Governance
* Easier Management

---

# 5. Landing Zone Components for Monolithic Application

### Interview Answer

Typical setup:

```text
Resource Group
↓
VNET
↓
Subnets
↓
NSG
↓
Application Gateway
↓
VM / App Service
↓
Database
↓
Key Vault
↓
Monitoring
```

Additional components:

* Backup
* RBAC
* Policies
* WAF

---

# 6. Types of Azure Load Balancers

### Interview Answer

### Azure Load Balancer

Layer 4

* Internal
* Public

### Application Gateway

Layer 7

### Azure Front Door

Global Layer 7

### Traffic Manager

DNS-based Global Load Balancing

---

# 7. Azure Load Balancer vs Application Gateway

| Load Balancer   | Application Gateway |
| --------------- | ------------------- |
| Layer 4         | Layer 7             |
| TCP/UDP         | HTTP/HTTPS          |
| No WAF          | Supports WAF        |
| No URL Routing  | URL Routing         |
| Network Traffic | Web Traffic         |

---

# 8. Global vs Regional Load Balancer

### Regional

Application Gateway

Azure Load Balancer

Single Region

### Global

Azure Front Door

Traffic Manager

Multi-Region

Disaster Recovery

---

# 9. Why Not Use Public Load Balancer?

### Interview Answer

"For internal applications.

Security concerns.

Instead use:

* Internal Load Balancer
* Private Endpoint
* VPN
* ExpressRoute"

---

# 10 & 11. Complete Traffic Flow from Internet to VM

### Interview Answer

```text
User
↓
DNS
↓
Azure Front Door
↓
Application Gateway (WAF)
↓
Load Balancer
↓
VM
```

Security Checks:

* DNS Resolution
* WAF Inspection
* NSG Validation
* Application Processing

---

# 12. Azure Traffic Manager

### Interview Answer

"Traffic Manager is a DNS-based global load balancer.

It routes users based on:

* Performance
* Priority
* Geographic Location
* Weighted Routing

It does not proxy traffic; it only returns the best endpoint."

---

# 13. What is a Route Table?

### Interview Answer

"A Route Table defines how network traffic should travel."

Example:

```text
Subnet
↓
Route Table
↓
Azure Firewall
↓
Internet
```

Used for custom routing.

---

# 14. What is NSG?

### Interview Answer

"NSG is a Layer 3/4 firewall that controls inbound and outbound traffic using rules."

Example:

```text
Allow 443
Deny 22
```

---

# 15. What is ASG?

### Interview Answer

"Application Security Group logically groups VMs.

Instead of using IP addresses, NSG rules can reference ASGs."

Example:

```text
Web-ASG
↓
Allow
↓
DB-ASG
```

Easier rule management.

---

# 16. Types of Azure Firewall

### Interview Answer

### Azure Firewall Standard

* Network Filtering
* Application Filtering

### Azure Firewall Premium

* TLS Inspection
* IDS/IPS
* URL Filtering

---

# 17. Configure Application Gateway with WAF

### Interview Answer

Steps:

1. Create Application Gateway
2. Enable WAF
3. Choose Prevention Mode
4. Configure OWASP Rules
5. Configure Backend Pool
6. Configure Listener
7. Configure Routing Rules

---

# 18. VPN Gateway

### Interview Answer

"VPN Gateway provides encrypted connectivity between Azure and on-premises networks."

Supports:

* Site-to-Site VPN
* Point-to-Site VPN
* VNET-to-VNET VPN

---

# 19. Configure VPN Gateway

### Interview Answer

1. Create Gateway Subnet
2. Create Public IP
3. Create VPN Gateway
4. Configure Local Network Gateway
5. Configure Connection
6. Exchange Shared Key

---

# 20. Private Endpoint vs Service Endpoint

| Private Endpoint | Service Endpoint         |
| ---------------- | ------------------------ |
| Private IP       | Public Endpoint          |
| More Secure      | Less Secure              |
| Private Access   | Public Access Restricted |
| Preferred        | Older Approach           |

---

# 21. Configure Private Endpoint

### Interview Answer

1. Select Azure Resource
2. Create Private Endpoint
3. Select VNET/Subnet
4. Configure DNS
5. Approve Connection

Resource receives a private IP.

---

# 22. What is NIC?

### Interview Answer

"NIC is the network interface attached to a VM.

It provides:

* Private IP
* Public IP Association
* NSG Association
* Network Connectivity

A VM cannot communicate without a NIC."

---

# 23. Terraform Linux VM Code

### Interview Answer

Interviewers usually want structure:

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  name                = "linuxvm"
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  size                = "Standard_B2s"

  admin_username = "azureuser"

  network_interface_ids = [
    azurerm_network_interface.nic.id
  ]
}
```

---

# 24. What is Azure Key Vault?

### Interview Answer

"Azure Key Vault securely stores:

* Secrets
* Certificates
* Encryption Keys

Benefits:

* Centralized Secret Management
* Audit Logging
* Access Control
* Managed Identity Integration"

---

# 25. Key Vault Integration with Azure DevOps

### Interview Answer

Pipeline
↓

Service Connection

↓

Key Vault

↓

Pipeline Variables

↓

Deployment

Secrets are fetched dynamically during execution.

---

# 26. Configure Key Vault

### Interview Answer

1. Create Key Vault
2. Add Secrets
3. Configure Access Policy or RBAC
4. Enable Private Endpoint (Optional)
5. Grant Access to Applications

---

# 27. Permission Required to Read Secrets

### Interview Answer

RBAC:

```text
Key Vault Secrets User
```

or

Access Policy:

```text
Get
List
```

Permissions.

---

# 28. Key Vault Objects

### Interview Answer

* Secrets
* Keys
* Certificates

---

# 29. What is Service Connection?

### Interview Answer

"A Service Connection allows Azure DevOps to authenticate and interact with external systems such as Azure subscriptions."

---

# 30. Types of Service Connections

### Interview Answer

* Azure Resource Manager
* Kubernetes
* Docker Registry
* GitHub
* Service Principal
* Generic Service Connection

---

# 31. Configure Service Connection

### Interview Answer

Project Settings

↓

Service Connections

↓

New Connection

↓

Azure Resource Manager

↓

Service Principal

↓

Validate & Save

---

# 32. What is App Registration?

### Interview Answer

"App Registration creates an identity in Microsoft Entra ID that applications use for authentication and authorization."

---

# 33. What is Environment in Azure DevOps?

### Interview Answer

"Environment represents a deployment target.

Examples:

* Dev
* QA
* UAT
* Production

Supports approvals and deployment history."

---

# 34. What are Jobs?

### Interview Answer

"A Job is a collection of tasks executed on an agent."

Example:

```yaml
jobs:
- job: Build
```

---

# 35. Which Agent Do You Use?

### Interview Answer

"Mostly Self-Hosted Agents because:

* Better Control
* Faster Execution
* Internal Network Access
* Custom Tools"

---

# 36. How Many Agents Per Project?

### Interview Answer

Depends on workload.

Typical:

* Dev Projects → 1–2 Agents
* Enterprise Projects → 3–10 Agents

High parallelism requires multiple agents.

---

# 37. Reduce Pipeline Execution Time

### Interview Answer

* Parallel Jobs
* Self-Hosted Agents
* Dependency Caching
* Incremental Builds
* Optimized Testing
* Reusable Templates

---

# 38. Why Terraform Modules?

### Interview Answer

Benefits:

* Reusability
* Standardization
* Maintainability
* Scalability

---

# 39. Call Child Module

```hcl
module "network" {
  source = "./modules/network"
}
```

---

# 40. Variables.tf Mandatory?

### Interview Answer

No.

Terraform only requires variables to be declared.

The filename can be anything.

However:

```text
variables.tf
```

is a best practice.

---

# 41. What Happens During terraform init?

### Interview Answer

Terraform:

1. Downloads Providers
2. Downloads Modules
3. Configures Backend
4. Initializes Working Directory

---

# 42 & 43. Data Block

### Interview Answer

Used to read existing resources.

Example:

```hcl
data "azurerm_subnet" "existing" {
  name = "subnet1"
}
```

No resource creation occurs.

---

# 44. Terraform Version?

### Interview Answer

Never memorize exact versions.

Say:

> "We use enterprise-approved stable versions. I always verify exact versions from the environment before implementation."

---

# 45. State Drift

### Interview Answer

Occurs when infrastructure changes outside Terraform.

Detection:

```bash
terraform plan
```

Resolution:

* Import Resource
* Update Code
* Reconcile Differences

---

# 46. Merge vs Rebase vs Pull

| Command | Purpose           |
| ------- | ----------------- |
| Merge   | Combines Branches |
| Rebase  | Rewrites History  |
| Pull    | Fetch + Merge     |

---

# 47. Why Rebase?

### Interview Answer

"Rebase creates a cleaner and linear Git history compared to merge."

---

# 48. Git Reset

### Interview Answer

Moves HEAD to a previous commit.

---

# 49. Types of Git Reset

### Soft

```bash
git reset --soft HEAD~1
```

Keep changes staged.

### Mixed

```bash
git reset HEAD~1
```

Keep changes unstaged.

### Hard

```bash
git reset --hard HEAD~1
```

Remove everything.

---

# 50. Protect Branches

### Interview Answer

Use:

* Branch Policies
* PR Approval
* Restricted Push Access
* Required Reviews
* Delete Protection

---

# 51 & 52. End-to-End Azure Security

### Interview Answer

### Network Layer

* NSG
* Azure Firewall
* Private Endpoint

### Identity Layer

* Entra ID
* MFA
* Managed Identity

### Compute Layer

* Defender for Cloud
* Patch Management

### Application Layer

* WAF
* TLS

### Data Layer

* Key Vault
* Encryption

---

# 53. App Works in Dev but Fails in Prod

### Interview Answer

Check:

1. Configuration Differences
2. Secrets
3. Environment Variables
4. Network Access
5. DNS
6. Database Connectivity
7. Logs
8. Recent Changes

Always compare Dev vs Prod.

---

# 54. Security Best Practices

### Interview Answer

* Least Privilege Access
* Private Endpoints
* Managed Identity
* WAF
* Encryption
* Network Segmentation
* Security Scanning
* Monitoring

---

# 55. IaaS vs PaaS

| IaaS                   | PaaS                 |
| ---------------------- | -------------------- |
| VM Management Required | Managed Platform     |
| More Control           | Less Management      |
| Example: VM            | Example: App Service |

---

# 56. What is an Epic?

### Interview Answer

"An Epic is a large business requirement that is broken into smaller features and user stories."

---

# 57. What is a Sprint?

### Interview Answer

"A Sprint is a fixed-duration iteration, usually 2–3 weeks, during which a team delivers planned work items."

---
