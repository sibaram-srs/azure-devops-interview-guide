# DevOps Interview Questions & Expert Answers (Azure | Terraform | Docker | Kubernetes)

---

# 1. Introduce Yourself

**Sample Answer (2–3 Minutes)**

Hi, I'm a DevOps Engineer with **around X years of experience** in designing, automating, and managing cloud infrastructure, primarily on **Microsoft Azure**.

My expertise includes:
- Azure Cloud Services
- Azure DevOps (CI/CD)
- Terraform (Infrastructure as Code)
- Docker
- Kubernetes (AKS)
- GitHub
- Azure Repos
- Linux Administration
- Monitoring using New Relic, Azure Monitor, Prometheus, and Grafana
- DevSecOps tools like SonarQube, tfsec, Checkov, Trivy, GitLeaks, and OWASP ZAP

In my current project, I manage end-to-end CI/CD pipelines for Java and .NET microservices deployed on Azure Kubernetes Service (AKS). I have built reusable Terraform modules to provision Azure infrastructure, integrated security scanning into pipelines, implemented centralized secret management using Azure Key Vault, and worked on cost optimization using OpenCost and Azure Advisor.

Some of my key achievements include:
- Reduced deployment time from **4 days to 4 hours** by automating CI/CD pipelines.
- Reduced infrastructure provisioning time from **2–3 days to less than 40 minutes** using Terraform.
- Improved application reliability through monitoring, automated deployments, and standardized infrastructure.
- Implemented DevSecOps practices by integrating SAST, DAST, container scanning, and Infrastructure-as-Code security checks into CI/CD pipelines.

I'm passionate about automation, cloud architecture, infrastructure security, and continuously improving deployment reliability and operational efficiency.

---

# 2. How do you create a Dockerfile?

A Dockerfile defines how to build a Docker image.

Typical steps include:

1. Choose a base image.
2. Set the working directory.
3. Copy application files.
4. Install dependencies.
5. Build the application (if required).
6. Expose the application port.
7. Define the startup command.

Example:

```dockerfile
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY target/app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

For production, I prefer **multi-stage Docker builds** to reduce image size and improve security.

Example:

```dockerfile
# Build Stage
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package

# Runtime Stage
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY --from=builder /app/target/app.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

Benefits:

- Smaller image size
- Faster deployments
- Reduced attack surface
- Cleaner production images

---

# 3. Explain Terraform Modules.

Terraform modules allow us to reuse infrastructure code instead of duplicating it.

Instead of writing the same Terraform code for every environment, we create reusable modules.

Example structure:

```
terraform/

modules/
    vnet/
    aks/
    sql/
    keyvault/
    storage/

environments/
    dev/
    uat/
    prod/
```

Each module manages a specific Azure resource.

Advantages:

- Code reuse
- Easier maintenance
- Consistent deployments
- Simplified testing
- Better scalability

---

# 4. How do Terraform modules communicate with each other?

Terraform modules communicate through **inputs (variables)** and **outputs**.

Example:

### VNet Module

```terraform
output "subnet_id" {
  value = azurerm_subnet.app.id
}
```

### AKS Module

```terraform
module "vnet" {
  source = "../modules/vnet"
}

module "aks" {
  source    = "../modules/aks"
  subnet_id = module.vnet.subnet_id
}
```

Flow:

```
VNet Module
      │
      │ Output
      ▼
Subnet ID
      │
      ▼
AKS Module Input
```

Outputs from one module become inputs to another, enabling modular and reusable infrastructure.

---

# 5. What Kubernetes deployment strategies have you used?

Common deployment strategies include:

## Rolling Update

- Default Kubernetes strategy.
- Updates pods gradually.
- No downtime.

Example:

```
Version 1
↓↓↓

Version 2 replaces pods gradually.
```

---

## Blue-Green Deployment

Two identical environments:

```
Blue (Current)

Green (New Version)
```

Traffic is switched to Green after validation.

Benefits:

- Instant rollback
- Minimal downtime

---

## Canary Deployment

Deploy the new version to a small percentage of users.

Example:

```
90% → Version 1

10% → Version 2
```

Monitor application performance before increasing traffic.

Benefits:

- Lower deployment risk
- Early issue detection

---

## Recreate Deployment

Stops the old version completely before deploying the new version.

Suitable when both versions cannot run simultaneously.

---

# 6. How do you integrate GitHub and Kubernetes?

A typical GitOps/CI-CD workflow is:

```
Developer
      ↓
GitHub Repository
      ↓
Pull Request
      ↓
GitHub Actions / Azure DevOps
      ↓
Build
      ↓
Unit Tests
      ↓
SonarQube
      ↓
Docker Build
      ↓
Trivy Scan
      ↓
Push Image to Azure Container Registry (ACR)
      ↓
Deploy to AKS
```

Deployment options:

- Helm Charts
- Kubernetes Manifests
- Argo CD (GitOps)
- Flux CD
- Azure DevOps Kubernetes Tasks

GitHub acts as the source of truth, while the CI/CD tool builds, tests, scans, and deploys the application to Kubernetes.

---

# 7. How can you make a CI/CD pipeline lightweight and faster?

Several optimizations can significantly reduce pipeline execution time.

## Build Optimization

- Incremental builds
- Dependency caching
- Maven/Gradle cache
- Docker layer caching

## Pipeline Optimization

- Parallel execution of independent jobs
- Skip unnecessary stages
- Trigger pipelines only for changed components
- Reuse pipeline templates

## Docker Optimization

- Multi-stage builds
- Smaller base images
- Minimize Docker layers

## Infrastructure Optimization

- Reusable Terraform modules
- Run `terraform plan` only when infrastructure changes

## Testing Optimization

- Parallel test execution
- Run smoke tests before full regression
- Execute only impacted tests where possible

Result:

- Faster feedback
- Lower compute costs
- Reduced deployment time

---

# 8. Which is better: Amazon RDS or MongoDB?

This question is better framed as:

**"When would you choose Amazon RDS (Relational Database) over MongoDB (NoSQL), and vice versa?"**

There is no universally better database; the choice depends on the application requirements.

| Amazon RDS | MongoDB |
|------------|----------|
| Relational Database | NoSQL Document Database |
| Structured data | Semi-structured data |
| Fixed schema | Flexible schema |
| ACID compliant | BASE model (supports ACID for many operations) |
| SQL queries | JSON/BSON documents |
| Best for financial systems | Best for content, catalogs, and event data |

### Use Amazon RDS when:

- Banking applications
- ERP systems
- HR systems
- Financial transactions
- Inventory management

### Use MongoDB when:

- Product catalogs
- User profiles
- IoT applications
- Content management
- Real-time analytics

Interview tip:

Don't say one is better than the other. Explain that the choice depends on the application's consistency, scalability, and data model requirements.

---

# 9. How do you scale application traffic up and down?

Scaling depends on where the bottleneck exists.

## Application Scaling (Kubernetes)

Use the Horizontal Pod Autoscaler (HPA).

Example:

```
Traffic Increases
        ↓
CPU > 70%
        ↓
HPA adds more Pods

Traffic Decreases
        ↓
CPU < 30%
        ↓
HPA removes Pods
```

---

## Cluster Scaling

Use the Kubernetes Cluster Autoscaler.

When pods cannot be scheduled due to insufficient resources:

- Add worker nodes automatically.

When nodes become underutilized:

- Remove excess nodes automatically.

---

## Load Balancing

Use:

- Azure Front Door
- Azure Application Gateway
- Kubernetes Ingress Controller

These distribute incoming requests across healthy application instances.

---

## Database Scaling

- Read replicas
- Geo-replication
- Connection pooling
- Query optimization

---

## Caching

Use Azure Cache for Redis to reduce database load and improve response times.

---

## End-to-End Scaling Flow

```
Users
      ↓
Azure Front Door
      ↓
Application Gateway
      ↓
AKS Ingress
      ↓
Horizontal Pod Autoscaler
      ↓
Microservices
      ↓
Azure SQL / MongoDB
      ↓
Redis Cache
```

This architecture allows the application to automatically scale during traffic spikes and scale down during low-traffic periods, optimizing both performance and cost.




# Azure DevOps & Azure Infrastructure Interview Questions and Answers

---

# 1. What major Azure activity have you performed recently?

**Sample Answer**

Recently, I worked on provisioning a complete production-ready Azure environment using **Terraform**.

The deployment included:

- Resource Groups
- Virtual Network (VNet)
- Subnets
- Network Security Groups (NSGs)
- Azure Kubernetes Service (AKS)
- Azure Container Registry (ACR)
- Azure Key Vault
- Azure SQL Database
- Azure Monitor
- Log Analytics Workspace
- Application Gateway with WAF
- Azure Front Door
- Private Endpoints
- Managed Identities
- Azure Backup

The entire deployment was automated through **Azure DevOps YAML pipelines**, reducing infrastructure provisioning time from **2–3 days to less than 40 minutes**.

---

# 2. Do you have knowledge of Azure Virtual Desktop (AVD)?

Yes.

Azure Virtual Desktop (AVD) is Microsoft's Desktop-as-a-Service (DaaS) solution that provides secure virtual desktops and remote applications from Azure.

### Key Components

- Host Pool
- Session Hosts
- Workspace
- Application Groups
- MS Entra ID Authentication
- FSLogix User Profiles
- Azure Files / Azure NetApp Files

### Benefits

- Remote access
- Centralized management
- Secure access
- Multi-session Windows
- Auto-scaling
- Cost optimization

---

# 3. Difference between Domain Admin and Enterprise Admin

| Domain Admin | Enterprise Admin |
|--------------|------------------|
| Manages a single Active Directory domain | Manages the entire Active Directory forest |
| Can create users, groups, GPOs, and manage domain resources | Can manage all domains in the forest |
| Limited to one domain | Forest-wide administrative privileges |
| Lower privilege | Highest privilege in Active Directory |

Example:

```
Forest
 ├── Domain A
 └── Domain B
```

- **Domain Admin** → Controls only Domain A or Domain B.
- **Enterprise Admin** → Controls both domains.

---

# 4. Have you performed on-premises to Azure migration?

Yes.

I have participated in migration projects involving:

- Virtual Machines
- SQL Databases
- File Servers
- Web Applications
- Storage
- Active Directory synchronization using Azure AD Connect (Microsoft Entra Connect)

Migration tools used:

- Azure Migrate
- Azure Site Recovery (ASR)
- Azure Database Migration Service (DMS)
- AzCopy
- Azure Data Box (for large datasets)

---

# 5. How would you migrate a Virtual Machine to Azure?

Typical process:

### Assessment

- Discover on-premises VMs using Azure Migrate.
- Assess compatibility.
- Estimate Azure sizing and cost.

### Replication

- Configure Azure Site Recovery (ASR).
- Replicate VM disks continuously.

### Test Migration

- Perform a non-disruptive test failover.
- Validate application functionality.

### Cutover

- Schedule downtime.
- Perform final synchronization.
- Execute planned failover.
- Start VM in Azure.
- Validate application.
- Decommission on-premises VM if migration is successful.

---

# 6. Azure SQL Database, Key Vault, and monthly billing reports—how would you automate reporting?

A common solution:

```
Azure Cost Management
          ↓
Azure Cost Management Exports
          ↓
Storage Account
          ↓
Azure Data Factory / Logic Apps
          ↓
Azure SQL Database
          ↓
Power BI
```

Alternative approach:

- Schedule Azure Automation Runbook or Logic App.
- Query Azure Cost Management APIs.
- Store results in Azure SQL Database.
- Generate reports using Power BI.
- Email reports monthly using Logic Apps.

---

# 7. How would you monitor Azure Virtual Machines?

Monitoring tools:

- Azure Monitor
- Log Analytics Workspace
- Azure Monitor Agent (AMA)
- Microsoft Defender for Cloud
- New Relic (if integrated)

Monitor:

- CPU
- Memory
- Disk utilization
- Network
- Process health
- Service status
- Boot diagnostics
- Availability
- Security events

Configure alerts for:

- High CPU
- Low disk space
- VM unavailable
- Failed login attempts

---

# 8. Terraform state is locked and no one can run Terraform. What will you do?

First, verify whether another Terraform operation is currently running.

If the lock is stale (for example, due to a failed pipeline or interrupted execution), identify the lock ID and remove it.

Command:

```bash
terraform force-unlock LOCK_ID
```

Steps:

1. Confirm no active deployment is in progress.
2. Identify the lock ID.
3. Run `terraform force-unlock`.
4. Re-run `terraform plan`.

**Never force unlock while another Terraform process is actively running**, as this may corrupt the state.

---

# 9. Someone manually changed an NSG rule in the Azure Portal, and Terraform detects drift. What will you do?

First, determine whether the manual change was approved.

### If the change is valid

- Update the Terraform code to match the approved configuration.
- Run:

```bash
terraform plan

terraform apply
```

### If the change is unauthorized

- Revert the manual modification by running:

```bash
terraform apply
```

Terraform will restore the desired state defined in code.

---

# 10. How do you prevent configuration drift in the future?

Best practices:

- Enforce Infrastructure as Code (IaC) for all changes.
- Restrict Portal access using RBAC.
- Use Azure Policy to prevent unauthorized changes.
- Protect production subscriptions.
- Require Pull Requests and approvals.
- Schedule regular `terraform plan` checks to detect drift.
- Enable activity logs and alerts for manual modifications.

---

# 11. After a deployment, the Application Gateway returns 502 or 503 errors. How would you troubleshoot?

### Step 1: Check backend health

Application Gateway → Backend Health

Verify whether backend targets are healthy.

### Step 2: Verify Kubernetes

```bash
kubectl get pods

kubectl get svc

kubectl get ingress
```

Ensure pods are running and services are available.

### Step 3: Validate health probes

- Probe path
- Port
- Protocol
- Timeout

### Step 4: Check application logs

- Pod logs
- Application logs
- Container logs

### Step 5: Verify networking

- NSGs
- Route tables
- Firewall rules

### Common causes

- Application not listening on expected port
- Incorrect health probe configuration
- Backend service unavailable
- SSL certificate issues
- DNS resolution problems

---

# 12. How would you design a Blue-Green deployment pipeline on AKS?

Architecture:

```
Production Traffic
         │
         ▼
Ingress Controller
      │
 ┌────┴────┐
 │         │
Blue      Green
(Current) (New)
```

Pipeline:

```
Build
      ↓
Test
      ↓
Deploy Green
      ↓
Smoke Test
      ↓
Approval
      ↓
Switch Traffic
      ↓
Monitor
      ↓
Remove Blue (after validation)
```

Traffic switching can be performed using:

- Kubernetes Ingress
- Azure Application Gateway Ingress Controller (AGIC)
- Azure Front Door

Benefits:

- Zero downtime
- Instant rollback
- Safer production deployments

---

# 13. Azure Monitor is generating noisy false-positive alerts. How would you fine-tune them?

Steps:

1. Identify frequently triggered alerts.
2. Verify whether they correspond to real issues.
3. Review alert thresholds.
4. Increase evaluation windows if alerts are too sensitive.
5. Filter non-production resources where appropriate.
6. Suppress duplicate alerts.
7. Configure action groups appropriately.
8. Validate with operations and application teams.
9. Monitor after tuning to ensure genuine incidents are still detected.

Goal:

Reduce alert fatigue while maintaining effective monitoring.

---

# 14. Can you write Terraform code to deploy a web application?

Basic example:

```terraform
resource "azurerm_linux_web_app" "app" {
  name                = "demo-webapp"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  service_plan_id     = azurerm_service_plan.plan.id

  site_config {
    always_on = true
  }
}
```

In production, this would typically include:

- App Service Plan
- Managed Identity
- Key Vault references
- Diagnostic settings
- Private Endpoint
- Application Insights
- Custom domain
- SSL certificate

---

# 15. Write a Dockerfile to build an application

Example for a Java Spring Boot application:

```dockerfile
# Build Stage
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package

# Runtime Stage
FROM eclipse-temurin:17-jre

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

Benefits:

- Smaller image
- Faster deployments
- Better security
- Reduced attack surface

---

# 16. Difference between a Subnet and a Gateway Subnet

| Subnet | Gateway Subnet |
|--------|----------------|
| Hosts application resources such as VMs, AKS, and App Gateway | Dedicated subnet for Azure VPN Gateway or ExpressRoute Gateway |
| User-defined | Reserved specifically for gateway resources |
| Can contain application workloads | Must not contain other resources |
| General-purpose | Used only for gateway deployment |

---

# 17. Can two VNets be peered across different subscriptions?

Yes.

Azure supports VNet Peering across:

- Different subscriptions
- Different resource groups
- Different regions (Global VNet Peering)

Requirements:

- Appropriate permissions on both VNets
- Non-overlapping IP address spaces

---

# 18. I want to restrict inbound and outbound traffic for all networks in my tenant. What is the best approach?

Recommended solution:

- Azure Firewall
- Hub-and-Spoke architecture
- Network Security Groups (NSGs)
- Azure Firewall Policies
- User Defined Routes (UDRs)
- Azure Policy
- Private Endpoints where applicable

This provides centralized control over network traffic and security.

---

# 19. Difference between Azure Load Balancer and Application Gateway

| Azure Load Balancer | Azure Application Gateway |
|---------------------|---------------------------|
| Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| No SSL termination | Supports SSL termination |
| No WAF | Built-in Web Application Firewall (WAF) |
| Basic traffic distribution | URL/path-based routing |
| No cookie affinity | Supports cookie-based session affinity |

---

# 20. Difference between Vertical and Horizontal Scaling

| Vertical Scaling | Horizontal Scaling |
|------------------|--------------------|
| Increase CPU/RAM of an existing server | Add more servers or instances |
| Limited by hardware | Scales almost indefinitely |
| May require downtime | Usually supports zero downtime |
| Scale up/down | Scale out/in |

For cloud-native applications, **horizontal scaling** is generally preferred.

---

# 21. How can you securely access a Virtual Machine?

Methods:

- Azure Bastion (preferred)
- Point-to-Site (P2S) VPN
- Site-to-Site (S2S) VPN
- ExpressRoute
- Just-In-Time (JIT) VM Access
- Private IP with VPN
- Azure Virtual Desktop (for end users)

Avoid exposing RDP (3389) or SSH (22) directly to the internet.

---

# 22. Employees work remotely and need secure access to the client environment. What is the best solution?

Recommended architecture:

```
Remote Users
      │
      ▼
Microsoft Entra ID
      │
      ▼
MFA
      │
      ▼
Point-to-Site VPN / Azure Virtual Desktop
      │
      ▼
Private Azure Environment
```

Additional security:

- Conditional Access
- Microsoft Defender for Endpoint
- Azure Bastion (for administrators)
- Private Endpoints

---

# 23. Difference between Site-to-Site VPN and Point-to-Site VPN

| Site-to-Site (S2S) | Point-to-Site (P2S) |
|--------------------|---------------------|
| Connects two networks | Connects individual users |
| Router-to-router connection | User device to Azure |
| Used for branch offices | Used for remote employees |
| Always-on connection | User initiates connection |

---

# 24. What is Microsoft Entra ID? Difference between Static and Dynamic Groups

Microsoft Entra ID (formerly Azure Active Directory) is Microsoft's cloud identity and access management service.

### Static Group

- Members are manually added or removed.
- Suitable for small or fixed teams.

### Dynamic Group

- Membership is based on rules.
- Users are added or removed automatically.

Example rule:

```
Department = IT
```

All users with the IT department attribute become members automatically.

---

# 25. MFA is enabled, but you need to exclude some users. How would you do it?

Use **Conditional Access Policies**.

Steps:

1. Create or edit the Conditional Access policy.
2. Enable MFA for the target users.
3. Under **Exclude**, add the users or groups that should not require MFA.
4. Apply the policy.

Exclusions should be limited and documented, as they reduce security.

---

# 26. Have you worked with on-premises Active Directory (Legacy AD)?

Yes.

Typical responsibilities included:

- User and group management
- Organizational Units (OUs)
- Group Policy Objects (GPOs)
- DNS and DHCP
- Active Directory replication
- Trust relationships
- Domain Controllers
- Azure AD Connect (Microsoft Entra Connect) synchronization

---

# 27. Azure Virtual Desktop (AVD): Difference between Pooled and Personal Host Pools

| Pooled Host Pool | Personal Host Pool |
|------------------|--------------------|
| Multiple users share session hosts | One dedicated VM per user |
| Lower infrastructure cost | Higher infrastructure cost |
| Best for task workers or call centers | Best for developers or power users |
| Sessions are load-balanced | Dedicated persistent desktop |
| Better resource utilization | Personalized user experience |

### When to choose each

**Pooled Host Pool**
- Call centers
- Shared office users
- Standard business applications

**Personal Host Pool**
- Developers
- Engineers
- Designers
- Users requiring administrative rights or persistent desktops
