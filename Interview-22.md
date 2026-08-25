# Coforge — Senior Engineer Interview Answers

## 1. Where are you located (Greater Noida or elsewhere)?

**Answer:**

I am currently based in **[YOUR LOCATION]**. I am comfortable working from Greater Noida and I am also flexible with the required hybrid or onsite model.

## 2. Can you tell us a brief about yourself, your technical journey, why you are changing roles, and your notice period/last working day?

**Answer:**

I have around 10 years of experience in DevOps and Cloud engineering, primarily working with Azure, Kubernetes/AKS, Terraform, Azure DevOps, Docker, CI/CD, Linux, networking, security, and monitoring.

In my recent role, I have been responsible for infrastructure automation using Terraform, CI/CD pipeline implementation, AKS deployments, Azure networking, monitoring, security, production troubleshooting, and operational support.

I started my journey with infrastructure and Linux administration and gradually moved into automation, cloud, containers, Kubernetes, Infrastructure as Code, and DevOps practices.

I am looking for a change because I want to take on a broader technical role where I can contribute more to cloud architecture, automation, Kubernetes, reliability, and platform engineering.

My current notice period is **[X DAYS]**, and my expected last working day is **[DATE]**.

## 3. Are you married or single? (Asked in the context of relocation flexibility)

**Answer:**

I prefer to keep personal details separate from professional discussions, but from a relocation perspective, I am flexible and can relocate to **[LOCATION]** if the role requires it.

## 4. What is your notice period, and what would be your joining date if selected?

**Answer:**

My current notice period is **[X DAYS]**. Based on that, my expected joining date would be **[DATE]**. If required, I can also discuss the possibility of an early release with my current organization.

## 5. Why did you choose to use YAML pipelines over the Classic version?

**Answer:**

I prefer YAML pipelines because they follow the Infrastructure-as-Code approach and keep the pipeline definition in source control.

The major advantages are version control, code review, reusability, auditability, branching, and consistency across environments.

With Classic pipelines, configuration is primarily maintained through the Azure DevOps UI, which can become difficult to manage at scale.

With YAML:

`Git → Pipeline YAML → Review → CI/CD`

So for enterprise environments, YAML gives better maintainability and traceability.

## 6. How does Terraform authenticate with Azure DevOps and Azure resources?

**Answer:**

Terraform itself needs authentication to Azure Resource Manager when provisioning Azure resources.

In Azure DevOps, I typically use a secure Service Connection based on a Service Principal or preferably Workload Identity/Federated Identity where supported.

The flow is:

`Azure DevOps Pipeline → Service Connection → Microsoft Entra ID → Azure Resource Manager → Azure Resources`

For Terraform state, I use a secure remote backend such as Azure Storage with appropriate authentication and RBAC.

I avoid storing credentials directly in Terraform code or pipeline YAML.

## 7. How do you create a service connection between Azure DevOps and the Azure cloud?

**Answer:**

In Azure DevOps, I go to:

`Project Settings → Service Connections → New Service Connection → Azure Resource Manager`

Then I select the appropriate authentication mechanism, preferably workload identity federation where available, select the Azure subscription and resource scope, provide the required permissions, and validate the connection.

From a security perspective, I follow least privilege and avoid giving the service connection unnecessary subscription-level permissions.

## 8. Have you created CI/CD pipelines from scratch, and for what kind of application deployments?

**Answer:**

Yes, I have created CI/CD pipelines from scratch for both application and infrastructure deployments.

I have worked with applications deployed to VMs as well as containerized applications deployed to AKS.

A typical application pipeline is:

`Checkout → Build → Unit Test → Code Scan → Security Scan → Docker Build → Image Scan → Push to ACR → Deploy to AKS → Smoke Test`

For infrastructure:

`Terraform Init → Validate → Plan → Approval → Apply`

## 9. What is the tech stack or language of the applications you have deployed?

**Answer:**

I have worked with multiple application stacks, primarily Java/Spring Boot, .NET, Python, Node.js, REST APIs, and microservices.

From the DevOps side, my responsibility is mainly around building, packaging, containerizing, configuring, deploying, monitoring, and troubleshooting these applications.

## 10. After deployment, how do you verify if an application is working correctly, and how do you monitor its health?

**Answer:**

After deployment, I perform multiple levels of validation.

First, I check Kubernetes resources:

```bash
kubectl get pods -n <namespace>
kubectl get svc -n <namespace>
kubectl get ingress -n <namespace>


Then I check application logs and health endpoints.

I also perform smoke tests against important APIs or URLs.

For monitoring, depending on the application, I use Azure Monitor, Application Insights, Log Analytics, Prometheus, and Grafana.

I monitor:

Availability
Response time
Error rate
CPU and memory
Pod restarts
HTTP status codes
Dependency failures
11. Do you write Terraform code from scratch or typically modify existing setups?

Answer:

I have done both.

I have created Terraform configurations and reusable modules from scratch as well as worked on existing enterprise Terraform codebases.

When creating from scratch, I focus on modularity, remote state, variables, outputs, naming standards, tagging, RBAC, security, and environment separation.

When working on existing code, I first understand the current architecture and state before making changes because modifying existing Terraform without understanding the state can cause unintended infrastructure changes.

12. If you have a VNet (/23 or /24) with VMs, SQL, Storage, and Key Vault, what is your strategy for designing this infrastructure with security and networking best practices?

Answer:

I would first design the address space and divide the VNet into dedicated subnets based on workload and security boundaries.

For example:

VNet
 |
 +-- Application Subnet
 |
 +-- VM Subnet
 |
 +-- Database Subnet
 |
 +-- Private Endpoint Subnet
 |
 +-- Azure Firewall Subnet


I would use NSGs at the subnet/NIC level, private endpoints for services such as SQL, Storage, and Key Vault where applicable, and private DNS zones for private connectivity.

I would avoid exposing databases and Key Vault directly to the internet.

The architecture would ideally be:

Users → Application Gateway/WAF → Application → Private Database/Storage/Key Vault

For outbound traffic, I would control it through Azure Firewall/NAT depending on the requirement.

I would also consider IP address planning, DNS, routing, RBAC, managed identities, logging, monitoring, and future growth before finalizing the CIDR design.

13. How do you manage resources that were manually created if you want to bring them under Terraform management?

Answer:

I use Terraform import.

First, I create the corresponding Terraform resource definition, then import the existing Azure resource into the Terraform state.

For example:

terraform import azurerm_resource_group.example /subscriptions/<id>/resourceGroups/<name>


After importing, I run:

terraform plan


Then I reconcile the Terraform configuration with the actual resource until the plan shows no unexpected changes.

The important point is that import only brings the resource into Terraform state; I still need to write the correct Terraform configuration.

14. If you have a hundred or more NSG rules to manage, how do you handle them efficiently in Terraform?

Answer:

I would not hardcode hundreds of individual NSG rules.

I would define the rules as structured variables or maps and use for_each to generate the rules dynamically.

For example:

variable "nsg_rules" {
  type = map(object({
    priority  = number
    direction = string
    protocol  = string
    source    = string
    destination_port = string
  }))
}


Then:

for_each = var.nsg_rules


This allows us to manage rules through data rather than continuously modifying Terraform resource blocks.

I would also use naming standards, validation, code review, and automated Terraform plan checks.

15. How do you identify and resolve cyclic dependencies in Terraform?

Answer:

Terraform builds a dependency graph. A circular dependency occurs when resources or modules depend on each other directly or indirectly.

For example:

Module A → Module B → Module A

Terraform cannot determine a valid creation order.

I identify it by reviewing the Terraform plan/error and tracing resource references and module outputs.

The solution is usually to redesign the dependency, extract the shared resource into a separate module, or pass only the required values.

For example:

Foundation Module → Module A

Foundation Module → Module B

This removes the cycle.

16. What is the difference between a NAT Gateway and an Internet Gateway, and why would you choose one over the other?

Answer:

The terminology differs between Azure and AWS.

In AWS, an Internet Gateway provides internet connectivity for resources in a VPC, while a NAT Gateway allows private subnet resources to initiate outbound internet connectivity without allowing unsolicited inbound internet connections.

In Azure, the closest concept to controlled outbound NAT is Azure NAT Gateway.

For a private workload that only needs outbound internet access, I would use NAT rather than exposing the workload directly to the internet.

The key difference is:

Internet Gateway → Internet connectivity

NAT Gateway → Controlled outbound connectivity for private resources

17. In a production environment where direct internet/NAT connectivity is avoided, how do you handle patching and updates for virtual machines?

Answer:

If direct internet access is not allowed, I would use controlled private connectivity and enterprise patching mechanisms.

Depending on the environment, this could involve Azure Update Manager, internal repositories, Azure services through Private Link, Azure Firewall-controlled egress, or an internal patch management server.

The architecture would be:

VM → Private/Controlled Network → Patch Repository/Service

I would ensure that only required outbound destinations are allowed and all activity is monitored.

18. Why do we need an Azure Firewall if we are already using NSGs?

Answer:

NSGs and Azure Firewall operate at different levels and solve different problems.

NSG is primarily used to control traffic at the subnet/NIC level using rules based on source, destination, port, and protocol.

Azure Firewall provides centralized network security and inspection capabilities, including centralized outbound filtering, application/network rules, logging, and integration with enterprise network architecture.

A common architecture is:

NSG → Local subnet-level control

Azure Firewall → Centralized network security and traffic inspection

So they complement each other rather than being direct replacements.

19. Have you implemented a Web Application Firewall (WAF)?

Answer:

Yes. I have worked with WAF using Azure Application Gateway and similar application delivery architectures.

WAF protects HTTP/HTTPS applications against common web attacks such as SQL injection and cross-site scripting.

A typical architecture is:

Internet → Front Door/Application Gateway + WAF → Ingress → AKS → Application

I also work with WAF logs and tune rules carefully because simply enabling WAF without monitoring false positives can impact legitimate application traffic.

20. Have you worked with scripting languages like Python, PowerShell, or Bash for automation?

Answer:

Yes. I regularly use Bash for Linux and DevOps automation and have used Python and PowerShell for automation tasks.

Typical use cases include Azure CLI automation, API calls, file processing, health checks, deployment automation, log processing, and repetitive operational tasks.

21. If you need to install a tool on five virtual machines simultaneously, how would you achieve that?

Answer:

I would avoid manually logging into each VM.

If Azure VMs are involved, I can use Azure Automation, Azure Update Manager, VM Run Command, Azure CLI, or another enterprise configuration management tool depending on the requirement.

If Ansible is available, I can use Ansible for centralized configuration management.

For five VMs, for example, I could execute a script remotely through Azure tooling and verify the installation afterward.

The important principle is:

Centralized automation → Parallel execution → Validation

22. How do you perform VM generalization and software loading?

Answer:

For Azure VMs, I first prepare the VM by installing and configuring the required software, removing machine-specific information, and ensuring the VM is in a reusable state.

For Windows, I typically use Sysprep for generalization.

For Linux, I prepare the image using the appropriate Azure-supported generalization process, such as deprovisioning machine-specific configuration where required.

Then I capture the generalized VM as an image and use it to create multiple consistent VMs.

The goal is to create a standardized golden image rather than manually configuring every VM.

23. If Ansible is not available, what other methods would you use to update or download items in a workflow?

Answer:

I would choose the method based on the requirement.

For Azure VMs, options include:

Azure CLI
Azure VM Run Command
Azure Automation
Custom Script Extension
PowerShell
Bash
Terraform provisioners as a last resort

I generally avoid using Terraform provisioners for ongoing configuration management because Terraform is primarily intended for infrastructure provisioning.

If I use a provisioner, it should be for a controlled bootstrap-type requirement rather than continuous software management.

24. Are you familiar with the Azure CLI?

Answer:

Yes, I use Azure CLI regularly for Azure administration and automation.

For example:

az login
az account set --subscription "<subscription-id>"
az group list
az vm list
az aks get-credentials --resource-group <rg> --name <aks>
az network vnet list


I also use Azure CLI inside CI/CD pipelines for deployment and operational automation.

25. What is the difference between COPY and ADD in a Dockerfile?

Answer:

Both can copy files into an image, but ADD has additional behavior such as handling local tar archives and supporting certain URL-related functionality.

I generally prefer COPY because it is simpler and more explicit.

For example:

COPY app.jar /app/app.jar


I use ADD only when I specifically need one of its additional capabilities.

26. If a pod is down, what step-by-step investigation and commands do you use?

Answer:

I first check the pod status:

kubectl get pods -n <namespace>


Then:

kubectl describe pod <pod-name> -n <namespace>


I check logs:

kubectl logs <pod-name> -n <namespace>


For a restarted container:

kubectl logs <pod-name> --previous -n <namespace>


Then I check events:

kubectl get events -n <namespace> --sort-by=.lastTimestamp


I investigate:

ImagePullBackOff
CrashLoopBackOff
OOMKilled
Failed probes
Configuration/secrets
Resource limits
Scheduling issues
Node problems
Network/dependency failures

My approach is:

Status → Describe → Logs → Events → Resources → Configuration → Node → Network/Dependencies

27. In a multi-namespace environment, how do you ensure one database/service does not communicate with another?

Answer:

I use Kubernetes NetworkPolicies to control pod-to-pod traffic.

For example, if namespace A should communicate only with its own database, I create an ingress policy on the database allowing traffic only from the required namespace/pods.

Conceptually:

Namespace A
   |
   +--> Application A
   |
   +--> Database A

Namespace B
   |
   +--> Application B
   |
   +--> Database B


NetworkPolicy can restrict traffic based on namespace selectors, pod selectors, IP blocks, and ports.

I also ensure the underlying AKS networking implementation supports the required NetworkPolicy behavior.

28. Can you explain the request flow from a user hitting a web URL down to the pod level?

Answer:

A typical flow is:

User → DNS → Front Door/Load Balancer → Application Gateway/Ingress → Kubernetes Service → Pod

For example:

User
 |
 v
DNS
 |
 v
Azure Front Door / Application Gateway
 |
 v
Ingress Controller
 |
 v
Kubernetes Service
 |
 v
Pod
 |
 v
Application


The Ingress performs Layer-7 routing based on host/path rules.

The Kubernetes Service provides a stable endpoint and forwards the request to matching pods.

29. How does a Kubernetes Service identify and route traffic to the correct pods given that pod IPs are dynamic?

Answer:

The Service uses label selectors.

For example, the pods may have:

labels:
  app: payment


The Service can have:

selector:
  app: payment


Kubernetes maintains the corresponding EndpointSlices for matching pods.

When pods are recreated and their IP addresses change, Kubernetes updates the EndpointSlices automatically.

So clients communicate with the stable Service rather than directly with dynamic pod IPs.

30. What is the difference between a Deployment and a ReplicaSet?

Answer:

A ReplicaSet ensures that a specified number of pod replicas are running.

A Deployment manages ReplicaSets and provides higher-level deployment capabilities such as rolling updates, rollbacks, and revision management.

The relationship is:

Deployment → ReplicaSet → Pods

In normal application deployments, I generally create a Deployment rather than managing ReplicaSets directly.

31. Do you have any experience with AWS?

Answer:

My strongest production experience is Azure, but I have worked with AWS concepts and understand the major services and architecture.

For example:

Azure VNet ≈ AWS VPC

Azure VM ≈ EC2

Azure Load Balancer ≈ AWS Load Balancer

Azure AKS ≈ Amazon EKS

Azure Blob Storage ≈ S3

I am comfortable learning and working across cloud platforms, but I would clearly position Azure as my primary hands-on cloud experience.

32. Have you worked with internal or application load balancers?

Answer:

Yes.

I have worked with internal and application-level load balancing concepts.

For application-level traffic, I have worked with Azure Application Gateway, including Layer-7 routing, host/path-based routing, TLS termination, health probes, and WAF.

For internal traffic, I have worked with private/internal load balancing where applications communicate within the virtual network without exposing services publicly.

The choice depends on whether we need Layer-4 or Layer-7 functionality and whether the service needs to be internal or internet-facing.

33. Do you have any questions for the interviewers?

Answer:

Yes, I would like to understand:

What are the main technical challenges the team is currently solving?
What is the current Azure/Kubernetes architecture?
How mature is the current CI/CD and Infrastructure-as-Code setup?
What is the production support and on-call model?
What would you expect from this role during the first six months?
