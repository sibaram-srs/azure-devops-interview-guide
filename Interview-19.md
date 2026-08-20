Bounteous x Accolite — Senior DevOps Engineer Interview Preparation

Target profile: Senior DevOps Engineer / 10+ years experience
Focus: Azure, Terraform, Kubernetes, CI/CD, GitOps, Linux, networking, observability and cost optimization

Interview strategy: At 10+ years, don't answer only with definitions. Explain how you implemented it, why you designed it that way, what trade-offs you considered, and how you troubleshoot failures.

1. Introduce yourself.
Interview Answer

I have over 10 years of experience in DevOps, cloud infrastructure, automation, CI/CD and container orchestration, with strong hands-on experience primarily around Azure, Terraform, Kubernetes, Git and CI/CD platforms.

My core expertise is in building and managing scalable, secure and highly available cloud infrastructure using Infrastructure as Code, particularly Terraform. I have worked on Azure networking, compute, identity, storage, monitoring, Azure Policy and enterprise landing-zone concepts.

On the DevOps side, I have designed CI/CD pipelines covering the complete lifecycle — source-code commit, build, unit testing, security scanning, artifact creation, infrastructure provisioning and application deployment.

I have also worked extensively with Kubernetes, including Services, Ingress, ConfigMaps, Secrets, probes, resource management and troubleshooting pod and deployment issues. For GitOps, I have experience with Argo CD and Helm.

From an engineering perspective, I focus not only on automation but also on security, reliability, observability, scalability and cost optimization. At a senior level, I also spend significant time on architecture decisions, reusable automation, mentoring engineers and establishing DevOps standards across teams.

Strong closing line

My approach is to treat DevOps as an engineering discipline rather than simply writing pipelines — the objective is repeatable, secure, observable and reliable delivery.

2. Do you have experience with multi-cloud environments?
Interview Answer

Yes. I have worked with cloud environments where the architecture and DevOps practices need to support multiple cloud platforms.

My approach is to separate cloud-agnostic DevOps practices from cloud-specific implementations.

For example:

Git is used as the source of truth.
Terraform is used for Infrastructure as Code.
Kubernetes provides a consistent application platform.
Helm is used for application packaging.
Argo CD can provide GitOps-based deployment.
CI pipelines handle validation, testing and artifact creation.
Cloud-specific modules handle networking, IAM, compute and managed services.

I avoid trying to make everything 100% identical across clouds because each cloud has different networking, IAM and managed-service capabilities.

Instead, I standardize the engineering process and interfaces, while keeping cloud-specific implementation inside reusable modules.

Senior-level point

For multi-cloud, the biggest challenge isn't provisioning resources. It's maintaining consistent security, identity, networking, observability, governance and operational processes across providers.

3. Explain the end-to-end CI/CD flow.
Interview Answer

I would explain it as two major pipelines:

Developer
   |
   v
Git Repository
   |
   v
Pull Request
   |
   +--> Code Review
   |
   +--> Static Analysis
   |
   +--> Unit Tests
   |
   +--> SAST / Dependency Scan
   |
   v
Build
   |
   v
Artifact / Container Image
   |
   +--> Image Scan
   |
   v
Artifact Registry
   |
   v
Infrastructure Validation
   |
   v
Terraform Plan
   |
   v
Approval / Policy Gate
   |
   v
Terraform Apply
   |
   v
Deployment
   |
   v
Kubernetes / Azure
   |
   v
Smoke / Integration Tests
   |
   v
Monitoring / Observability

Target deployment platform

The target platform depends on the application architecture. For containerized applications, I would typically target AKS/Kubernetes. For serverless workloads, Azure Functions could be the target. For VM-based workloads, deployment could be to Azure VMs or VM Scale Sets.

Important senior-level distinction

I prefer to separate CI from CD.

CI should produce a versioned, immutable artifact. CD should promote that same artifact through environments rather than rebuilding it separately for each environment.

For Kubernetes:

Developer
   |
   v
Git
   |
   v
CI Pipeline
   |
   v
Docker Image
   |
   v
Container Registry
   |
   v
Helm / GitOps Repository
   |
   v
Argo CD
   |
   v
AKS

4. Azure Landing Zone and Azure Policies
Interview Answer

Yes, I understand and have worked with Azure Landing Zone concepts. A landing zone provides the foundational Azure environment in which application workloads can be deployed consistently and securely.

Typical components include:

Management Groups
       |
       +-- Subscriptions
       |
       +-- Identity / RBAC
       |
       +-- Networking
       |
       +-- Security
       |
       +-- Logging / Monitoring
       |
       +-- Azure Policy
       |
       +-- Governance

Azure Policy

Azure Policy is used for governance and compliance.

Examples:

Restrict resource locations.
Require mandatory tags.
Restrict public IP creation.
Enforce approved VM SKUs.
Require diagnostic settings.
Audit encryption.
Enforce security configurations.
Senior-level answer

I would not treat a Landing Zone as simply installing plugins or configuring tools. The important part is establishing governance boundaries, subscription structure, networking, identity, policy, security and operational standards before application teams start consuming the platform.

5. How do you provision Azure resources? Are you using Terraform?
Interview Answer

Yes, Terraform is my preferred approach for provisioning Azure infrastructure because it provides declarative Infrastructure as Code, version control, repeatability and a consistent deployment process.

Typical flow:

Terraform Code
      |
      v
terraform fmt
      |
      v
terraform validate
      |
      v
terraform plan
      |
      v
Security / Policy Checks
      |
      v
Approval
      |
      v
terraform apply
      |
      v
Azure Resources


I would typically use:

Terraform
AzureRM Provider
Remote State
Azure Storage Account
State Locking
CI/CD Pipeline


I also prefer reusable Terraform modules rather than putting all resources into a single large configuration.

6. Automation/scripting tools and Shell example
Interview Answer

I have used Bash/Shell scripting extensively, along with Python and automation tools such as Terraform, Ansible and CI/CD pipeline scripting.

A simple production-oriented Bash example:

#!/bin/bash

set -euo pipefail

APP_NAME="myapp"
NAMESPACE="production"

echo "Checking deployment status..."

if kubectl get deployment "$APP_NAME" -n "$NAMESPACE" >/dev/null 2>&1; then
    echo "Deployment exists"

    kubectl rollout status \
        deployment/"$APP_NAME" \
        -n "$NAMESPACE" \
        --timeout=120s
else
    echo "Deployment does not exist"
    exit 1
fi

echo "Deployment validation completed successfully."

Why set -euo pipefail?

-e stops execution when a command fails, -u catches undefined variables and pipefail ensures pipeline failures are not silently ignored.

That demonstrates production-quality scripting rather than just writing a basic loop.

7. How do you design modular Terraform?

A structure I commonly prefer:

terraform/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   │
│   ├── staging/
│   └── prod/
│
├── modules/
│   ├── network/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── README.md
│   │
│   ├── vm/
│   ├── aks/
│   └── storage/
│
└── README.md

Module design principle

A module should represent a logical capability.

For example:

network
  |
  +-- vnet
  +-- subnets
  +-- NSG
  +-- route tables


The environment layer composes modules:

module "network" {
  source = "../../modules/network"

  environment = "prod"
  location    = "eastus"
}

Senior-level point

I keep environment-specific values outside reusable modules and keep modules generic. I also avoid over-modularization because creating a module for every small resource can make the code harder to understand.

8. Management no longer wants Terraform to manage a resource

This is a very important Terraform question.

Interview Answer

First, I would clarify whether the resource should remain in Azure but simply be removed from Terraform management.

If yes, I would remove it from the Terraform state without destroying the real resource.

For modern Terraform:

terraform state rm <resource_address>


For example:

terraform state rm azurerm_storage_account.example


This tells Terraform:

"Stop tracking this object."

It does not delete the Azure resource.

Important distinction
terraform destroy
        |
        +--> Deletes infrastructure

terraform state rm
        |
        +--> Removes Terraform tracking
        +--> Infrastructure remains

Senior-level caveat

Before doing this, I would verify dependencies and ownership. If the resource is still referenced by other Terraform resources, I would also review those dependencies. I would document the ownership change so another engineer doesn't accidentally re-import or recreate it later.

9. How do you track manually created resources in Terraform?

Terraform cannot automatically manage a manually created resource just because it exists in Azure.

I would:

Step 1 — Define it in Terraform
resource "azurerm_resource_group" "example" {
  name     = "existing-rg"
  location = "East US"
}

Step 2 — Import the existing resource
terraform import azurerm_resource_group.example /subscriptions/<id>/resourceGroups/existing-rg

Step 3 — Run
terraform plan


Then reconcile the Terraform configuration with the actual resource.

Important concept
Existing Azure Resource
          |
          v
Terraform Import
          |
          v
Terraform State
          |
          v
Terraform Configuration


Import establishes Terraform's ownership/tracking relationship; it doesn't magically generate a perfect Terraform configuration for the resource.

10. Terraform code to create a VM + service account
Azure VM example
resource "azurerm_linux_virtual_machine" "example" {
  name                = "devops-vm"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  size                = "Standard_D2s_v5"

  admin_username = "azureuser"

  network_interface_ids = [
    azurerm_network_interface.example.id
  ]

  admin_ssh_key {
    username   = "azureuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Premium_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-jammy"
    sku       = "22_04-lts-gen2"
    version   = "latest"
  }

  identity {
    type = "SystemAssigned"
  }
}

Azure equivalent of a service account

In Azure, I would generally use Managed Identity rather than embedding credentials inside the VM.

For example:

identity {
  type = "SystemAssigned"
}


Then assign RBAC:

resource "azurerm_role_assignment" "vm_reader" {
  scope                = azurerm_resource_group.example.id
  role_definition_name = "Reader"
  principal_id         = azurerm_linux_virtual_machine.example.identity[0].principal_id
}

Strong interview statement

In Azure, I prefer Managed Identity and RBAC over storing service-account passwords or client secrets on VMs.

11. On-premises or Azure Cloud?
Interview Answer

I am comfortable working in both environments. However, for a modern DevOps platform I would prefer Azure where there is a business requirement for cloud adoption because it gives us managed services, elasticity, automation and better integration with cloud-native capabilities.

At the same time, I understand hybrid environments because many enterprises still have on-premises dependencies.

Typical hybrid architecture:

On-Prem
   |
   | ExpressRoute / VPN
   |
Azure VNet
   |
   +-- AKS
   +-- Azure Functions
   +-- Private Endpoints
   +-- Key Vault
   +-- Storage

12. Deployment completed but application alert received — troubleshoot

This is where a senior engineer should demonstrate structured troubleshooting.

My approach
Alert
 |
 v
Understand impact
 |
 v
Check recent deployment
 |
 v
Check application health
 |
 v
Check Kubernetes
 |
 v
Check infrastructure
 |
 v
Check dependencies
 |
 v
Check logs / metrics / traces
 |
 v
Identify root cause
 |
 v
Mitigate
 |
 v
Root Cause Analysis

Step 1 — Understand the alert

I check:

What alert fired?
When did it start?
Which service?
Which environment?
How many users are affected?
Is it availability, latency, errors or resource exhaustion?
Step 2 — Correlate with deployment
kubectl rollout history deployment/myapp -n production


If the issue started immediately after deployment, deployment correlation becomes a strong signal.

Step 3 — Check pods
kubectl get pods -n production
kubectl describe pod <pod> -n production

Step 4 — Logs
kubectl logs <pod> -n production


For previous crashed container:

kubectl logs <pod> -n production --previous

Step 5 — Check service
kubectl get svc -n production
kubectl get ingress -n production

Step 6 — Check dependencies

I would verify:

Database
Redis/cache
External APIs
DNS
Secrets
Certificates
Network connectivity
Step 7 — Mitigation

If the new release is confirmed as the cause:

kubectl rollout undo deployment/myapp -n production


Then perform RCA.

Senior-level answer

My priority during an incident is restore service first, investigate deeply second. After stabilization, I perform root-cause analysis and introduce preventive controls.

13. Difference between Ingress and Service
Service	Ingress
Provides stable network endpoint for Pods	Provides HTTP/HTTPS routing
Works mainly at L4	Generally operates at L7
Load-balances traffic across Pods	Routes traffic based on host/path
ClusterIP, NodePort, LoadBalancer	Requires an Ingress Controller
Example: api-service	Example: api.company.com/users

Example:

Internet
   |
   v
Ingress Controller
   |
   +---- /api ----> api-service ----> Pods
   |
   +---- /web ----> web-service ----> Pods

14. How do you configure Ingress and what is its purpose?

Example:

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  ingressClassName: nginx

  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: app-service
                port:
                  number: 80

Purpose

Ingress provides:

Host-based routing
Path-based routing
TLS termination
Centralized HTTP/HTTPS entry point
Routing to multiple Kubernetes Services
Important

An Ingress resource itself doesn't necessarily process traffic. An Ingress Controller such as NGINX or an Azure-supported ingress implementation actually implements the routing behavior.

15. Pods need internet access to download packages. What is required?

This depends on the AKS/network architecture.

For private workloads, I would check:

Pod
 |
 v
Node / Azure CNI networking
 |
 v
Subnet
 |
 v
Route / NAT
 |
 v
Internet


For controlled outbound access, I would commonly use Azure NAT Gateway.

Why NAT Gateway?

It provides predictable outbound connectivity and scalable SNAT behavior.

I would also verify:

Route tables
NSGs
Firewall
DNS
NAT configuration
Egress policies
Proxy requirements
Senior-level point

I would not simply allow unrestricted internet access. In an enterprise environment, outbound traffic should be controlled, monitored and preferably routed through approved egress infrastructure.

16. Pod keeps restarting — possible reasons?

First I check:

kubectl get pods
kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs <pod> --previous


Then:

kubectl get events --sort-by=.lastTimestamp

Common causes
1. Application crash
CrashLoopBackOff


Application starts and exits.

2. OOMKilled

Container exceeds memory limit.

3. Failed liveness probe

Kubernetes determines the container is unhealthy and restarts it.

4. Bad configuration

Examples:

Wrong environment variable
Missing Secret
Wrong ConfigMap
Invalid command/arguments
5. Dependency failure

Application cannot connect to:

Database
Redis
External API
6. Image problem
ImagePullBackOff
ErrImagePull

7. Permission/security issue

Container cannot access required files/resources.

Senior-level troubleshooting

I don't assume CrashLoopBackOff itself is the root cause. It's a symptom. I inspect the container's exit code, termination reason, previous logs, events, probes and resource limits to identify the actual failure.

17. Have you configured probes?

Yes.

Kubernetes commonly provides:

Liveness Probe

Checks whether the application is alive.

Readiness Probe

Checks whether the application is ready to receive traffic.

Startup Probe

Useful for slow-starting applications.

Example:

livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5

startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10

18. Do probes restart the pod?
Precise answer

Liveness probe failure can cause Kubernetes to restart the container.

But:

Readiness probe failure does not restart the container.

It removes the Pod from the Service's ready endpoints so that traffic is not sent to it.

Startup probe

If the startup probe keeps failing beyond its configured threshold, Kubernetes treats the container as failed and restarts it.

Easy way to remember
Startup  -> "Has my application started?"
Readiness -> "Can I receive traffic?"
Liveness  -> "Am I still alive?"

19. Who provides the CIDR range?
Interview Answer

In an enterprise environment, CIDR allocation is normally governed by the network/IPAM team because address ranges must be coordinated across the organization's network estate.

For example:

VNet: 10.20.0.0/16

Subnets:

AKS       10.20.1.0/24
App       10.20.2.0/24
Private   10.20.3.0/24
Firewall  10.20.4.0/24


Terraform can then consume those approved ranges:

address_space = ["10.20.0.0/16"]

Senior-level point

DevOps should not randomly select CIDRs in an enterprise. CIDRs must consider existing networks, peering, ExpressRoute/VPN, future growth and IP overlap.

20. Do you create VNet or does network team handle it?
Strong answer

It depends on the organization's operating model.

In a centralized enterprise model, the network team may own the hub VNet, firewall, ExpressRoute and connectivity, while the DevOps/platform team manages application spokes and workload subnets.

In a smaller environment, the DevOps team may provision the complete network using Terraform.

A common Azure enterprise pattern:

                 Hub VNet
                    |
       +------------+------------+
       |                         |
   Spoke VNet 1              Spoke VNet 2
       |                         |
      AKS                       Apps

Important

I prefer clearly defined ownership boundaries and Terraform-managed infrastructure wherever possible, rather than having different teams manually modify the same resources.

21. What is Azure Function and why is it required?
Interview Answer

Azure Functions is a serverless compute service used to execute code in response to events without managing the underlying servers.

It can be triggered by:

HTTP
Timer
Queue
Service Bus
Event Grid
Blob events

Example:

Blob uploaded
      |
      v
Event
      |
      v
Azure Function
      |
      v
Process file

Why use it?
Event-driven architecture
Reduced infrastructure management
Automatic scaling
Short-running workloads
Scheduled jobs
Lightweight APIs
Background processing
Senior-level point

I wouldn't use Functions simply because it's serverless. I choose it when the workload's execution model benefits from event-driven, elastic compute.

22. Azure services used to connect Azure Cloud with Azure DevOps

Common integrations include:

Azure DevOps
     |
     +-- Azure Resource Manager
     |
     +-- Azure Service Connections
     |
     +-- Azure Key Vault
     |
     +-- Azure Container Registry
     |
     +-- AKS
     |
     +-- Azure Storage
     |
     +-- Azure Monitor


For authentication, I prefer:

Workload identity federation
Managed identities where applicable
Service principals when required
Strong interview point

I avoid long-lived client secrets wherever possible. Modern pipelines should use federated identity or managed identity-based authentication.

23. How do you perform cost optimization?

I divide cost optimization into several areas.

1. Compute
Right-size VMs
Stop non-production resources outside working hours
Use autoscaling
Use appropriate VM families
Evaluate reserved capacity/savings plans
2. Kubernetes
Right-size CPU/memory requests
Configure HPA
Configure cluster autoscaling
Remove idle workloads
Use appropriate node pools
3. Storage
Lifecycle policies
Appropriate storage tier
Delete unused snapshots/disks
Archive cold data
4. Networking
Reduce unnecessary data transfer
Optimize architecture
Use appropriate egress architecture
5. Governance

Mandatory:

Cost Center
Environment
Application
Owner


tags.

Senior-level answer

Cost optimization should not mean simply choosing the cheapest resource. The objective is to optimize cost per business transaction while maintaining availability, performance and security.

24. Best practices for cost-effective and scalable applications

I follow the principle:

Scalability
+
Reliability
+
Security
+
Performance
+
Cost Efficiency

Architecture
Stateless application design
Horizontal scaling
Caching
Managed services where appropriate
Asynchronous processing
Database optimization
Kubernetes
HPA
 |
 +-- Scale Pods
 |
Cluster Autoscaler
 |
 +-- Scale Nodes

Infrastructure
Infrastructure as Code
Autoscaling
Right-sizing
Multi-zone architecture where required
Resource tagging
Policy enforcement
Application
Containerized workloads
Health endpoints
Graceful shutdown
Proper resource requests/limits
Observability
Senior-level point

Scalability should be designed around the actual bottleneck. Simply adding more Pods doesn't solve a database bottleneck.

25. Terraform variable data types

Terraform supports:

Primitive types
string
number
bool


Example:

variable "environment" {
  type = string
}

Collection types
list(string)
set(string)
map(string)


Example:

variable "regions" {
  type = list(string)
}

Structural types
object({
  name     = string
  location = string
})


and:

tuple([
  string,
  number,
  bool
])

Any
variable "config" {
  type = any
}

Interview-friendly summary
Primitive:
string
number
bool

Collection:
list
set
map

Structural:
object
tuple

Generic:
any

26. Terraform state file accidentally deleted — recovery?

This is a very important production question.

If using remote backend

This is one reason I prefer remote state.

For example:

Terraform
    |
    v
Azure Storage Account
    |
    +-- terraform.tfstate


If the local state is deleted, I can reinitialize Terraform and retrieve state from the remote backend.

terraform init
terraform state list
terraform plan

If the remote state itself is deleted

Recovery depends on the backend's backup/versioning capabilities.

For Azure Storage, I would check:

Blob versioning
Soft delete
Storage backups
Replication
Previous state versions
Worst case

If state cannot be recovered:

I would not immediately run terraform apply.

I would first identify the existing infrastructure and reconstruct/import resources into Terraform state.

Existing Infrastructure
        |
        v
Terraform Configuration
        |
        v
terraform import
        |
        v
Rebuilt State

Senior-level point

Terraform state is critical infrastructure data. I protect it with a remote backend, access control, encryption, versioning/recovery mechanisms and restricted write access.

27. Linux commands commonly used

I group commands by purpose.

Files
ls
cd
pwd
find
cp
mv
rm

Text processing
grep
awk
sed
cut
sort
uniq
head
tail

Processes
ps
top
htop
kill
pkill

Networking
curl
wget
ss
netstat
dig
nslookup
ping
traceroute

Disk
df -h
du -sh
lsblk

Memory/CPU
free -m
uptime
top
vmstat

Logs
journalctl
tail -f
grep

Permissions
chmod
chown
umask

Kubernetes troubleshooting
kubectl get
kubectl describe
kubectl logs
kubectl exec
kubectl top
kubectl get events

Senior-level statement

I don't just memorize Linux commands. I use them primarily for troubleshooting CPU, memory, disk, networking, processes, permissions and application behavior.

28. How does Argo CD work?
Interview Answer

Argo CD is a GitOps continuous delivery tool for Kubernetes. Git acts as the desired-state source of truth.

Architecture:

Developer
    |
    v
Git Repository
    |
    | Desired State
    v
Argo CD
    |
    | Compare
    v
Kubernetes Cluster
    |
    v
Actual State


Argo CD continuously compares:

Desired State
     VS
Actual State


If they differ:

OutOfSync


Argo CD can synchronize the cluster to the Git-defined state.

Typical workflow
Code Change
    |
    v
CI Pipeline
    |
    v
Build Docker Image
    |
    v
Container Registry
    |
    v
Update Helm values / manifest repository
    |
    v
Argo CD detects Git change
    |
    v
Deploy to AKS

Why GitOps?
Git becomes audit trail
Declarative deployments
Easy rollback
Drift detection
Better separation of CI and CD
Kubernetes state is reproducible
Strong senior-level point

I prefer CI to build and publish immutable artifacts, while Argo CD handles deployment. This creates a clean separation between artifact creation and environment reconciliation.

29. What is Helm?
Interview Answer

Helm is a package manager for Kubernetes. It allows us to package Kubernetes manifests into reusable, parameterized charts.

Typical structure:

myapp/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   └── configmap.yaml
└── charts/

values.yaml
replicaCount: 3

image:
  repository: myregistry/myapp
  tag: "1.2.3"

service:
  port: 80

Install
helm upgrade --install myapp ./myapp \
  --namespace production \
  --create-namespace

Why Helm?

Without Helm, we may have many environment-specific YAML files.

With Helm:

Same Chart
   |
   +-- dev values
   |
   +-- staging values
   |
   +-- production values

Helm + Argo CD

A strong production pattern is:

Git
 |
 +-- Application Code
 |
 +-- Helm Chart
 |
 +-- Environment Values
 |
 v
Argo CD
 |
 v
AKS


Argo CD renders the Helm chart and reconciles the resulting Kubernetes manifests against the cluster.
