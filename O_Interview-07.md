# TDCIS
# Senior Azure DevOps Engineer Interview Guide (10+ Years Experience)

> **Part 1 (Q1-Q20)**

---

# 1. Tell me about yourself, your experience, technical expertise, and roles & responsibilities.

**Interview Answer**

"I have over 10 years of experience as a Senior Azure DevOps and Cloud Engineer specializing in Azure Infrastructure, Azure DevOps, Terraform, Kubernetes, Docker, Azure Networking, Security, and CI/CD automation.

Throughout my career, I have designed enterprise Azure Landing Zones, implemented Infrastructure as Code using Terraform, automated CI/CD pipelines using Azure DevOps and GitHub Actions, deployed containerized applications on AKS, and implemented monitoring using Azure Monitor, Log Analytics, Prometheus, and Grafana.

In my current role, I'm responsible for designing cloud infrastructure, automating deployments, implementing governance, managing Azure networking, securing cloud resources, troubleshooting production issues, optimizing cloud costs, and mentoring junior engineers.

I closely collaborate with developers, security teams, architects, and operations teams to deliver secure and highly available cloud platforms."

---

# 2. Have you been part of creating an Azure Landing Zone?

**Interview Answer**

Yes.

In my current project, I participated in designing and implementing an Enterprise Azure Landing Zone following Microsoft's Cloud Adoption Framework.

My responsibilities included:

- Subscription Design
- Management Groups
- RBAC
- Azure Policy
- Hub-Spoke Networking
- Azure Firewall
- Private DNS
- Private Endpoints
- Azure Monitor
- Log Analytics
- Terraform Automation
- Azure DevOps Integration

The Landing Zone became the standard platform for onboarding multiple application teams.

---

# 3. What are the steps to create an Azure Landing Zone from scratch?

**Interview Answer**

The process I usually follow is:

1. Create Management Groups.
2. Design Subscription hierarchy.
3. Integrate Azure AD.
4. Define RBAC model.
5. Create Resource Group strategy.
6. Deploy Hub Network.
7. Deploy Spoke VNets.
8. Configure VNet Peering.
9. Deploy Azure Firewall.
10. Configure Route Tables (UDRs).
11. Configure NSGs.
12. Deploy Private DNS Zones.
13. Configure Private Endpoints.
14. Enable Azure Policy.
15. Enable Defender for Cloud.
16. Configure Log Analytics.
17. Configure Azure Monitor.
18. Configure Backup and DR.
19. Configure Terraform Remote Backend.
20. Automate deployment using Azure DevOps.

---

# 4. Explain Azure Landing Zone briefly.

**Interview Answer**

Azure Landing Zone is a pre-configured Azure environment that provides a secure, scalable, and governed foundation for deploying enterprise workloads.

It includes:

- Identity
- Networking
- Security
- Governance
- Monitoring
- Resource Organization
- Connectivity
- Automation

Instead of building infrastructure from scratch for every project, all application teams deploy onto the Landing Zone.

---

# 5. Apart from Hub-and-Spoke networking, what other components are part of a Landing Zone?

**Interview Answer**

Besides networking, a Landing Zone includes:

- Azure AD
- Management Groups
- Subscriptions
- Resource Groups
- RBAC
- Azure Policy
- Azure Firewall
- Private DNS
- Private Endpoints
- Key Vault
- Azure Monitor
- Log Analytics
- Microsoft Defender for Cloud
- Backup
- Cost Management
- Azure Advisor
- Azure Bastion
- CI/CD Integration

Networking is only one component of the overall Landing Zone architecture.

---

# 6. What governance controls do you implement in a Landing Zone?

**Interview Answer**

Governance is implemented using:

- Azure Policy
- Management Groups
- RBAC
- Resource Locks
- Naming Standards
- Tagging Policy
- Cost Budgets
- Defender for Cloud
- Activity Logs
- Diagnostic Settings
- Azure Blueprints (legacy) / Policy Initiatives

Examples:

- Allow only approved Azure regions.
- Restrict Public IP creation.
- Enforce tagging.
- Enforce HTTPS only.
- Restrict VM SKUs.
- Require encryption.

---

# 7. How do you implement IAM and RBAC in a Landing Zone?

**Interview Answer**

I always follow the Principle of Least Privilege.

Roles are assigned through Azure AD Groups instead of individual users.

Typical hierarchy:

- Management Group
- Subscription
- Resource Group
- Resource

Examples:

Developers:
- Contributor (Dev only)

Operations:
- Contributor

Security Team:
- Security Reader
- Key Vault Administrator

Application Team:
- Reader

CI/CD Pipeline:
- Contributor only for deployment resources

Production access requires approval.

---

# 8. How do you approach networking when a client has hundreds or thousands of VNets?

**Interview Answer**

For a small environment, Hub-and-Spoke works well.

For enterprise-scale environments with hundreds or thousands of VNets, I prefer Azure Virtual WAN.

Benefits:

- Better scalability
- Simplified routing
- Global connectivity
- Reduced peering complexity
- Centralized firewall integration
- Lower operational overhead

---

# 9. Hub-and-Spoke has peering limits. How will you overcome that limitation?

**Interview Answer**

Hub-and-Spoke becomes difficult to manage as the number of VNets grows.

Instead of manually managing thousands of peerings, I would migrate to Azure Virtual WAN.

Virtual WAN provides:

- Central routing
- Automatic connectivity
- Simplified management
- Multi-region support
- Better scalability

---

# 10. Which Azure service helps overcome Hub-and-Spoke scaling limitations?

**Interview Answer**

Azure Virtual WAN.

It provides:

- Centralized connectivity
- Simplified routing
- Global transit network
- Integrated VPN
- ExpressRoute
- Azure Firewall Manager
- Branch connectivity

For enterprise-scale networking, Virtual WAN is Microsoft's recommended architecture.

---

# 11. How do you implement firewall architecture in Azure?

**Interview Answer**

I deploy Azure Firewall in the Hub VNet.

All application VNets are connected to the Hub.

Traffic is forced through Azure Firewall using User Defined Routes.

Firewall policies include:

- Network Rules
- Application Rules
- NAT Rules
- Threat Intelligence
- DNS Proxy
- Logging

Azure Firewall logs are forwarded to Log Analytics.

---

# 12. How do you ensure all outbound traffic passes through Azure Firewall?

**Interview Answer**

I create a Route Table with:

Destination:

```
0.0.0.0/0
```

Next Hop:

```
Virtual Appliance
```

Next Hop IP:

```
Azure Firewall Private IP
```

The Route Table is associated with workload subnets.

This forces all outbound traffic through Azure Firewall.

---

# 13. How do you design centralized egress traffic through Azure Firewall?

**Interview Answer**

The Hub network contains Azure Firewall.

Every spoke subnet has a Route Table directing Internet traffic to Azure Firewall.

Azure Firewall then:

- Inspects traffic
- Applies security rules
- Logs traffic
- Performs SNAT
- Sends approved traffic to Internet

This provides centralized outbound security.

---

# 14. What is the role of User Defined Routes (UDRs)?

**Interview Answer**

UDRs override Azure's default routing behavior.

They allow administrators to control traffic paths.

Common use cases:

- Force traffic through Firewall
- Route traffic to NVA
- Hybrid connectivity
- ExpressRoute
- VPN Gateway
- Custom routing

Without UDRs, Azure uses system routes.

---

# 15. Where do you associate UDRs?

**Interview Answer**

UDRs are associated with Subnets.

Examples:

- AKS Subnet
- VM Subnet
- Application Subnet

They cannot be associated directly with:

- VNet
- Resource Group
- Subscription

---

# 16. Explain the complete packet flow when traffic is forced through Azure Firewall.

**Interview Answer**

Traffic flow:

1. Application sends request.
2. Packet reaches Subnet.
3. UDR redirects traffic to Azure Firewall.
4. Firewall checks Network Rules.
5. Firewall checks Application Rules.
6. Threat Intelligence inspection.
7. SNAT performed.
8. Packet exits to Internet.
9. Response returns through Firewall.
10. Firewall forwards response to application.

This ensures every packet is inspected.

---

# 17. How do you make sure Azure uses your custom routes instead of system routes?

**Interview Answer**

Azure automatically prefers User Defined Routes over System Routes.

To ensure this:

- Associate Route Table with the subnet.
- Configure the correct next hop.
- Verify Effective Routes from the NIC.
- Confirm no conflicting BGP routes.

I always validate routing using Effective Routes in Azure Portal.

---

# 18. Which CI/CD tools have you worked on besides Azure DevOps?

**Interview Answer**

Besides Azure DevOps, I have worked with:

- GitHub Actions
- Jenkins
- GitLab CI/CD
- Argo CD
- Helm
- Terraform Cloud

Azure DevOps remains my primary CI/CD platform because of its native Azure integration and enterprise governance capabilities.

---

# 19. How do you fetch secrets from Azure Key Vault in Azure DevOps Pipeline?

**Interview Answer**

The pipeline authenticates using a Service Connection.

The Service Connection uses either:

- Service Principal
- Managed Identity (Self-hosted agent)

Azure DevOps retrieves secrets using the Azure Key Vault task before deployment.

The secrets are exposed as secure pipeline variables and are never hardcoded in the repository or pipeline YAML.

---

# 20. How is the connection established between Azure DevOps and Azure Key Vault?

**Interview Answer**

The connection flow is:

1. Azure DevOps Pipeline starts.
2. Pipeline uses an Azure Service Connection.
3. Service Connection authenticates using a Service Principal or Managed Identity.
4. Azure AD issues an access token.
5. Azure Key Vault validates the token.
6. Secrets are securely retrieved.
7. Pipeline consumes the secrets during deployment.

For production environments, I prefer Managed Identity with a Self-hosted Agent whenever possible, as it eliminates the need to manage client secrets and improves security.

# Senior Azure DevOps Engineer Interview Guide (10+ Years Experience)

> **Part 2 (Q21–Q40)**

---

# 21. Explain the Service Connection authentication flow.

**Interview Answer**

"In Azure DevOps, a Service Connection securely authenticates the pipeline to Azure.

The authentication flow is:

1. Pipeline starts.
2. Azure DevOps uses the configured Service Connection.
3. The Service Connection authenticates using a **Service Principal** or **Managed Identity**.
4. Azure AD validates the identity and issues an OAuth access token.
5. Azure DevOps uses this token to call Azure Resource Manager (ARM).
6. ARM authorizes the request based on the assigned RBAC roles.
7. Resources are created or updated.

I always assign the minimum required RBAC permissions to the Service Principal and avoid using the Owner role."

---

# 22. Explain your complete CI/CD architecture.

**Interview Answer**

"Our CI/CD architecture follows GitOps and Infrastructure as Code principles.

Flow:

- Developers commit code to Azure Repos.
- Pull Request triggers validation pipeline.
- Code Review and Approval.
- Build Stage compiles the application.
- Unit Tests execute.
- SonarQube performs SAST analysis.
- Dependency vulnerability scan runs.
- Docker image is built.
- Image is pushed to Azure Container Registry (ACR).
- Terraform provisions or updates infrastructure.
- Helm deploys the application to AKS.
- Azure Key Vault provides secrets.
- Smoke tests validate deployment.
- Manual approval is required before Production.
- Azure Monitor and Application Insights continuously monitor the application."

---

# 23. Explain every step in your Azure DevOps Pipeline.

**Interview Answer**

"My production pipeline consists of the following stages:

1. Source Code Checkout
2. Restore Dependencies
3. Build
4. Unit Testing
5. Code Coverage Validation
6. SonarQube Analysis
7. Security Scan
8. Docker Image Build
9. Push Image to ACR
10. Terraform Init
11. Terraform Validate
12. Terraform Plan
13. Manual Approval (Production)
14. Terraform Apply
15. Helm Deployment
16. Smoke Test
17. Health Check
18. Notification
19. Monitoring"

---

# 24. Where do you deploy your .NET application?

**Interview Answer**

"It depends on the application architecture.

For microservices, I deploy .NET applications on Azure Kubernetes Service (AKS).

For lightweight web applications, Azure App Service is preferred.

For serverless APIs, Azure Functions are used.

In my current project, all .NET microservices are containerized using Docker and deployed to AKS using Helm charts."

---

# 25. What are the stages in your application deployment pipeline?

**Interview Answer**

Typical stages:

- Build
- Unit Test
- Code Quality (SonarQube)
- Security Scan
- Docker Build
- Push Image
- Terraform Plan
- Approval
- Terraform Apply
- Deploy to AKS
- Smoke Test
- Integration Test
- Production Deployment
- Post-Deployment Validation
- Monitoring

---

# 26. What SAST tools have you implemented?

**Interview Answer**

"I have implemented:

- SonarQube
- Checkmarx
- Microsoft Defender for DevOps
- GitHub CodeQL
- Snyk Code

SonarQube is our primary SAST tool because it integrates well with Azure DevOps and enforces Quality Gates."

---

# 27. Explain how SonarQube works in your pipeline.

**Interview Answer**

"After the application is built, SonarQube scans the source code for:

- Bugs
- Code Smells
- Security Vulnerabilities
- Duplicated Code
- Maintainability Issues
- Code Coverage

A Quality Gate is evaluated.

If the Quality Gate fails (for example, code coverage is below 80% or critical vulnerabilities are found), the pipeline fails automatically, preventing deployment."

---

# 28. What is DAST?

**Interview Answer**

"DAST (Dynamic Application Security Testing) scans a running application to identify security vulnerabilities from an attacker's perspective.

Unlike SAST, DAST does not analyze source code. Instead, it performs runtime testing.

It is typically executed after deployment to a test environment."

---

# 29. How do you perform DAST testing?

**Interview Answer**

"In our pipeline:

1. Deploy the application to a staging environment.
2. Trigger OWASP ZAP scan.
3. Scan application endpoints.
4. Generate a security report.
5. Fail the pipeline if High or Critical vulnerabilities are detected.
6. Developers fix the issues before Production deployment."

---

# 30. Which OWASP Top 10 vulnerabilities does your DAST tool detect?

**Interview Answer**

"OWASP ZAP helps detect vulnerabilities such as:

- SQL Injection
- Cross-Site Scripting (XSS)
- Security Misconfiguration
- Broken Authentication
- Sensitive Data Exposure
- Insecure HTTP Headers
- Cross-Site Request Forgery (CSRF)
- Directory Traversal
- Open Redirects
- Server Information Disclosure"

---

# 31. If code coverage is below 80%, how will you fail the pipeline?

**Interview Answer**

"I configure the SonarQube Quality Gate to require a minimum of 80% code coverage.

The Azure DevOps pipeline waits for the Quality Gate result.

If the Quality Gate status is Failed, the pipeline exits with a non-zero status, preventing further stages from executing."

---

# 32. How do you enforce Quality Gates in Azure DevOps?

**Interview Answer**

"I integrate SonarQube with Azure DevOps.

The pipeline includes:

- Prepare Analysis
- Run Analysis
- Publish Quality Gate Result

The deployment stage depends on a successful Quality Gate.

Any Critical Vulnerability, Blocker Issue, or failed Quality Gate stops the deployment automatically."

---

# 33. Which agent do you use—Microsoft Hosted Agent or Self Hosted Agent?

**Interview Answer**

"I use both depending on the workload.

Microsoft Hosted Agents are used for:
- Standard application builds
- Unit testing
- Docker image builds
- Public Azure deployments

Self Hosted Agents are used when:
- Access to Private Endpoints is required
- Internal resources must be accessed
- Custom software is needed
- Large builds require higher performance"

---

# 34. Why are you using Microsoft Hosted Agent?

**Interview Answer**

"I use Microsoft Hosted Agents because:

- No infrastructure management
- Always up to date
- Faster provisioning
- Easy scalability
- Lower maintenance
- Microsoft-managed security

For public workloads, they reduce operational overhead significantly."

---

# 35. Why Ubuntu agent instead of Windows agent?

**Interview Answer**

"I prefer Ubuntu agents because:

- Faster startup time
- Better Docker support
- Native Terraform support
- Better Bash scripting
- Lower resource usage
- Faster pipeline execution
- Most cloud-native tools are Linux-first

I use Windows agents only when building Windows-specific applications, such as .NET Framework applications requiring MSBuild."

---

# 36. Your Azure Key Vault is behind a Private Endpoint. How will a Microsoft Hosted Agent access it?

**Interview Answer**

"It cannot access the Private Endpoint directly because Microsoft Hosted Agents run on Microsoft's public network.

To access a Private Endpoint, I deploy a Self Hosted Agent inside the same VNet or a peered VNet with proper DNS resolution."

---

# 37. Will a Microsoft Hosted Agent be able to access a Private Endpoint directly?

**Interview Answer**

"No.

Microsoft Hosted Agents do not have network connectivity to private Azure VNets.

Private Endpoints expose private IP addresses only, so a Hosted Agent cannot reach them without additional networking solutions."

---

# 38. How do you securely communicate from Microsoft Hosted Agent to private Azure resources?

**Interview Answer**

"There are several approaches:

- Preferred: Use a Self Hosted Agent inside the VNet.
- Use Azure DevOps Managed DevOps Pools (if supported for the scenario).
- Use a VPN or ExpressRoute to connect build infrastructure.
- Expose only required APIs through Application Gateway with WAF if appropriate.

For highly secure production environments, I always recommend a Self Hosted Agent."

---

# 39. How did you implement this architecture in your project?

**Interview Answer**

"In my current project:

- Azure Key Vault is secured using a Private Endpoint.
- DNS resolution is handled through Private DNS Zones.
- Self Hosted Azure DevOps Agents run on dedicated VMs within the Hub VNet.
- Pipelines authenticate using Managed Identity.
- Secrets are retrieved directly from Key Vault over the private network.
- No public network access is enabled for Key Vault.

This design meets enterprise security and compliance requirements."

---

# 40. Rate yourself on Terraform.

**Interview Answer**

"I would rate myself **9/10** in Terraform.

I have extensive experience with:

- Infrastructure as Code
- AzureRM Provider
- AzAPI Provider
- Remote State Management
- State Locking
- Modules
- Workspaces
- Variables and Outputs
- Terraform Import
- CI/CD Integration
- Drift Detection
- Provider Upgrades
- Multi-Environment Deployments
- Enterprise Module Design

I have designed reusable Terraform modules for networking, AKS, Key Vault, App Service, Storage Accounts, Azure Firewall, and complete Azure Landing Zones, and I have integrated Terraform deployments into Azure DevOps pipelines with approval gates and remote state stored in Azure Storage."

# Senior Azure DevOps Engineer Interview Guide (10+ Years Experience)

> **Part 3 (Q41–Q54)**

---

# 41. What is the difference between the Terraform block (`required_providers`) and the Provider block?

**Interview Answer**

The **Terraform block** defines the Terraform version and the providers required for the configuration. It tells Terraform **which provider plugin** and **which version** to download.

The **Provider block** configures how Terraform connects to the cloud provider by specifying authentication and subscription details.

Example:

```hcl
terraform {
  required_version = ">=1.6"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>4.0"
    }
  }
}

provider "azurerm" {
  features {}
  subscription_id = var.subscription_id
}
```

**Key Difference**

- `terraform {}` → Downloads provider plugins.
- `provider {}` → Authenticates and connects to Azure.

---

# 42. What is the difference between AzureRM Provider and AzAPI Provider?

**Interview Answer**

The **AzureRM Provider** is the primary provider for Azure resources and supports most GA (General Availability) services.

The **AzAPI Provider** allows Terraform to deploy Azure resources that are **not yet supported** by AzureRM, including Preview features and the latest API versions.

| AzureRM | AzAPI |
|----------|--------|
| Stable Resources | Preview Resources |
| Typed Resources | Generic ARM Resources |
| Easier Syntax | ARM JSON-based |
| Recommended for Production | Used when AzureRM lacks support |

In production, I always prefer **AzureRM**. I use **AzAPI** only when AzureRM does not yet support a required resource.

---

# 43. When should you use the AzAPI Provider?

**Interview Answer**

I use AzAPI when:

- AzureRM doesn't support a newly released Azure service.
- A Preview feature is required.
- A specific API version is needed.
- Microsoft releases a new resource before AzureRM adds support.

Once AzureRM supports the resource, I migrate from AzAPI to AzureRM for better maintainability.

---

# 44. Suppose a resource was created manually in Azure. How will you manage it using Terraform?

**Interview Answer**

I would **import** the resource into Terraform instead of recreating it.

Steps:

1. Write the Terraform resource block.
2. Run `terraform import`.
3. Run `terraform plan`.
4. Compare configuration with Azure.
5. Update Terraform code until the plan shows **No Changes**.
6. Commit the code to Git.

This approach avoids downtime and brings the resource under Infrastructure as Code management.

---

# 45. Explain the Terraform Import process.

**Interview Answer**

Terraform Import allows existing Azure resources to be managed without recreating them.

Example:

```bash
terraform import azurerm_storage_account.sa \
/subscriptions/xxx/resourceGroups/rg-demo/providers/Microsoft.Storage/storageAccounts/tfstate001
```

After import:

- Execute `terraform plan`.
- Terraform identifies missing configuration.
- Update the `.tf` file until no drift exists.
- Commit the configuration.

Import only updates the **state file**. It **does not generate Terraform code**, so the configuration must be written manually.

---

# 46. Which files are required while importing existing resources into Terraform?

**Interview Answer**

The minimum required files are:

- `main.tf`
- `provider.tf`
- `terraform.tf`
- `variables.tf` (if variables are used)
- `terraform.tfvars`
- Backend configuration (if remote state is used)

After importing, I run:

```bash
terraform plan
```

until the output shows:

```text
No changes.
Infrastructure is up-to-date.
```

---

# 47. Have you worked with Terraform Modules?

**Interview Answer**

Yes.

In enterprise environments, I always use reusable modules instead of writing resources directly.

Typical modules include:

- Resource Group
- VNet
- NSG
- Route Table
- Storage Account
- Key Vault
- AKS
- App Service
- SQL Database
- Azure Firewall

Benefits:

- Code Reusability
- Standardization
- Easier Maintenance
- Version Control
- Reduced Duplication

---

# 48. How do you pass values from one module (e.g., VNet) to another module (e.g., VM)?

**Interview Answer**

Terraform modules communicate using **outputs** and **input variables**.

Example:

**VNet Module**

```hcl
output "subnet_id" {
  value = azurerm_subnet.app.id
}
```

**VM Module**

```hcl
module "vm" {
  source    = "./modules/vm"
  subnet_id = module.vnet.subnet_id
}
```

Terraform automatically builds the dependency graph, ensuring the VNet is created before the VM.

---

# 49. Explain the complete Azure Firewall routing design.

**Interview Answer**

In my projects, Azure Firewall is deployed in the **Hub VNet**.

The design includes:

- Azure Firewall in the Hub.
- Application workloads in Spoke VNets.
- UDR associated with every workload subnet.
- Default route (`0.0.0.0/0`) points to the Azure Firewall private IP.
- Firewall policies define:
  - Network Rules
  - Application Rules
  - NAT Rules
- Firewall logs are sent to Log Analytics.

Traffic flow:

Application → UDR → Azure Firewall → Internet

Inbound traffic:

Internet → Application Gateway/WAF → Application

This ensures centralized inspection and logging of all outbound traffic.

---

# 50. Design a scalable Azure network architecture for thousands of VNets.

**Interview Answer**

For enterprise-scale environments, I recommend **Azure Virtual WAN** instead of a traditional Hub-and-Spoke architecture.

Architecture:

- Management Groups
- Multiple Subscriptions
- Azure Virtual WAN
- Regional Virtual Hubs
- Azure Firewall Manager
- ExpressRoute
- VPN Gateway
- Branch Connectivity
- Private DNS
- Private Endpoints

Benefits:

- Supports thousands of VNets.
- Simplified routing.
- Centralized security.
- Multi-region connectivity.
- Lower operational complexity.

This is Microsoft's recommended approach for large enterprises.

---

# 51. Explain the complete CI/CD flow from code commit to deployment.

**Interview Answer**

Our production workflow is:

1. Developer commits code to Azure Repos.
2. Pull Request triggers validation.
3. Code Review and Approval.
4. Build application.
5. Execute Unit Tests.
6. Run SonarQube (SAST).
7. Execute Dependency Scan.
8. Build Docker image.
9. Push image to Azure Container Registry.
10. Run Terraform Init.
11. Terraform Validate.
12. Terraform Plan.
13. Manual approval for Production.
14. Terraform Apply.
15. Deploy application using Helm to AKS.
16. Execute Smoke Tests.
17. Verify Health Checks.
18. Notify stakeholders.
19. Monitor using Azure Monitor and Application Insights.

---

# 52. Design a secure pipeline that accesses Azure Key Vault over a Private Endpoint.

**Interview Answer**

My production design includes:

- Azure DevOps
- Self Hosted Agent inside the Hub VNet
- Managed Identity authentication
- Private DNS Zone
- Azure Key Vault with Public Access Disabled
- Private Endpoint
- RBAC-based authorization
- Secrets retrieved at runtime

Why Self Hosted Agent?

Microsoft Hosted Agents cannot access private IP addresses exposed by Private Endpoints.

This architecture ensures secrets never traverse the public internet.

---

# 53. How would you migrate manually created Azure resources into Terraform?

**Interview Answer**

My migration process is:

1. Inventory existing Azure resources.
2. Write Terraform configuration.
3. Configure the remote backend.
4. Import each resource using `terraform import`.
5. Execute `terraform plan`.
6. Update Terraform code until no changes are detected.
7. Store the state in Azure Storage with state locking.
8. Commit the Terraform code to Git.
9. Integrate Terraform into the Azure DevOps pipeline.
10. Restrict manual changes using RBAC and Azure Policy.

This approach avoids downtime and ensures future changes are fully automated.

---

# 54. How do multiple Terraform modules communicate with each other using outputs, variables, and dependencies?

**Interview Answer**

Terraform modules communicate through **outputs** and **input variables**.

Example:

**Network Module**

```hcl
output "subnet_id" {
  value = azurerm_subnet.app.id
}
```

**Virtual Machine Module**

```hcl
variable "subnet_id" {}

resource "azurerm_linux_virtual_machine" "vm" {
  network_interface_ids = [
    azurerm_network_interface.nic.id
  ]
}
```

**Root Module**

```hcl
module "network" {
  source = "./modules/network"
}

module "vm" {
  source    = "./modules/vm"
  subnet_id = module.network.subnet_id
}
```

Terraform automatically determines dependencies based on these references. If an explicit dependency is required (for example, when no direct reference exists), I use:

```hcl
depends_on = [
  module.network
]
```

This ensures resources are provisioned in the correct order and keeps modules loosely coupled, reusable, and maintainable.

---

# Final Interview Tips

- Emphasize **Infrastructure as Code (Terraform)** over manual changes.
- Prefer **Managed Identity** over Service Principals whenever possible.
- Use **Azure Virtual WAN** for large-scale networking.
- Implement **Hub-and-Spoke** for medium-sized environments.
- Store Terraform state remotely with **Azure Storage**, **Blob Versioning**, and **State Locking**.
- Enforce **Least Privilege RBAC** and **Azure Policy**.
- Use **Self Hosted Agents** for accessing private Azure resources.
- Integrate **SAST (SonarQube)** and **DAST (OWASP ZAP)** into CI/CD.
- Deploy applications using **Blue-Green** or **Canary** strategies with rollback plans.
- Always enable monitoring, logging, alerting, and post-deployment health checks in production.
