# Azure DevOps, Azure CI/CD & Azure Services Interview Questions and Answers

---

# 1. Explain your experience with Azure DevOps YAML Pipelines.

I have hands-on experience designing and maintaining **Azure DevOps YAML Pipelines** for building, testing, securing, and deploying applications and infrastructure.

## Technologies Deployed

- Java (Spring Boot)
- .NET Core
- React
- Angular
- Node.js
- Python
- Docker
- Kubernetes (AKS)
- Terraform

## Pipeline Features

- Multi-stage YAML pipelines
- Reusable YAML templates
- Variable Groups
- Azure Key Vault integration
- Manual approvals
- Environment-specific deployments
- Parallel jobs
- Pipeline caching
- Artifact publishing
- Release gates

## Security Integration

- SonarQube (SAST)
- tfsec / Checkov (Terraform Security)
- Trivy (Container Scanning)
- GitLeaks (Secret Scanning)
- OWASP ZAP (DAST)

## Typical Pipeline Flow

```
Developer Commit
        │
Pull Request
        │
Build
        │
Unit Tests
        │
SonarQube
        │
Terraform Validate
        │
tfsec
        │
Docker Build
        │
Trivy Scan
        │
Push to Azure Container Registry (ACR)
        │
Deploy to AKS
        │
Smoke Test
        │
Approval
        │
Production
```

Using YAML pipelines helped us standardize deployments, improve consistency, and reduce release time from **4 days to approximately 4 hours**.

---

# 2. How would you deploy a React application to Azure Static Web Apps?

Deployment steps:

### Step 1

Push the React application to GitHub or Azure Repos.

### Step 2

Create an **Azure Static Web App**.

### Step 3

Connect the repository.

### Step 4

Configure the build settings.

Example:

```
App Location: /

Build Location: build

Output Location: build
```

### Step 5

Azure automatically creates a CI/CD workflow.

Pipeline:

```
Developer Push
       │
GitHub / Azure Repos
       │
Build React Application
       │
Run npm install
       │
Run npm build
       │
Deploy to Azure Static Web Apps
```

Each commit automatically triggers a deployment.

---

# 3. Difference between Azure Static Web Apps and Azure App Service

| Azure Static Web Apps | Azure App Service |
|------------------------|-------------------|
| Designed for static websites | Designed for full web applications |
| HTML, CSS, JavaScript, React, Angular, Vue | .NET, Java, Node.js, Python, PHP, etc. |
| Built-in global CDN | No built-in CDN |
| Automatic GitHub/Azure DevOps integration | Supports CI/CD but requires configuration |
| Supports Azure Functions for APIs | Hosts complete backend applications |
| Optimized for frontend workloads | Suitable for frontend and backend workloads |

### Use Azure Static Web Apps for

- React
- Angular
- Vue
- Static websites
- Documentation sites

### Use Azure App Service for

- Java APIs
- .NET applications
- Node.js backends
- Python web applications
- Enterprise web applications

---

# 4. How do you secure secrets in CI/CD pipelines?

Secrets should **never** be stored in source code or pipeline YAML files.

## Best Practices

- Store secrets in Azure Key Vault.
- Use Managed Identity or Service Connections for authentication.
- Retrieve secrets at runtime.
- Mark pipeline variables as secret.
- Restrict access using RBAC.
- Rotate secrets regularly.
- Mask secrets in pipeline logs.
- Avoid exposing secrets in output variables or scripts.

Pipeline example:

```
Pipeline
      │
Managed Identity
      │
Azure Key Vault
      │
Retrieve Secrets
      │
Deploy Application
```

Typical secrets:

- Database passwords
- API keys
- Storage account keys
- Certificates
- Connection strings

---

# 5. What are Managed Identities in Azure?

A **Managed Identity** is an Azure-managed identity that allows Azure resources to authenticate to other Azure services without storing credentials.

Azure automatically manages the lifecycle of the identity and its credentials.

## Types

### System-Assigned Managed Identity

- Created with the Azure resource.
- Deleted when the resource is deleted.
- One identity per resource.

### User-Assigned Managed Identity

- Independent Azure resource.
- Can be attached to multiple Azure resources.
- Managed separately.

## Common Use Cases

- Access Azure Key Vault
- Access Azure Storage
- Authenticate to Azure SQL Database
- Pull images from Azure Container Registry (ACR)
- Access Azure Monitor

Benefits:

- No credential management.
- Automatic credential rotation.
- Improved security.

---

# 6. How would you implement Pull Request (PR) validation?

PR validation ensures that code meets quality and security standards before merging.

Typical checks:

- Build validation
- Unit tests
- SonarQube Quality Gate
- Dependency scanning
- Terraform validation
- tfsec / Checkov
- Code review approval
- Branch protection rules

Pipeline:

```
Developer
      │
Pull Request
      │
Build
      │
Unit Tests
      │
SonarQube
      │
Terraform Validate
      │
Security Scan
      │
Approval
      │
Merge
```

Merge is blocked if any required check fails.

---

# 7. Explain a Multi-Stage Deployment Pipeline.

A multi-stage pipeline separates deployment into logical environments.

Example:

```
Build Stage
      │
Test Stage
      │
Security Stage
      │
Deploy to Dev
      │
Deploy to UAT
      │
Approval
      │
Deploy to Production
```

### Build Stage

- Compile application
- Run unit tests
- Publish artifacts

### Security Stage

- SonarQube
- Dependency scan
- Terraform security scan
- Container scan

### Deployment Stages

- Development
- UAT
- Pre-Production
- Production

Each stage has its own:

- Variables
- Approvals
- Service Connections
- Deployment strategy

Benefits:

- Better governance
- Controlled releases
- Easy rollback
- Environment isolation

---

# 8. How do you implement rollback strategies?

Rollback depends on the deployment approach.

## Kubernetes

- Rolling Update rollback

```bash
kubectl rollout undo deployment/app
```

## Blue-Green Deployment

- Switch traffic back to the previous (Blue) environment.

## Canary Deployment

- Route all traffic back to the stable version.

## Azure App Service

- Swap back to the previous deployment slot.

## Infrastructure

- Revert Terraform changes using version-controlled code and apply the previous known-good configuration.

Key practices:

- Version artifacts.
- Maintain deployment history.
- Automate rollback procedures.
- Monitor deployments before promoting changes.

---

# 9. What is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is the practice of provisioning and managing infrastructure using code instead of manual configuration.

Common IaC tools:

- Terraform
- Azure Bicep
- ARM Templates
- Pulumi

Benefits:

- Automation
- Version control
- Consistency
- Repeatability
- Faster deployments
- Reduced manual errors

Typical workflow:

```
Developer
      │
Terraform Code
      │
Git
      │
CI/CD Pipeline
      │
Azure Infrastructure
```

---

# 10. How do you monitor Azure applications?

Monitoring stack:

- Azure Monitor
- Application Insights
- Log Analytics Workspace
- Azure Alerts
- New Relic
- Prometheus
- Grafana

Monitor:

- Response time
- Request count
- Failure rate
- Availability
- CPU
- Memory
- Database performance
- Dependency calls
- Exceptions
- Custom metrics

Alert examples:

- CPU > 80%
- Memory > 85%
- HTTP 5xx errors
- High response time
- Pod failures
- Disk space utilization

---

# 11. What quality gates would you implement?

Quality gates ensure that only high-quality and secure code is deployed.

Typical gates include:

### Code Quality

- SonarQube Quality Gate
- Code coverage threshold
- Code duplication threshold
- Maintainability rating

### Security

- No critical vulnerabilities
- Dependency scanning
- Secret scanning
- IaC security scanning
- Container image scanning

### Testing

- Unit tests passed
- Integration tests passed
- Smoke tests passed

### Infrastructure

- Terraform validation
- Terraform plan review
- Policy compliance

### Deployment

- Manual approval for production
- Successful health checks

Example pipeline:

```
Build
     │
Unit Test
     │
SonarQube
     │
Security Scan
     │
Terraform Validate
     │
Deploy to Test
     │
Smoke Test
     │
Approval
     │
Production
```

---

# 12. Explain Azure Container Apps.

**Azure Container Apps (ACA)** is a **serverless container platform** that allows you to run containerized applications without managing Kubernetes clusters or virtual machines.

It is built on Kubernetes internally but abstracts the underlying infrastructure.

## Features

- Serverless container hosting
- Automatic scaling
- Scale to zero
- Revision management
- HTTPS by default
- Built-in ingress
- Dapr integration
- Event-driven scaling using KEDA
- Managed environment

## Supported Workloads

- APIs
- Microservices
- Background jobs
- Event-driven applications
- HTTP services

## Typical Architecture

```
Users
      │
      ▼
Azure Front Door
      │
      ▼
Azure Container Apps
      │
      ▼
Azure SQL Database
      │
      ▼
Azure Key Vault
```

## Azure Container Apps vs AKS

| Azure Container Apps | Azure Kubernetes Service (AKS) |
|-----------------------|-------------------------------|
| Fully managed serverless platform | Managed Kubernetes cluster |
| No cluster management | Full control over Kubernetes |
| Scale to zero | Minimum node count required (unless using virtual nodes or other optimizations) |
| Best for small to medium microservices | Best for complex enterprise Kubernetes workloads |
| Simpler operations | Greater flexibility and customization |

### When to use Azure Container Apps

- Lightweight microservices
- Event-driven applications
- APIs
- Background workers
- Teams that want container benefits without managing Kubernetes

### When to use AKS

- Large-scale microservices
- Advanced networking requirements
- Service mesh
- Custom Kubernetes operators
- Stateful workloads
- Enterprise-grade Kubernetes deployments


# Azure, Kubernetes & Terraform Interview Questions and Answers

---

# 1. What are the key points to check when an application is running slowly?

When troubleshooting a slow application, I follow a layered approach to identify the bottleneck.

## Step 1: Check Application Health

- Response time
- Error rate (4xx/5xx)
- Request throughput
- Application logs
- Exception logs

Tools:

- Azure Monitor
- Application Insights
- New Relic

---

## Step 2: Check Infrastructure

- CPU utilization
- Memory utilization
- Disk I/O
- Network latency

For Virtual Machines:

```bash
top
htop
iostat
vmstat
```

---

## Step 3: Check Kubernetes (if applicable)

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>
```

Check for:

- CrashLoopBackOff
- OOMKilled
- Pending pods
- High CPU/Memory usage
- Restart count

---

## Step 4: Check Database

- Slow queries
- Connection pool usage
- Locks and deadlocks
- CPU utilization
- Storage performance

---

## Step 5: Check Networking

- DNS resolution
- Application Gateway health
- Azure Front Door health
- Firewall rules
- NSGs
- Latency between services

---

## Step 6: Check Monitoring Metrics

Review dashboards for:

- CPU
- Memory
- Response time
- Request count
- Failed requests

---

## Root causes are commonly:

- High CPU or memory utilization
- Database bottlenecks
- Resource contention
- Poor application code
- Network latency
- Incorrect autoscaling configuration

---

# 2. How do you ensure high availability for an application in Azure and Kubernetes?

High availability is achieved by eliminating single points of failure.

## Azure Level

- Availability Zones
- Zone-Redundant Services
- Azure Front Door
- Application Gateway
- Azure Load Balancer
- Geo-Replication (Azure SQL)
- Geo-Redundant Storage (GRS)

---

## Kubernetes Level

- Multiple replicas

Example:

```yaml
replicas: 3
```

- Rolling Updates
- Pod Disruption Budgets (PDBs)
- Readiness Probes
- Liveness Probes
- Horizontal Pod Autoscaler (HPA)
- Cluster Autoscaler
- Multiple node pools

---

## Overall Architecture

```
Users
      │
Azure Front Door
      │
Application Gateway
      │
AKS Cluster
      │
Multiple Pods
      │
Azure SQL (Geo-Replication)
```

This architecture ensures continuous availability even if a node, zone, or region experiences issues.

---

# 3. What networking do we use in an Azure Landing Zone?

A Landing Zone typically follows a **Hub-and-Spoke architecture**.

Components include:

- Virtual Networks (VNets)
- Hub VNet
- Spoke VNets
- Azure Firewall
- Azure Bastion
- VPN Gateway
- ExpressRoute
- Network Security Groups (NSGs)
- User Defined Routes (UDRs)
- Private Endpoints
- Private DNS Zones

Example:

```
Hub VNet
│
├── Azure Firewall
├── Bastion
├── VPN Gateway
└── Shared Services

      │

Spoke VNets

├── Production
├── UAT
├── Development
└── Shared Services
```

This design provides centralized security, governance, and connectivity.

---

# 4. How does traffic flow in a Hub-and-Spoke network?

Typical traffic flow:

```
Internet
      │
Azure Front Door
      │
Application Gateway
      │
Azure Firewall
      │
Hub VNet
      │
VNet Peering
      │
Spoke VNet
      │
AKS / App Service / VM
```

### Internal Communication

```
Spoke A
     │
     ▼
Hub
     ▲
     │
Spoke B
```

Traffic between spokes is routed through the Hub, allowing centralized inspection, logging, and policy enforcement.

---

# 5. How do you ensure zero-downtime networking?

Zero downtime is achieved through redundancy and resilient deployment strategies.

Key practices:

- Deploy resources across multiple Availability Zones.
- Use Azure Front Door for global load balancing and failover.
- Use Application Gateway with multiple backend instances.
- Enable health probes.
- Configure redundant VPN or ExpressRoute connections.
- Use rolling, blue-green, or canary deployments.
- Implement Kubernetes readiness and liveness probes.
- Enable autoscaling for applications and infrastructure.

Example:

```
Users
      │
Azure Front Door
      │
Application Gateway
      │
AKS (Multiple Pods)
      │
Multiple Availability Zones
```

If one instance becomes unavailable, traffic is automatically routed to healthy instances.

---

# 6. What is a Virtual Network (VNet)?

A **Virtual Network (VNet)** is the fundamental networking construct in Azure.

It provides a logically isolated network for Azure resources, similar to a traditional on-premises network.

A VNet enables:

- Secure communication between Azure resources.
- Communication with on-premises networks via VPN or ExpressRoute.
- Internet connectivity (when required).
- Network segmentation using subnets.
- Traffic filtering using NSGs and Azure Firewall.

Example:

```
VNet

├── Web Subnet
├── Application Subnet
├── Database Subnet
└── Gateway Subnet
```

---

# 7. How do you assign different subnets for Azure services?

Each Azure service should be deployed into an appropriate subnet based on security and networking requirements.

Example:

| Azure Service | Recommended Subnet |
|---------------------------|------------------|
| Virtual Machines | VM Subnet |
| AKS Nodes | AKS Subnet |
| Application Gateway | Dedicated App Gateway Subnet |
| Azure Firewall | AzureFirewallSubnet (reserved name) |
| VPN Gateway | GatewaySubnet (reserved name) |
| Azure Bastion | AzureBastionSubnet (reserved name) |
| Private Endpoints | Private Endpoint Subnet |
| Databases (Private Access) | Database Subnet |

Benefits:

- Better security
- Easier network management
- Traffic isolation
- Granular NSG application

---

# 8. How do you check backoff issues in Kubernetes?

The question is commonly asked as:

**"How do you troubleshoot CrashLoopBackOff or ImagePullBackOff in Kubernetes?"**

### Step 1

Check pod status:

```bash
kubectl get pods
```

Example output:

```
CrashLoopBackOff

ImagePullBackOff
```

---

### Step 2

Describe the pod:

```bash
kubectl describe pod <pod-name>
```

Look for:

- Events
- Restart count
- Probe failures
- Image pull errors

---

### Step 3

Check logs:

```bash
kubectl logs <pod-name>
```

For previous container logs:

```bash
kubectl logs <pod-name> --previous
```

---

### Common Causes

- Application startup failure
- Incorrect image
- Missing secrets
- Wrong environment variables
- Failed readiness or liveness probes
- OOMKilled
- Configuration errors

---

# 9. How do you make Terraform deployments more scalable?

Scalable Terraform design focuses on modularity, reusability, and automation.

## Use Reusable Modules

```
modules/

vnet/

aks/

sql/

storage/
```

---

## Separate Environments

```
environments/

dev/

uat/

prod/
```

Each environment has its own:

- Backend
- Variables
- State file

---

## Use `for_each` and `count`

Instead of duplicating resources:

```terraform
resource "azurerm_resource_group" "rg" {

  for_each = var.resource_groups

  name     = each.key
  location = each.value
}
```

---

## Remote State

Store state in Azure Storage.

Benefits:

- Collaboration
- State locking
- Versioning

---

## CI/CD Integration

```
Git
    │
Azure DevOps
    │
terraform fmt
    │
terraform validate
    │
tfsec
    │
terraform plan
    │
Approval
    │
terraform apply
```

---

## Version Modules

```
v1.0.0

v1.1.0

v2.0.0
```

This enables controlled upgrades and rollbacks.

---

# 10. What is Azure Key Vault, and why is it important?

**Azure Key Vault** is a managed Azure service used to securely store and control access to sensitive information.

It stores:

- Secrets
- Passwords
- API keys
- Connection strings
- Certificates
- Encryption keys

## Why is it important?

### Centralized Secret Management

All sensitive information is stored in one secure location instead of application code or configuration files.

---

### Enhanced Security

- Encryption at rest
- Encryption in transit
- RBAC integration
- Microsoft Entra ID authentication
- Managed Identity support

---

### Secret Rotation

Secrets and certificates can be rotated without changing application code.

---

### Audit and Compliance

Every access to a secret is logged, helping meet security and compliance requirements.

---

### CI/CD Integration

Pipelines retrieve secrets securely at runtime.

Example flow:

```
Azure DevOps Pipeline
          │
Managed Identity / Service Connection
          │
Azure Key Vault
          │
Retrieve Secret
          │
Deploy Application
```

### Best Practices

- Never hardcode secrets.
- Use Managed Identities whenever possible.
- Restrict access with RBAC.
- Enable soft delete and purge protection.
- Rotate secrets regularly.
- Monitor access using Azure Monitor and Activity Logs.
