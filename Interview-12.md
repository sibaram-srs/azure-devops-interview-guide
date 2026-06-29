
## Interview Ready Answers (Azure Security + Terraform + AKS)

This round is heavily focused on **Azure Security, Networking, Identity, Terraform, and AKS**. Interviewers usually expect architect-level answers.

---

# 1. Brief Personal Introduction (Non-Technical)

### Interview Ready Answer

"Thank you for the opportunity. My name is SR, and I have around 8 years of experience in the IT industry. Throughout my career, I have been passionate about learning new technologies and solving complex problems. I enjoy working in collaborative environments, taking ownership of responsibilities, and continuously improving processes. Outside of work, I like keeping myself updated with cloud and technology trends and spending time with family and friends."

---

# 2. If You Already Have an Offer, Why Are You Applying Here?

### Interview Ready Answer

"While compensation is always one factor, my primary motivation is the role itself and the opportunity for long-term growth.

What attracted me to this position is the chance to work on larger-scale cloud and DevOps initiatives, gain exposure to new challenges, and contribute to a mature engineering environment. I am looking for a role where I can continue growing technically while also taking on more ownership and responsibility."

---

# 3. Current Responsibilities & Security Experience

### Interview Ready Answer

"My responsibilities include:

* Azure Infrastructure Automation using Terraform
* AKS Administration
* CI/CD Pipeline Management
* Security Integration
* Monitoring and Reliability
* Production Support

From a security perspective, I work on:

* Key Vault Integration
* Managed Identity
* RBAC
* Azure Policies
* Private Endpoints
* WAF
* Security Scanning using Checkov, tfsec, and Trivy
* Secret Management
* Container Security"

---

# 4. Checkov vs tfsec vs Trivy

### Interview Ready Answer

| Tool    | Purpose                                                            |
| ------- | ------------------------------------------------------------------ |
| tfsec   | Terraform Security Scanner                                         |
| Checkov | IaC Security Scanner (Terraform, Kubernetes, CloudFormation, etc.) |
| Trivy   | Container & Image Vulnerability Scanner                            |

### Example

### tfsec

Checks:

```text
Open NSG Rules
Public Storage Accounts
Missing Encryption
```

### Checkov

Checks:

```text
Terraform
Kubernetes YAML
ARM Templates
Dockerfiles
```

### Trivy

Checks:

```text
Docker Images
OS Packages
Application Dependencies
Known CVEs
```

### Summary

"tfsec focuses mainly on Terraform, Checkov supports multiple IaC technologies, and Trivy focuses primarily on container and image vulnerabilities."

---

# 5. What is a Bastion Host?

### Interview Ready Answer

"Azure Bastion is a managed service that allows secure RDP and SSH access to virtual machines directly through the Azure Portal without exposing public IPs on the VMs."

Architecture:

```text
Admin
↓
Azure Bastion
↓
Private VM
```

---

# 6. Bastion Access Public or Private?

### Interview Ready Answer

"Users connect to the Azure Portal over the internet, but the actual VM access happens through the private network.

The VM itself does not require a public IP address.

So from the VM perspective, access remains private."

---

# 12. What is a Private Endpoint?

### Interview Ready Answer

"A Private Endpoint assigns a private IP address from a VNET to an Azure PaaS service.

This allows traffic to stay entirely within Microsoft's private network."

Example:

```text
AKS
↓
Private Endpoint
↓
Storage Account
```

No internet exposure is required."

---

# 13. Private Endpoint vs VNET Integration

### Interview Ready Answer

| Private Endpoint                        | VNET Integration                                     |
| --------------------------------------- | ---------------------------------------------------- |
| Used to access Azure services privately | Used by App Services to access resources inside VNET |
| Inbound to Azure Service                | Outbound from App Service                            |
| Creates Private IP                      | No Private IP Created                                |

### Example

Private Endpoint:

```text
AKS → Storage Account
```

VNET Integration:

```text
App Service → Database
```

---

# 14. What is Subnet Delegation?

### Interview Ready Answer

"Subnet Delegation allows a subnet to be dedicated to a specific Azure service.

Example:

```text
Microsoft.Web/serverFarms
```

for App Service.

This enables Azure services to manage networking resources within that subnet."

---

# 15. System Routes vs UDR

### Interview Ready Answer

| System Route          | User Defined Route |
| --------------------- | ------------------ |
| Automatically Created | Created by User    |
| Default Azure Routing | Custom Routing     |
| Managed by Azure      | Managed by Admin   |

### Example

Default:

```text
VNET → Internet
```

Custom:

```text
VNET → Firewall
```

using UDR."

---

# 16. Why Use UDR if VNET Peering Already Exists?

### Interview Ready Answer

"Peering only provides connectivity.

UDRs provide traffic control.

Example:

Without UDR:

```text
VNET-A → VNET-B
```

Directly.

With UDR:

```text
VNET-A
↓
Azure Firewall
↓
VNET-B
```

This enables inspection, filtering, logging, and security controls."

---

# 17. Load Balancers Worked With?

### Interview Ready Answer

"I have worked with:

### Azure Standard Load Balancer

Layer 4

* TCP
* UDP

### Azure Application Gateway

Layer 7

* HTTP
* HTTPS

### Azure Front Door

Global Layer 7 Load Balancer"

---

# 18. App Registration vs Service Principal

### Interview Ready Answer

### App Registration

Identity Definition.

Created in Entra ID.

### Service Principal

Actual Instance of that identity used for authentication.

Think:

```text
App Registration = Blueprint

Service Principal = Working Identity
```

---

# 19. App Registration vs Service Principal vs Managed Identity

### Interview Ready Answer

| Feature                 | App Registration | Service Principal | Managed Identity |
| ----------------------- | ---------------- | ----------------- | ---------------- |
| Identity Definition     | Yes              | No                | No               |
| Used for Authentication | Indirectly       | Yes               | Yes              |
| Secret Management       | Required         | Required          | No               |
| Azure Managed           | No               | No                | Yes              |

### Best Practice

Managed Identity whenever possible."

---

# 20. Why Application Gateway Instead of Load Balancer?

### Interview Ready Answer

Load Balancer:

* Layer 4
* TCP/UDP

Application Gateway:

* Layer 7
* HTTP/HTTPS
* SSL Offload
* URL Routing
* WAF

For web applications, Application Gateway is preferred."

---

# 21. What is WAF?

### Interview Ready Answer

"WAF protects web applications from application-layer attacks.

Examples:

* SQL Injection
* Cross-Site Scripting (XSS)
* Command Injection
* File Inclusion Attacks
* OWASP Top 10 Threats"

---

# 22. What Are Listeners in Application Gateway?

### Interview Ready Answer

"A Listener checks incoming requests and determines how traffic should be processed.

It contains:

* Protocol (HTTP/HTTPS)
* Port
* Hostname
* SSL Certificate

Example:

```text
https://app.company.com
```

The listener receives traffic and forwards it to routing rules."

---

# 23. What is Inline WAF Policy?

### Interview Ready Answer

"An inline WAF policy is configured directly within an Application Gateway.

It contains:

* Managed Rules
* Custom Rules
* Rate Limiting
* OWASP Protection

Newer designs generally use standalone WAF policies because they are reusable across multiple gateways."

---

# 24. Your Three Major Roles in Terraform

### Interview Ready Answer

"My primary Terraform responsibilities are:

1. Infrastructure Provisioning
2. Module Development & Maintenance
3. CI/CD Integration and State Management"

---

# 25. Resources Created Using Terraform

### Interview Ready Answer

Typically:

* Resource Groups
* VNETs
* Subnets
* NSGs
* Route Tables
* AKS
* ACR
* Key Vault
* Storage Accounts
* Application Gateway
* Azure SQL
* Managed Identities

Almost all Azure infrastructure is provisioned through Terraform."

---

# 26. Resource Created Elsewhere – Data Block or Import?

### Interview Ready Answer

### Read Existing Resource

Use:

```hcl
data "azurerm_subnet" "example" {}
```

### Manage Existing Resource

Use:

```bash
terraform import
```

### Interview Summary

"Data block reads resources. Import brings resources under Terraform management."

---

# 27. How Do You Upgrade a Terraform Module?

### Interview Ready Answer

Update:

```hcl
module "aks" {
  source  = "..."
  version = "2.0.0"
}
```

Then run:

```bash
terraform init -upgrade
terraform plan
terraform apply
```

Always test in lower environments first."

---

# 28. Resource from Another Subscription?

### Interview Ready Answer

Use Provider Alias:

```hcl
provider "azurerm" {
 alias           = "prod"
 subscription_id = "xxxx"
}
```

Then:

```hcl
provider = azurerm.prod
```

This allows cross-subscription deployments."

---

# 29. Pipelines Manual or YAML?

### Interview Ready Answer

"YAML pipelines.

Benefits:

* Version Controlled
* Reusable
* Auditable
* Easier Maintenance

Classic pipelines are mostly legacy."

---

# 30. New Repository – Pipeline Creation Steps

### Interview Ready Answer

1. Create Repository
2. Add Application Code
3. Add YAML Pipeline File
4. Configure Service Connections
5. Configure Variables
6. Configure Build Stage
7. Configure Security Scans
8. Configure Docker Build
9. Configure Deployment Stage
10. Test and Validate

---

# 31. Microsoft Defender for Cloud Experience?

### Interview Ready Answer

"Yes.

We use Defender for Cloud for:

* Security Recommendations
* Vulnerability Assessment
* Compliance Monitoring
* Threat Detection
* Secure Score Tracking

It provides centralized cloud security posture management."

---

# 32. AKS Experience?

### Interview Ready Answer

"I work on:

* AKS Provisioning
* Node Pools
* Networking
* Ingress
* Autoscaling
* Upgrades
* Monitoring
* Troubleshooting
* Security Integration

Both platform management and deployment support."

---

# 33. What are Helm Charts?

### Interview Ready Answer

"Helm is the package manager for Kubernetes.

A Helm Chart contains all Kubernetes manifests required to deploy an application.

Benefits:

* Reusability
* Parameterization
* Easier Upgrades
* Easier Rollbacks"

---

# 34. What Do You Use for Ingress/Egress?

### Interview Ready Answer

Ingress:

* NGINX Ingress Controller
* Azure Application Gateway Ingress Controller (AGIC)

Egress:

* Azure Firewall
* NAT Gateway
* NSGs

depending on security requirements."

---

# 35. Cluster Capability or Application Deployment?

### Interview Ready Answer

"I contribute to both areas.

### Platform Side

* AKS Provisioning
* Networking
* Security
* Monitoring

### Application Side

* CI/CD
* Helm Deployments
* Release Management
* Troubleshooting

So my role covers the complete AKS lifecycle from provisioning to production support."

---
