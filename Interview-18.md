#  Senior DevOps Engineer (Interview Ready Answers)

---

## 1. Could you please provide an introduction of yourself and your experience?

**Answer:**

I have around **8+ years of experience** as a DevOps Engineer, primarily working on Azure Cloud, Kubernetes, Terraform, Azure DevOps, Docker, and CI/CD automation. I've been responsible for designing cloud infrastructure, building deployment pipelines, automating infrastructure using Terraform, managing AKS clusters, implementing monitoring solutions, and supporting production environments. I closely work with developers, architects, and security teams to ensure reliable, scalable, and secure deployments.

---

## 2. Have you worked with database or integration services?

**Answer:**

Yes. I've worked with Azure SQL Database, Azure Storage Accounts, Cosmos DB, Azure Key Vault, Azure Service Bus, API Management, Function Apps, Logic Apps, and Event Grid. My responsibilities include provisioning, networking, security, monitoring, and CI/CD automation for these services.

---

## 3. Do you have experience in both writing infrastructure code and CI/CD pipeline code?

**Answer:**

Yes. Infrastructure as Code and CI/CD automation are my primary responsibilities. I write Terraform modules to provision Azure infrastructure and develop Azure DevOps YAML pipelines for build, test, infrastructure deployment, application deployment, approvals, and rollback strategies.

---

## 4. Are you working as an individual contributor or as part of a team?

**Answer:**

I primarily work as a Senior Individual Contributor while collaborating closely with developers, architects, QA, security teams, and project managers in Agile Scrum teams.

---

## 5. How is Kubernetes architecture designed in a master and worker node style?

**Answer:**

Kubernetes consists of two main layers:

### Control Plane (Master Node)

- API Server
- Scheduler
- Controller Manager
- etcd Database
- Cloud Controller Manager (optional)

The Control Plane manages the entire cluster and schedules workloads.

### Worker Node

- Kubelet
- Container Runtime (containerd/Docker)
- Kube Proxy
- Pods

Worker nodes host and run the application containers.

---

## 6. What are the components of a master node and a worker node?

**Answer:**

### Master Node

- kube-apiserver
- etcd
- kube-scheduler
- kube-controller-manager
- cloud-controller-manager

### Worker Node

- kubelet
- kube-proxy
- container runtime
- pods

---

## 7. If there is an issue with a pod or code not working, how do you investigate and propose a fix?

**Answer:**

I follow a structured troubleshooting approach:

1. Check pod status.
2. Describe the pod for events.
3. Review application logs.
4. Verify image version.
5. Check ConfigMaps and Secrets.
6. Verify readiness and liveness probes.
7. Check resource utilization.
8. Connect inside the pod if required.
9. Identify root cause.
10. Fix the issue and redeploy through the CI/CD pipeline.

Useful commands:

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl logs -f <pod-name>

kubectl logs <pod-name> -c <container-name>

kubectl exec -it <pod-name> -- bash

kubectl get events

kubectl top pod
```

---

## 8. What commands do you use to check pod details and logs?

**Answer:**

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl logs -f <pod-name>

kubectl logs <pod-name> -c <container-name>

kubectl exec -it <pod-name> -- bash

kubectl get events

kubectl top pod
```

---

## 9. What kind of Azure resources do you typically provision using Terraform?

**Answer:**

I have provisioned:

- Resource Groups
- VNets
- Subnets
- NSGs
- Route Tables
- Public IPs
- Azure Firewall
- Load Balancers
- Application Gateway
- Azure Front Door
- Virtual Machines
- VM Scale Sets
- AKS
- Azure Container Registry
- Storage Accounts
- Key Vault
- App Services
- Function Apps
- API Management
- Azure SQL
- Cosmos DB
- Private Endpoints
- Log Analytics Workspace
- Azure Monitor

---

## 10. What is a Terraform state file, and what purpose does it serve?

**Answer:**

The Terraform state file (`terraform.tfstate`) stores the mapping between Terraform configuration and actual infrastructure resources.

It is used to:

- Track deployed resources
- Compare desired state with current infrastructure
- Detect infrastructure drift
- Generate execution plans
- Prevent duplicate resource creation

For production environments, we store the state remotely in Azure Storage with state locking enabled.

---

## 11. How do you manage a virtual machine in Terraform that was originally created manually in the Azure portal?

**Answer:**

I first write the Terraform configuration matching the existing VM.

Then I import it into the Terraform state.

```bash
terraform import azurerm_linux_virtual_machine.vm <resource-id>
```

After importing:

```bash
terraform plan
```

This ensures Terraform can manage the resource without recreating it.

---

## 12. What specific code do you write for a virtual machine resource?

**Answer:**

A typical VM deployment includes:

- Resource Group
- Virtual Network
- Subnet
- Network Interface
- Public IP
- Network Security Group
- Managed Disk
- Linux or Windows Virtual Machine
- Image Reference
- VM Size
- Admin Credentials
- Managed Identity
- Boot Diagnostics

---

## 13. What happens if you try to apply code for a resource that already exists manually?

**Answer:**

Terraform attempts to create the resource again because it does not exist in the state file, resulting in a **Resource Already Exists** error.

The correct approach is to import the resource into the Terraform state before managing it.

---

## 14. What kind of applications are you typically deploying?

**Answer:**

I have deployed:

- Java Spring Boot
- .NET Core
- NodeJS
- Python
- Angular
- React

Applications are typically deployed to AKS, Azure App Service, or Virtual Machines.

---

## 15. Can you describe the end-to-end CI/CD pipeline for a microservices project?

**Answer:**

### Continuous Integration (CI)

- Source Code Checkout
- Dependency Installation
- Static Code Analysis
- Unit Testing
- Build Application
- Build Docker Image
- Vulnerability Scan
- Push Image to Azure Container Registry

### Continuous Deployment (CD)

- Deploy Infrastructure using Terraform
- Deploy Helm Charts or Kubernetes Manifests
- Smoke Tests
- QA Deployment
- UAT Deployment
- Production Approval
- Production Deployment
- Health Validation
- Rollback if Required

---

## 16. Can you write Terraform code for an App Service from memory?

**Answer:**

Yes.

I can write the overall structure including:

- Resource Group
- Service Plan
- App Service
- Runtime Stack
- App Settings
- Managed Identity
- Key Vault References
- VNet Integration
- Diagnostic Settings

I may refer to the provider documentation for less commonly used attributes.

---

## 17. What is the specific Docker command for building an image?

**Answer:**

```bash
docker build -t myapp:v1 .
```

To push the image:

```bash
docker push myacr.azurecr.io/myapp:v1
```

---

## 18. What are the different instructions used in a Dockerfile?

**Answer:**

Common Dockerfile instructions include:

- FROM
- LABEL
- WORKDIR
- COPY
- ADD
- RUN
- ENV
- ARG
- EXPOSE
- CMD
- ENTRYPOINT
- USER
- HEALTHCHECK
- VOLUME

---

## 19. How do you use multi-stage builds in Docker?

**Answer:**

Multi-stage builds separate the build environment from the runtime environment.

- The first stage compiles the application.
- The final stage copies only the compiled artifacts into a lightweight runtime image.

Benefits include:

- Smaller image size
- Better security
- Faster deployments
- Reduced attack surface

---

## 20. What is Azure Service Bus, and what is it used for?

**Answer:**

Azure Service Bus is a fully managed messaging service used for asynchronous communication between applications and microservices.

It supports:

- Queues
- Topics
- Publish/Subscribe Pattern
- Dead Letter Queues
- Reliable Messaging
- Retry Mechanisms
- Ordered Message Delivery

---

## 21. How do you use Azure Monitor to track infrastructure health, and what thresholds/alerts do you set?

**Answer:**

I integrate Azure Monitor with Log Analytics and Application Insights.

Typical alerts include:

- CPU > 80%
- Memory > 80%
- Disk Usage > 85%
- Pod Restart Count
- Node Not Ready
- HTTP 5xx Errors
- High Response Time
- Storage Capacity
- Failed Deployments

Alerts are sent through Azure Action Groups to Email, Teams, or ITSM tools.

---

## 22. What is Prometheus, and from where does it collect metrics?

**Answer:**

Prometheus is an open-source monitoring system that follows a **pull model**.

It scrapes metrics from:

- Kubernetes Nodes
- kube-state-metrics
- cAdvisor
- Node Exporter
- Application `/metrics` endpoints
- Blackbox Exporter

Prometheus stores metrics, while Grafana is typically used for visualization.

---

## 23. Do you need to install an agent on a VM for Prometheus to collect data?

**Answer:**

Yes.

For Linux VMs, we install **Node Exporter**.

For Windows VMs, we install **Windows Exporter**.

Prometheus periodically scrapes metrics exposed by these exporters over HTTP.

---

## 24. What are the most difficult challenges you have faced in your DevOps role, and how did you overcome them?

**Answer:**

One major challenge was intermittent production deployment failures caused by configuration inconsistencies across environments.

To resolve this:

- Standardized infrastructure using Terraform modules.
- Created reusable Azure DevOps YAML templates.
- Added automated validation and approval gates.
- Implemented deployment health checks and rollback mechanisms.

This significantly improved deployment reliability and reduced production incidents.

---

## 25. How do you handle issues during the application deployment phase, such as image tagging issues?

**Answer:**

I verify:

- The Docker image was successfully built.
- The image was pushed to Azure Container Registry.
- The deployment manifest references the correct image tag.
- Registry authentication is working.
- Kubernetes image pull events.
- Pipeline variables.

I avoid using the `latest` tag and instead use immutable tags such as the Build ID or Git Commit SHA.

---

## 26. What is your branching strategy, and how do you manage code merges?

**Answer:**

We follow **Trunk-Based Development**.

Workflow:

1. Create a short-lived feature branch.
2. Develop and commit changes.
3. Raise a Pull Request.
4. Automated build, testing, and code quality checks execute.
5. Peer review is completed.
6. Merge into the main branch.
7. The CI/CD pipeline automatically deploys the application to the target environment.

This strategy keeps the main branch stable while enabling frequent and reliable deployments.
