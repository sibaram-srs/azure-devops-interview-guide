# DevOps Interview Questions & Expert Answers (Azure + Terraform + Kubernetes)

## 1. New Relic setup you did? Was it for APM or Log Management? Where were logs stored?

In my project, New Relic was used for both **Application Performance Monitoring (APM)** and **Infrastructure Monitoring**.

### APM
- JVM metrics (Heap, GC, Threads)
- Transaction tracing
- Response time
- Error rate
- Throughput
- Database query analysis
- External service latency

### Infrastructure Monitoring
- CPU
- Memory
- Disk
- Network
- Process monitoring

### Log Management
Application logs were collected using:
- Fluent Bit / Azure Monitor Agent
- New Relic Infrastructure Agent

Logs were forwarded to **New Relic Log Management**, where they were indexed and retained based on retention policies.

For long-term retention, some projects also stored logs in:
- Azure Log Analytics Workspace
- Azure Storage Account (Archive)

---

# 2. Security tools used? Difference between SAST and DAST?

## Tools Used

### Infrastructure Security
- Terraform Checkov
- tfsec
- Terrascan

### SAST
- SonarQube
- GitHub CodeQL
- Snyk Code

### Dependency Scanning
- Snyk
- OWASP Dependency Check

### Container Scanning
- Trivy
- Microsoft Defender for Cloud

### Secret Scanning
- GitLeaks
- GitHub Secret Scanning

### DAST
- OWASP ZAP

---

## Difference between SAST and DAST

| SAST | DAST |
|------|------|
| Static testing | Dynamic testing |
| Tests source code | Tests running application |
| Finds coding issues | Finds runtime vulnerabilities |
| Runs before deployment | Runs after deployment |
| White-box testing | Black-box testing |

Example:

SAST finds:
- SQL Injection code
- Hardcoded password
- Buffer overflow

DAST finds:
- Broken authentication
- XSS
- CSRF
- API vulnerabilities

---

# 3. How Backup and DR works?

We followed a **3-layer DR strategy**.

## Database

- Azure SQL Auto Backup
- Point-in-Time Restore
- Geo-replication

## VM

Azure Backup Vault

Daily Incremental Backup

Weekly Full Backup

Retention:
- 30 days
- 90 days
- 1 year

## Storage

- GRS (Geo-Redundant Storage)

## Kubernetes

- Velero backup
- Azure Disk Snapshot

## Terraform

Infrastructure stored in Git

Terraform State stored in Azure Storage

Versioning enabled

## DR

Secondary region maintained.

In disaster:

- Restore database
- Restore storage
- Terraform deploy infrastructure
- Kubernetes deploy applications
- Azure Front Door redirects traffic

---

# 4. Technologies deployed using Azure Pipeline

We deployed:

- Java (Spring Boot)
- .NET Core
- NodeJS
- React
- Angular
- Python APIs
- Docker Containers
- Kubernetes workloads
- Azure Functions

---

# 5. Explain Java Application Pipeline

Typical Azure DevOps Pipeline:

```
Developer Commit
       ↓
Pull Request
       ↓
Build
       ↓
Maven Compile
       ↓
Unit Test
       ↓
SonarQube
       ↓
Dependency Scan
       ↓
Docker Build
       ↓
Container Scan
       ↓
Push to ACR
       ↓
Terraform
       ↓
Deploy to AKS
       ↓
Smoke Test
       ↓
Approval
       ↓
Production
```

---

# 6. Have you used SonarQube?

Yes.

Integrated SonarQube into Azure DevOps YAML pipeline.

Every Pull Request triggered:
- Code analysis
- Quality Gate
- Code coverage
- Security analysis

Deployment failed if Quality Gate failed.

---

# 7. What can we achieve using SonarQube?

SonarQube provides:

- Code Smells
- Bugs
- Vulnerabilities
- Security Hotspots
- Code Coverage
- Duplicate Code Detection
- Technical Debt
- Maintainability Rating
- Reliability Rating
- Security Rating

Quality Gates stop deployment if thresholds are not met.

---

# 8. Is SAST and DAST implemented?

Yes.

Pipeline:

```
Developer
    ↓
Build
    ↓
SonarQube (SAST)
    ↓
Dependency Scan
    ↓
Docker Build
    ↓
Trivy Scan
    ↓
Deploy to Test
    ↓
OWASP ZAP (DAST)
    ↓
Production
```

---

# 9. Secret Management using Azure Key Vault

No secrets are stored in Git.

Stored secrets:

- Database password
- Storage Account Key
- API Keys
- Service Principal Secret
- TLS Certificates
- VM Credentials

Applications use Managed Identity to access Key Vault.

Password expiry is maintained (e.g., every 90 days).

Rotation is automated using:
- Azure Automation
- Azure Functions
- Event Grid

---

# 10. Example of Key Vault Secret Rotation

Example:

Database password stored in Key Vault.

Rotation steps:

1. Generate new password.
2. Update Azure SQL.
3. Store new password in Key Vault.
4. Application reads latest secret.
5. Restart application if required.

No code changes needed.

---

# 11. VM Password stored in Key Vault. How is it used?

Terraform never stores passwords.

Terraform retrieves secret:

```
data "azurerm_key_vault_secret"
```

VM resource references:

```
admin_password = data.azurerm_key_vault_secret.vm.value
```

Password never appears in source code.

---

# 12. Azure PaaS services used

- Azure App Service
- Azure SQL Database
- Azure Key Vault
- Azure Container Registry
- Azure Kubernetes Service
- Azure Functions
- Azure Service Bus
- Azure Event Hub
- Azure Redis Cache
- Azure Storage Account
- Azure API Management
- Azure Front Door

---

# 13. Databases used

- Azure SQL Database
- Azure PostgreSQL
- Azure MySQL
- Cosmos DB
- Redis Cache

---

# 14. Azure Front Door

Azure Front Door provides:

- Global Load Balancing
- SSL Offloading
- Web Application Firewall
- URL Routing
- CDN
- Health Probe
- Failover
- Caching

It routes users to the nearest healthy region.

---

# 15. Azure Application Gateway

Application Gateway provides:

- Layer 7 Load Balancer
- WAF
- SSL Termination
- Path-based Routing
- Cookie-based Affinity
- URL Rewrite

Typical Flow:

```
Internet
    ↓
Front Door
    ↓
Application Gateway
    ↓
AKS
```

---

# 16. Why Hub and Spoke?

Advantages:

- Centralized Security
- Shared Firewall
- Shared VPN
- Shared ExpressRoute
- Better Isolation
- Lower Cost
- Easier Governance

Hub contains:

- Firewall
- Bastion
- DNS
- VPN

Spokes contain application workloads.

---

# 17. Kubernetes Experience

Yes.

Worked on:

- AKS
- Deployments
- StatefulSets
- DaemonSets
- HPA
- Ingress
- ConfigMaps
- Secrets
- Persistent Volumes
- Helm
- RBAC
- Network Policies

---

# 18. Reduced deployment cycle by 70%

Achieved by:

- Full CI/CD Automation
- YAML Pipelines
- Parallel Jobs
- Automated Testing
- SonarQube
- Terraform Automation
- Blue-Green Deployment
- Approval Automation

Earlier:
6 hours

Current:
Less than 2 hours

---

# 19. Reduced Infra Creation Time

Earlier:

Manual Portal Creation

Took:
2–3 days

Now:

Terraform Modules

Single pipeline

Infrastructure created in around 30–40 minutes.

---

# 20. New Environment Creation Best Practice

Repository:

```
Terraform

modules/
   vnet/
   aks/
   sql/
   keyvault/

environments/
   dev/
   uat/
   preprod/
   prod/
```

Modules are reusable.

Each environment only changes variables.

---

# 21. Terraform Workflow

## Root Module

```
main.tf
variables.tf
outputs.tf
backend.tf
providers.tf
terraform.tfvars
```

## Child Modules

```
modules/

aks/
vnet/
sql/
keyvault/
storage/
monitoring/
```

Workflow:

```
terraform init

terraform fmt

terraform validate

terraform plan

Approval

terraform apply
```

State stored in Azure Storage with locking.

---

# 22. OpenCost Use Case

OpenCost was used in AKS.

Collected:

- Namespace Cost
- Deployment Cost
- Pod Cost
- Node Cost
- CPU Cost
- Memory Cost

Integrated with Prometheus.

---

# 23. Cost Optimization using OpenCost

Optimized:

- Overprovisioned CPU
- Memory Requests
- Idle Pods
- Unused Namespaces
- Zombie Workloads
- Node Utilization

Saved around 25–30% Kubernetes cost.

---

# 24. Other Azure Cost Optimizations

- Reserved Instances
- Savings Plan
- Auto Shutdown VMs
- Right Sizing
- Azure Advisor Recommendations
- Storage Lifecycle Rules
- Spot VM
- Autoscaling AKS
- Delete orphan disks
- Remove unused Public IPs

---

# 25. Separate Kubernetes Team?

Large organizations separate responsibilities.

Platform Team:
- AKS
- Networking
- Cluster Upgrade
- Node Pools

Application Team:
- Deployments
- Helm
- CI/CD
- Monitoring

This separation improves ownership and governance.

---

# 26. Docker Multi-stage Build

Stages:

```
Stage 1

Maven Build

↓

Stage 2

Run Unit Tests

↓

Stage 3

Package JAR

↓

Stage 4

Runtime Image (JRE)
```

Benefits:

- Smaller Image
- Better Security
- Faster Pull
- Less Storage

---

# 27. Application Architecture

Typical Architecture:

```
User

↓

Azure Front Door

↓

Application Gateway

↓

AKS

↓

Microservices

↓

Service Bus

↓

Azure SQL

↓

Redis

↓

Storage

↓

Monitoring

New Relic
```

---

# 28. Number of Microservices

Project had approximately:

30–50 microservices

Including:

- Authentication
- Order
- Payment
- Notification
- Inventory
- Customer
- Reporting

---

# 29. Team Size

Typical Team:

- Product Owner
- Scrum Master
- 6–8 Developers
- 2 QA
- 2 DevOps
- 1 Platform Engineer
- 1 Cloud Architect

Total:
12–16 members

---

# 30. Managing Base Image for 100 Microservices

Best Practice:

Create one standardized corporate base image.

Example:

```
company/java-base:11
```

All Dockerfiles:

```
FROM company/java-base:11
```

Single source of maintenance.

---

# 31. Upgrade JDK 11 to JDK 17

Approach:

- Build new base image

```
company/java-base:17
```

Run POC.

Execute:

- Unit Tests
- Integration Tests
- Performance Tests

Roll out gradually.

---

# 32. Rollout Base Image to 100 Services

Never update manually.

Approach:

- Update centralized base image
- Use Renovate/Dependabot or automation scripts to update `FROM` references
- Trigger CI pipelines
- Deploy in batches (Dev → UAT → PreProd → Prod)
- Monitor and rollback if needed

---

# 33. Upgrade Ubuntu Version on 5 VMs

Use:

- Azure Update Manager
- Azure Automation Update Management
- Ansible
- Terraform (if recreating immutable VMs)

Preferred:

Azure Update Manager with Maintenance Windows.

---

# 34. How will you refer VM inventory in for_each?

Example:

```hcl
variable "vms" {
  default = {
    vm1 = "Standard_B2s"
    vm2 = "Standard_B2s"
    vm3 = "Standard_D2s"
    vm4 = "Standard_D2s"
    vm5 = "Standard_B4ms"
  }
}

resource "azurerm_linux_virtual_machine" "vm" {
  for_each = var.vms

  name = each.key
  size = each.value
}
```

For Ansible Inventory:

```ini
[servers]
vm1
vm2
vm3
vm4
vm5
```

Then execute:

```bash
ansible-playbook upgrade.yml
```

This upgrades all VMs consistently with a single playbook.
