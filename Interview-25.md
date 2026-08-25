# Optum — Senior DevOps Engineer Interview Answers

**1. Can you give us a high-level overview of your experience, keeping it cloud-centric?**

I have around 10 years of experience in DevOps and Cloud engineering, with Azure as my primary cloud platform. My core areas are Azure, AKS, Terraform, Azure DevOps, Kubernetes, Docker, CI/CD, networking, security, monitoring, and production support. In my current role, I work on infrastructure automation, AKS platform management, CI/CD, Azure networking, security, observability, troubleshooting, and cost optimization.

**2. What project are you currently working on?**

Currently, I am working on a cloud modernization project where applications are hosted on Azure and containerized workloads are running on AKS. My responsibilities include Terraform-based infrastructure, Azure DevOps CI/CD, AKS deployments, Azure networking, Key Vault, monitoring, security, production support, and automation. I also work closely with application teams to improve deployment reliability and reduce operational issues.

**3. How do you prevent the accidental destruction of resources?**

I use multiple layers of protection. At the Terraform level, for critical resources I use:

```hcl
lifecycle {
  prevent_destroy = true
}


I also use Azure resource locks where appropriate, RBAC with least privilege, separate production environments, pull-request approvals, Terraform plan reviews, and controlled CI/CD pipelines.

For production, I never allow a developer or pipeline to directly execute destructive changes without proper authorization.

4. Suppose you have an Azure storage account containing your Terraform state file, and it is accidentally deleted while production resources are still running. If there is no version control enabled, how will you recover the state file?

If the storage account and state file are completely deleted and there is no blob versioning, soft delete, backup, or another copy, Terraform cannot reconstruct the exact previous state automatically.

I would first check whether the storage account/blob has any recovery mechanism enabled, such as soft delete, backups, replication, or organizational backup.

If no recovery mechanism exists, I would rebuild the state by importing the existing Azure resources into a new Terraform state.

For example:

terraform init
terraform import azurerm_resource_group.example <resource-id>
terraform plan


I would then progressively import all managed resources and reconcile the Terraform configuration with the actual infrastructure.

This is why I consider Terraform state a critical production asset and always use a secured remote backend with state protection, versioning/recovery capabilities, restricted RBAC, and backup strategy.

5. What are Terraform null resources?

null_resource is a Terraform resource that doesn't represent an actual infrastructure object. It is traditionally used to execute provisioners or trigger actions based on changes.

For example:

resource "null_resource" "example" {
  triggers = {
    version = var.version
  }

  provisioner "local-exec" {
    command = "echo Deployment triggered"
  }
}


I use it cautiously because Terraform should primarily manage infrastructure declaratively. For modern Terraform, I would also consider alternatives such as terraform_data where appropriate.

6. If an engineer modifies a resource manually using the Azure console and Terraform now shows unexpected changes, how do you reduce or prevent these manual changes in production?

First, I would identify whether Terraform or the manual configuration should be the source of truth.

For production, Terraform should normally be the source of truth. I would prevent manual changes through least-privilege RBAC, removing unnecessary write permissions, using Azure Policy, requiring changes through pull requests, and auditing Azure Activity Logs.

For highly critical environments, I would also use management controls and CI/CD policies to ensure infrastructure changes go through the approved Terraform workflow.

7. How do you resolve state drift after manual console modifications are made?

First I run:

terraform plan


and identify exactly what has changed.

Then I compare the actual Azure configuration with the Terraform configuration.

If the manual change was unauthorized, I let Terraform revert it:

terraform apply


If the manual change was intentional and should become the new desired state, I update the Terraform code to represent that configuration and then run:

terraform plan


until the plan shows the expected result.

The important principle is: decide which configuration is the source of truth before resolving drift.

8. How do you figure out and compare the actual against the desired configuration to fix a drift?

I use Terraform as the desired-state definition and Azure as the actual state.

I start with:

terraform plan


Then I inspect the resource in Azure using Azure Portal, Azure CLI, or resource-specific commands.

I compare:

Terraform Configuration
        ↓
Desired State

Azure Resource
        ↓
Actual State


If the desired configuration needs to change, I update the Terraform resource/module/variable accordingly. I don't simply modify state to hide the drift.

9. When you run a plan after updating a resource block to match a drift, will it show additional resources getting created, or that changes are getting imported?

It depends on what happened.

If the resource already exists in Terraform state and I update the configuration to match the actual Azure configuration, ideally terraform plan will show no changes.

If the resource exists in Azure but is not present in Terraform state, Terraform doesn't automatically import it. I need to explicitly import it:

terraform import <resource-address> <azure-resource-id>


After import and configuration reconciliation, terraform plan should show no unexpected changes.

Terraform does not automatically interpret an existing resource as an imported resource simply because it exists in Azure.

10. How do you deploy to different environments like Dev, QA, and Production? Do you use a single Terraform configuration?

I prefer reusable Terraform modules with separate environment configurations or environment-specific variable/state management.

For example:

terraform/
├── modules/
│   ├── network/
│   ├── aks/
│   └── storage/
│
├── dev/
├── qa/
└── prod/


The modules are reusable, while each environment has its own variables, state, sizing, and configuration.

I also keep separate Terraform state for each environment to prevent accidental cross-environment changes.

11. In Azure, how are the management groups, subscriptions, policies, and RBAC roles organized?

A typical enterprise hierarchy is:

Tenant Root
   |
   +-- Platform Management Group
   |      |
   |      +-- Connectivity Subscription
   |      +-- Management Subscription
   |
   +-- Landing Zones
          |
          +-- Production Subscription
          +-- Non-Production Subscription


Azure Policies and initiatives can be assigned at management-group scope and inherited by subscriptions.

RBAC can be assigned at management group, subscription, resource group, or resource scope depending on the requirement.

I follow least privilege and avoid broad Owner permissions wherever possible.

12. What happens automatically when a new subscription is onboarded in the context of an Azure Landing Zone?

In a properly automated Landing Zone, onboarding a subscription can automatically apply the enterprise baseline.

This may include:

Management group placement
Azure Policy assignments
RBAC
Logging/monitoring
Security controls
Networking configuration
Defender/security configuration
Diagnostic settings
Naming/tagging standards
Budget and cost controls

The objective is that a new subscription becomes compliant with the organization's baseline without manually configuring everything.

13. What is the difference between a Git merge and a Git rebase?

merge combines two branches and normally creates a merge commit.

git checkout feature
git merge main


rebase moves my feature commits on top of the latest main branch and creates a cleaner linear history.

git checkout feature
git rebase main


I prefer rebase for keeping my local feature branch current, but I avoid rebasing shared/public branches because it rewrites commit history.

14. How would you revert a bad production deployment if someone made a commit to the production repository that caused an issue?

First, I would assess the impact and use the fastest safe rollback mechanism.

For Git, I would use:

git revert <commit>


This creates a new commit that reverses the bad change without rewriting shared history.

Then the CI/CD pipeline would deploy the reverted version.

If the issue is at the Kubernetes deployment level, I can also perform an application rollback:

kubectl rollout undo deployment/<deployment> -n <namespace>


or with Helm:

helm rollback <release> <revision> -n <namespace>


After rollback, I validate application health and then perform root-cause analysis.

15. How do you manage and secure credentials/secrets for authenticating pipelines?

I don't store credentials directly in source code or YAML.

I prefer managed identity/workload identity or federated authentication wherever possible.

For secrets that are actually required, I use Azure Key Vault or the CI/CD platform's secure secret store.

The principles are:

Least privilege
Secret rotation
No hardcoding
Masked pipeline variables
Short-lived credentials where possible
RBAC
Audit logging

16. Where do you actually store those credentials, and do you store them using Terraform or manually using the Azure Console?

For Azure authentication, I prefer workload identity/federated credentials or managed identities rather than long-lived client secrets.

Application secrets are stored in Azure Key Vault.

Terraform can create the Key Vault, RBAC assignments, private endpoint, and configuration. I generally avoid putting actual production secret values directly into Terraform source code.

The secret value can be populated through a secure secret-management process.

17. Are you aware of other vault options or GitHub features for storing secret variables?

Yes.

On Azure, I use Azure Key Vault.

For CI/CD, Azure DevOps provides secret pipeline variables and variable groups, while GitHub provides GitHub Actions Secrets and environments with protected deployment controls.

Other enterprise options include HashiCorp Vault and cloud-native secret managers.

My preference is to keep secrets in a dedicated secret-management system and allow pipelines to retrieve them securely when needed.

18. Are you more familiar with GitHub Actions or Azure DevOps?

My stronger hands-on production experience is with Azure DevOps, particularly Azure Repos, YAML pipelines, environments, approvals, service connections, artifacts, and release automation.

I also have knowledge and exposure to GitHub Actions, and the underlying CI/CD concepts are very similar.

19. If you want to add manual approvals between your Terraform plan and Terraform apply stages, how do you configure that?

I separate the pipeline into Plan and Apply stages.

For example:

Terraform Plan
      |
      v
Production Environment
      |
   Approval
      |
      v
Terraform Apply


The production environment is configured with approval/check requirements.

The pipeline generates the Terraform plan as an artifact, and after approval the apply stage consumes the approved plan.

This is safer than running a fresh uncontrolled plan after approval.

20. What are the specific steps to add that? Are you going to add an environment in the pipeline?

Yes.

I would:

Create a Production environment in Azure DevOps.
Configure the environment's approval/checks.
Reference that environment from the Apply stage.
Keep Plan and Apply as separate stages.
Publish the Terraform plan as an artifact.
Require approval before the Apply stage executes.
Execute Terraform Apply using the approved plan.

Conceptually:

stages:
- stage: Plan
  jobs:
  - job: TerraformPlan
    ...

- stage: Apply
  dependsOn: Plan
  jobs:
  - deployment: TerraformApply
    environment: Production
    ...


21. How do you enable and set up the approval settings after adding the environment section in your pipeline YAML?

In Azure DevOps, I go to:

Pipelines
   → Environments
      → Production
         → Approvals and checks


Then I configure the required approval/check mechanism and specify the appropriate users or groups.

The YAML references the environment:

environment: Production


When the pipeline reaches that deployment job, Azure DevOps evaluates the configured checks and requires approval before proceeding.

I prefer groups rather than individual users where possible because it is easier to manage operationally.

22. Can you explain the request flow from when a user hits a web URL down to the backend pod level in AKS?

A typical flow is:

User
 |
 v
DNS
 |
 v
Azure Front Door / Application Gateway
 |
 v
AKS Ingress Controller
 |
 v
Kubernetes Service
 |
 v
EndpointSlice
 |
 v
Backend Pod
 |
 v
Application
 |
 v
Database / External Services


DNS resolves the application hostname to the public entry point.

The load balancer/WAF receives the request and forwards it to the Ingress.

Ingress performs host/path-based routing to the appropriate Kubernetes Service.

The Service uses selectors and EndpointSlices to route traffic to healthy pods.

23. What does this flow look like if a user is trying to access the application from the outside internet?

For an internet-facing application:

Internet User
      |
      v
Public DNS
      |
      v
Azure Front Door + WAF
      |
      v
Application Gateway / Public Load Balancer
      |
      v
AKS Ingress
      |
      v
Kubernetes Service
      |
      v
Application Pod


I prefer keeping the backend services private and exposing only the required entry point.

WAF provides protection at the HTTP layer, while network controls such as NSGs, private endpoints, and network policies provide additional security.

24. Do you know or have experience with programming languages like Python?

Yes. I have working knowledge of Python and use it mainly for DevOps automation rather than full-time application development.

I use Python for API automation, Azure automation, data processing, health checks, deployment utilities, and operational scripts.

I am also comfortable with Bash, PowerShell, YAML, and basic application troubleshooting.

25. If you have database instances on-premises and need to move the data to a cloud environment, what are your connectivity options to bridge both isolated environments?

The main connectivity options are:

Site-to-Site VPN
ExpressRoute
AWS Direct Connect
Azure ExpressRoute
VPN over the internet
Dedicated private connectivity
Data transfer services depending on the migration scenario

For Azure, a common architecture is:

On-Premises
     |
     v
VPN / ExpressRoute
     |
     v
Azure VNet
     |
     v
Azure Storage / Database


For large-scale enterprise production workloads, I generally prefer private dedicated connectivity such as ExpressRoute when the requirements justify it.

26. What is the main limitation or deal breaker for a Site-to-Site VPN compared to an ExpressRoute?

The biggest difference is that Site-to-Site VPN uses an encrypted tunnel over the public internet, so latency, bandwidth, and availability can be affected by internet conditions.

ExpressRoute provides private connectivity between the on-premises environment and Azure through a connectivity provider.

ExpressRoute is generally preferred for predictable performance, higher bandwidth requirements, enterprise connectivity, and workloads where internet-based VPN is not sufficient.

27. Suppose you have an Azure VM and an AWS EC2 instance with network connectivity. You can reach both sides using IP addresses, but accessing the EC2 instance using its URL does not work. What is the main problem?

If IP connectivity works but hostname/URL access does not, I would first suspect DNS resolution.

The network path is working, but the hostname is probably not resolving to the correct private IP.

I would test:

nslookup <hostname>
dig <hostname>
curl -v http://<hostname>


Then I would check DNS zones, DNS forwarding, private hosted zones, Azure VNet DNS settings, and whether the Azure environment knows how to resolve the AWS private hostname.

If DNS resolves correctly, then I would investigate application-layer issues such as HTTP/HTTPS, certificates, routing, and host-header configuration.

28. How is the DNS managed within an Azure VNet?

An Azure VNet can use:

Azure-provided DNS
Custom DNS servers
Azure Private DNS Zones
Azure DNS Private Resolver

For private Azure services, Private DNS Zones are commonly used.

For example:

VNet
 |
 +-- Private DNS Zone
       |
       +-- SQL Private Endpoint
       +-- Key Vault Private Endpoint
       +-- Storage Private Endpoint


The VNet's DNS configuration determines which DNS servers clients use.

29. What do we need to introduce or configure to ensure DNS requests are reliably resolved and forwarded to the other cloud provider?

For hybrid or multi-cloud DNS, I would introduce a controlled DNS forwarding architecture.

In Azure, Azure DNS Private Resolver is a strong option.

A typical design is:

Azure VM
   |
   v
Azure DNS Private Resolver
   |
   | DNS Forwarding Ruleset
   |
   v
VPN / ExpressRoute
   |
   v
AWS DNS Resolver
   |
   v
AWS Private Hosted Zone


I would configure DNS forwarding rules for the AWS private domains and make sure the network path between the DNS resolvers is available.

I would also verify:

DNS ports 53 UDP/TCP
Route tables
NSGs/firewall rules
DNS forwarding rules
Private hosted zones
Split-horizon DNS where required
High availability of DNS resolvers

The key point is that IP connectivity alone is not enough; DNS resolution must also work across the hybrid/multi-cloud boundary.
