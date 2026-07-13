## Dt. 13-07-26 #DI

# Impressico Business Solutions - DevOps Interview Questions & Interview-Ready Answers

---

# 1. Tell Me About Your Last Project.

I worked on an enterprise Azure-based application that processes satellite imagery and geospatial data. My role was to design, automate, and manage the cloud infrastructure using Terraform and Azure DevOps. I was responsible for Azure Landing Zone implementation, networking, CI/CD automation, infrastructure deployment, monitoring, and production support.

**Technology Stack**
- Azure Cloud
- Terraform
- Azure DevOps
- AKS
- Docker
- Azure Load Balancer
- Azure Monitor
- Log Analytics
- Key Vault
- Azure Policies
- VM Scale Sets

---

# 2. What Was the Project Architecture?

The project follows a Hub & Spoke Azure Landing Zone Architecture.

```text
Satellite Data
       │
       ▼
 API Gateway / App Gateway
       │
 Azure Load Balancer
       │
 VM Scale Set / AKS
       │
 Backend APIs
       │
 Azure SQL / Storage Account
       │
 Azure Monitor & Log Analytics
```

Infrastructure is deployed using Terraform through Azure DevOps CI/CD pipelines.

---

# 3. What Were Your Responsibilities?

- Infrastructure provisioning using Terraform
- Azure Landing Zone implementation
- CI/CD pipeline creation in Azure DevOps
- VNet, NSG, Load Balancer, VMSS deployment
- Azure Policy and RBAC implementation
- Monitoring using Azure Monitor
- Production deployment support
- Troubleshooting infrastructure issues
- DR support
- Cost optimization

---

# 4. What Was the Business Requirement?

The business needed a secure and scalable platform to receive, process, and analyze satellite imagery with high availability, disaster recovery, and automated deployments.

---

# 5. How Is Data from Satellites Sent to Your System?

Satellite providers upload data through secure APIs or cloud storage. The application ingests the data, processes it through backend services, stores it in Azure Storage, and makes it available for analysis.

---

# 6. Is This Application Deployed in Production?

Yes.

The application is deployed in Production using Azure DevOps pipelines with approval gates and Infrastructure as Code.

---

# 7. How Is the Application Deployed?

Deployment Flow:

```text
Developer
      │
Git Repository
      │
Azure DevOps Pipeline
      │
Terraform Infrastructure Deployment
      │
Docker Image Build
      │
Push to Azure Container Registry
      │
Deploy to AKS / VMSS
      │
Validation
      │
Production
```

---

# 8. What Azure Services Are Used?

- Azure DevOps
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Azure Virtual Network
- Azure Load Balancer
- VM Scale Sets
- Azure Key Vault
- Azure Monitor
- Log Analytics
- Azure SQL
- Storage Account
- Azure Policy
- Azure RBAC

---

# 9. Which Azure Resources Are You Responsible For?

- Resource Groups
- VNets
- Subnets
- NSGs
- Load Balancer
- VM Scale Sets
- AKS
- Storage Accounts
- Key Vault
- Azure Policies
- RBAC
- Log Analytics
- Azure Monitor

---

# 10. Explain Your CI/CD Pipeline.

Pipeline Flow:

```text
Developer Commit

↓

Build

↓

Code Validation

↓

Security Scan

↓

Terraform Plan

↓

Approval

↓

Terraform Apply

↓

Docker Build

↓

Push to ACR

↓

Deploy to AKS

↓

Health Check

↓

Production
```

---

# 11. What Stages Are There in Azure DevOps Pipeline?

- Source Checkout
- Build
- Unit Testing
- Terraform Validate
- Terraform Plan
- Security Scan
- Manual Approval
- Terraform Apply
- Docker Build
- Image Push
- Deployment
- Health Check

---

# 12. What Validation and Scanning Tools Do You Use?

Infrastructure

- Terraform fmt
- Terraform validate
- Terraform plan

Security

- Checkov
- tfsec
- Trivy
- SonarQube

Container

- Trivy
- Microsoft Defender

---

# 13. How Do You Promote Code from Dev → QA → Production?

- Separate environments
- Separate Terraform workspaces
- Separate pipelines
- Manual approval before Production
- Same artifact promoted across environments

---

# 14. How Is Approval Handled Before Production?

Azure DevOps Environments provide Manual Approval Gates.

Production deployment starts only after approval from the Project Manager or Release Manager.

---

# 15. Are You Following a Modular Terraform Approach?

Yes.

We maintain reusable modules for:

- VNet
- NSG
- VMSS
- Storage
- Key Vault
- Monitoring
- Load Balancer

This improves reusability and consistency.

---

# 16. Which Azure Services Are Created Through Terraform?

- Resource Group
- VNet
- Subnets
- NSG
- Route Table
- Load Balancer
- VM Scale Set
- AKS
- Storage Account
- Key Vault
- Log Analytics
- Azure Monitor
- Public IP

---

# 17. Are You Using Virtual Machine Scale Sets (VMSS)?

Yes.

VMSS provides:

- Auto Scaling
- High Availability
- Load Balancing
- Automatic VM Management

---

# 18. How Did You Create the Azure Virtual Network?

Using Terraform.

```hcl
resource "azurerm_virtual_network" "vnet" {
  name = "project-vnet"
  address_space = ["10.0.0.0/16"]
}
```

---

# 19. What Components Are Created Inside a VNet?

- Subnets
- NSG
- Route Tables
- Private Endpoints
- NAT Gateway
- Network Interfaces

---

# 20. What CIDR Range Are You Using?

Example:

```
VNet

10.0.0.0/16

Frontend Subnet

10.0.1.0/24

Backend Subnet

10.0.2.0/24

Database

10.0.3.0/24
```

---

# 21. Why Separate Frontend and Backend Subnets?

Benefits:

- Better security
- Network isolation
- Different NSG rules
- Controlled communication
- Easier monitoring

---

# 22. Why Are You Using Azure Load Balancer?

To distribute incoming traffic across multiple backend virtual machines and ensure high availability.

---

# 23. How Does Azure Load Balancer Distribute Traffic?

Azure Load Balancer uses Layer 4 (TCP/UDP) load balancing and health probes to send traffic only to healthy backend instances.

---

# 24. How Many Virtual Machines Are There?

Typically 4–8 VMs depending on environment.

Production usually has multiple VM instances for high availability.

---

# 25. Are All VMs Part of One VMSS?

No.

Different workloads use different VM Scale Sets.

---

# 26. Why Multiple VM Scale Sets?

Because different services have different:

- Scaling requirements
- VM sizes
- CPU usage
- Memory usage
- Availability requirements

---

# 27. What Workloads Run on Each VMSS?

Example:

VMSS-1

Application Servers

VMSS-2

Background Processing

VMSS-3

API Services

---

# 28. Which Monitoring Tools Are You Using?

- Azure Monitor
- Log Analytics
- Application Insights
- Grafana
- Prometheus

---

# 29. What Dashboards Do You Monitor?

- CPU
- Memory
- Disk
- Network
- Application Response Time
- Request Failure
- Availability
- VM Health
- AKS Health

---

# 30. How Do You Monitor Application Health?

- Health Probes
- Azure Monitor Alerts
- Application Insights
- Availability Tests
- Log Analytics Queries

---

# 31. Explain Azure Landing Zone.

Azure Landing Zone is a pre-configured Azure environment with governance, security, networking, identity, and monitoring best practices for enterprise cloud adoption.

---

# 32. What Is a Management Group?

Management Group is a logical container used to organize multiple Azure subscriptions and apply governance policies centrally.

---

# 33. Why Are You Using Management Groups?

- Centralized Governance
- Policy Management
- RBAC
- Standardization
- Compliance

---

# 34. Why Separate Subscriptions?

To isolate:

- Billing
- Security
- Access
- Resource Limits
- Production Risk

---

# 35. Purpose of Resource Groups?

Resource Groups logically organize Azure resources for lifecycle management, permissions, and deployments.

---

# 36. Purpose of Azure Subscription?

Subscription provides:

- Billing boundary
- Resource quota
- Security boundary
- Access management

---

# 37. Why Do Organizations Implement Azure Landing Zones?

- Governance
- Security
- Compliance
- Standardization
- Scalability
- Automation

---

# 38. What Security Measures Are You Taking?

- RBAC
- Azure Policy
- NSG
- Key Vault
- Private Endpoint
- Managed Identity
- Defender for Cloud
- HTTPS
- MFA

---

# 39. Which Security Scanning Tools Are Used?

- SonarQube
- Checkov
- tfsec
- Trivy
- Microsoft Defender

---

# 40. What Do SAST and DAST Mean?

**SAST (Static Application Security Testing)** scans source code for vulnerabilities without running the application.

**DAST (Dynamic Application Security Testing)** scans a running application by simulating real attacks.

---

# 41. Are SAST and DAST Software or Methodologies?

They are **security testing methodologies** implemented using tools.

Examples:

SAST:
- SonarQube
- Checkmarx

DAST:
- OWASP ZAP
- Burp Suite

---

# 42. What Is Your Disaster Recovery Strategy?

We use an **Active-Passive DR** architecture.

- Primary Region handles traffic.
- Secondary Region remains on standby.
- Data is continuously replicated.
- Failover is initiated during a disaster.

---

# 43. Why Active-Passive Instead of Active-Active?

- Lower cost
- Easier management
- Simpler synchronization
- Suitable for our business RTO/RPO requirements

---

# 44. How Do You Synchronize the DR Environment?

- Azure Site Recovery (for VMs)
- Geo-redundant Storage (GRS)
- Azure Backup
- Database Replication

---

# 45. How Do You Synchronize the Database?

Using:

- Azure SQL Active Geo-Replication
- Automated database backups
- Transaction log replication

---

# 46. Explain RTO and RPO.

**RTO (Recovery Time Objective)** is the maximum acceptable time to restore services after a disaster.

**RPO (Recovery Point Objective)** is the maximum acceptable amount of data loss measured in time.

Example:
- RTO = 2 Hours
- RPO = 15 Minutes

---

# 47. What Is the Most Challenging Production Issue You Faced?

A production deployment failed because a Terraform change accidentally modified the Network Security Group, blocking application traffic.

---

# 48. How Did You Troubleshoot It?

- Checked Azure DevOps pipeline logs.
- Reviewed Terraform plan.
- Verified NSG rules.
- Checked Azure Monitor metrics.
- Validated application health.
- Compared current configuration with the previous release.

---

# 49. How Did You Identify the Root Cause?

Terraform plan showed that an incorrect NSG rule was being applied, which blocked inbound traffic to the application.

---

# 50. How Did You Perform the Rollback?

- Stopped the deployment.
- Reverted the Git commit.
- Redeployed the last stable Terraform code.
- Verified infrastructure and application health.

---

# 51. What Was Your Rollback Strategy?

- Version-controlled infrastructure.
- Immutable build artifacts.
- Previous stable release stored in Azure Artifacts/ACR.
- One-click redeployment using Azure DevOps.

---

# 52. Explain How Rollback Works Internally in Your CI/CD Pipeline.

1. Every successful deployment is tagged with a release version.
2. Build artifacts and Docker images are stored with version tags.
3. On failure, the release manager selects the previous stable version.
4. The pipeline redeploys that version using the same deployment steps.
5. Health checks validate the rollback before traffic is fully restored.

---

# 53. How Does the Pipeline Identify the Previous Working Version?

The pipeline retrieves the last successful build or release tag from Azure DevOps (or the previous Docker image tag stored in Azure Container Registry). Only builds marked as successful are eligible for rollback.

---

# 54. How Does the Pipeline Redeploy the Old Build?

- Select the previous successful artifact or Docker image.
- Pull the version from Azure Artifacts or ACR.
- Redeploy it using the same release pipeline.
- Run post-deployment health checks.
- If validation passes, the rollback is completed and traffic continues on the stable version.
