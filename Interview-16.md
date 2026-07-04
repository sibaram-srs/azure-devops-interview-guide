# Senior Associate Infrastructure (Azure) L1 Interview Guide

---

# 1. Can you briefly introduce yourself and explain your current role and responsibilities?

### Interview Answer

Hi, I'm a DevOps Engineer with experience in Azure Cloud, Azure DevOps, Terraform, Docker, Kubernetes, and monitoring tools like Prometheus and Grafana.

In my current project, I am responsible for:

* Building and maintaining CI/CD pipelines using Azure DevOps.
* Provisioning Azure infrastructure using Terraform.
* Containerizing applications using Docker and deploying them on AKS.
* Managing Kubernetes deployments using Helm.
* Implementing monitoring with Prometheus and Grafana.
* Integrating security tools like SonarQube, Trivy, and Checkov into CI/CD pipelines.
* Supporting production deployments and troubleshooting infrastructure issues.

---

# 2. Can you describe the project you're currently working on and your responsibilities?

### Interview Answer

Currently, I'm working on a cloud-native application hosted on Azure.

My responsibilities include:

* Creating Infrastructure as Code using Terraform.
* Managing Azure DevOps pipelines.
* Deploying applications to AKS.
* Managing Azure networking resources like VNets, NSGs, and Application Gateway.
* Implementing monitoring and alerting.
* Troubleshooting deployment and infrastructure issues.

---

# 3. What CI/CD tools and strategies are you using in your current project?

### Interview Answer

We use:

* Azure Repos for source code
* Azure DevOps YAML Pipelines
* Terraform for Infrastructure as Code
* Docker for containerization
* AKS for deployments
* Helm for application deployment
* Azure Container Registry (ACR)

Pipeline flow:

```
Code Commit
      ↓
Build
      ↓
SonarQube
      ↓
Docker Build
      ↓
Trivy Scan
      ↓
Push to ACR
      ↓
Terraform
      ↓
Helm Deployment
```

---

# 4. What security tools have you integrated into your CI/CD pipeline?

### Interview Answer

In our pipeline we use:

* **SonarQube** → Code Quality
* **Trivy** → Docker Image Scanning
* **Checkov** → Terraform Security Scan
* **Snyk (optional)** → Dependency Vulnerability Scan

Security scanning is performed before deployment to prevent vulnerable code from reaching production.

---

# 5. How do you configure an Azure DevOps YAML pipeline to trigger automatically when a Pull Request is created or merged?

### Interview Answer

Using **trigger** and **pr** sections.

```yaml
trigger:
- main

pr:
- main
```

* **trigger** runs after merge.
* **pr** runs when a Pull Request is created or updated.

---

# 6. What is a parameterized YAML template in Azure DevOps, and why would you use it?

### Interview Answer

Parameterized templates allow us to reuse the same pipeline for multiple environments.

Example:

```yaml
parameters:
- name: environment
  type: string

steps:
- script: echo Deploying to ${{ parameters.environment }}
```

Benefits:

* Reusable
* Less duplication
* Easy maintenance
* Standardized pipelines

---

# 7. What is a Hub-and-Spoke network architecture in Azure?

### Interview Answer

Hub-and-Spoke is a centralized networking model.

* **Hub VNet** contains shared resources like:

  * Firewall
  * VPN Gateway
  * Bastion
  * Application Gateway

* **Spoke VNets** contain application workloads.

Benefits:

* Better security
* Centralized management
* Reduced cost
* Easy scalability

---

# 8. How do Hub and Spoke VNets communicate with each other?

### Interview Answer

They communicate using **VNet Peering**.

Traffic stays on Microsoft's backbone network.

Sometimes Azure Firewall or NVA in the Hub controls traffic between spokes.

---

# 9. How do you configure VNet Peering in Azure?

### Interview Answer

Steps:

1. Create both VNets.
2. Open Hub VNet.
3. Create Peering.
4. Select Spoke VNet.
5. Allow Virtual Network Access.
6. Save.

Can also be done using Terraform.

---

# 10. How would you troubleshoot communication issues between peered VNets?

### Interview Answer

I would check:

* VNet Peering status
* NSG rules
* Route Tables
* Firewall rules
* DNS resolution
* Effective Routes
* Network Watcher Connectivity Check

---

# 11. What is the role of a Listener in Azure Application Gateway?

### Interview Answer

A Listener accepts incoming client requests.

It listens on:

* IP Address
* Port
* Protocol (HTTP/HTTPS)
* Host Name (optional)

Then forwards traffic to Backend Pool using Routing Rules.

---

# 12. What configurations are required when creating an Application Gateway Listener?

### Interview Answer

Required configurations:

* Frontend IP
* Port
* HTTP or HTTPS
* SSL Certificate (HTTPS)
* Host Name (optional)
* Listener Type (Basic/Multi-site)

---

# 13. If an Azure Application Gateway is not working, how would you troubleshoot it?

### Interview Answer

I would check:

* Backend Health
* Listener
* Routing Rules
* NSG
* Health Probe
* DNS
* SSL Certificate
* Application Gateway Logs

Backend Health is usually the first place to check.

---

# 14. What is a Shared Access Signature (SAS) token, and when would you use it?

### Interview Answer

A SAS Token provides **temporary, secure access** to Azure Storage without sharing the account key.

Example:

* Upload files
* Download blobs
* Limited access for external users

It can be time-limited and permission-based.

---

# 15. What is the difference between ZRS and GRS?

| ZRS                                  | GRS                                |
| ------------------------------------ | ---------------------------------- |
| Replicates across Availability Zones | Replicates to another Azure Region |
| High Availability                    | Disaster Recovery                  |
| Same Region                          | Cross Region                       |

---

# 16. What are the different types of Managed Identities in Azure?

### Interview Answer

Two types:

### System Assigned

* Linked to one resource
* Deleted automatically with resource

Example:
Azure VM accessing Key Vault.

### User Assigned

* Independent Azure resource
* Can be attached to multiple resources

Example:
Multiple Azure Functions accessing Storage.

---

# 17. What is the difference between RTO and RPO?

### Interview Answer

**RTO (Recovery Time Objective)**

Maximum acceptable downtime.

Example:
Application should recover within 30 minutes.

---

**RPO (Recovery Point Objective)**

Maximum acceptable data loss.

Example:
Maximum 5 minutes of data loss.

---

# 18. If your database is hosted in East US and your Azure Function is deployed in West Europe, how do they communicate?

### Interview Answer

They communicate securely over Azure's network using:

* Private Endpoint
* VNet Integration
* VPN/ExpressRoute (if required)

The Function connects using the database endpoint and authentication credentials.

---

# 19. What factors would you consider to reduce latency?

### Interview Answer

* Deploy both resources in the same region.
* Use Private Endpoint.
* Optimize database queries.
* Use caching (Redis).
* Enable Connection Pooling.
* Choose the nearest Azure Region.

---

# 20. What happens internally when you execute `terraform init`?

### Interview Answer

`terraform init` performs:

* Downloads provider plugins
* Downloads modules
* Initializes backend
* Creates `.terraform` folder
* Creates lock file
* Prepares working directory

It does **not** create resources.

---

# 21. What are Terraform Provisioners, and when would you use them?

### Interview Answer

Provisioners are used to execute scripts or commands on a resource after it is created or destroyed.

Common types:

* `local-exec` → Runs commands on the machine executing Terraform.
* `remote-exec` → Runs commands on the created VM via SSH/WinRM.

Example use cases:

* Install software after VM creation.
* Run bootstrap scripts.

**Note:** Provisioners should be the last option. Prefer cloud-init, Custom Script Extension, or configuration management tools like Ansible whenever possible.

---

# 22. Write Terraform code to create three Resource Groups (A, B, and C) in the same region.

```hcl
variable "resource_groups" {
  default = ["A", "B", "C"]
}

resource "azurerm_resource_group" "rg" {
  for_each = toset(var.resource_groups)

  name     = each.value
  location = "Central India"
}
```

**Interview Tip:** Mention that `for_each` is preferred when resources have unique names.

---

# 23. What is the difference between `CMD` and `ENTRYPOINT` in a Dockerfile?

| CMD                        | ENTRYPOINT                           |
| -------------------------- | ------------------------------------ |
| Provides default command   | Defines the main executable          |
| Can be overridden easily   | Runs every time the container starts |
| Used for default arguments | Used for fixed startup commands      |

**Example**

```dockerfile
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```

---

# 24. How can you reduce the size of a Docker image?

### Interview Answer

Best practices:

* Use lightweight base images like `alpine`.
* Use multi-stage builds.
* Combine `RUN` commands.
* Remove temporary files and caches.
* Use `.dockerignore`.
* Install only required packages.

---

# 25. Write a multi-stage Dockerfile for a sample application.

```dockerfile
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app
COPY . .
RUN mvn clean package

FROM eclipse-temurin:17-jre-alpine

WORKDIR /app
COPY --from=builder /app/target/app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

**Interview Tip:** Explain that the final image contains only the JAR file and runtime, making it much smaller.

---

# 26. How do you reference a Docker image from a registry in a Kubernetes Deployment YAML?

```yaml
containers:
- name: app
  image: myacr.azurecr.io/sampleapp:v1
```

If the registry is private, create an Image Pull Secret:

```bash
kubectl create secret docker-registry acr-secret
```

Then reference it:

```yaml
imagePullSecrets:
- name: acr-secret
```

---

# 27. What is Helm, and how is it used in Kubernetes?

### Interview Answer

Helm is the package manager for Kubernetes.

It uses **Charts** to deploy applications.

Benefits:

* Easy deployments
* Version control
* Reusable templates
* Supports upgrades and rollbacks

Common commands:

```bash
helm install
helm upgrade
helm rollback
helm uninstall
```

---

# 28. How do you perform a rolling update in Kubernetes?

### Interview Answer

Update the image:

```bash
kubectl set image deployment/app app=myimage:v2
```

or

```bash
kubectl apply -f deployment.yaml
```

Kubernetes gradually replaces old Pods with new Pods without downtime.

Check status:

```bash
kubectl rollout status deployment/app
```

---

# 29. How do you roll back to a previous Deployment revision?

```bash
kubectl rollout undo deployment/app
```

Specific revision:

```bash
kubectl rollout undo deployment/app --to-revision=2
```

View history:

```bash
kubectl rollout history deployment/app
```

---

# 30. What is a Service Mesh, and why is it used?

### Interview Answer

A Service Mesh manages communication between microservices.

Popular tools:

* Istio
* Linkerd

Features:

* Traffic management
* Mutual TLS (mTLS)
* Service-to-service authentication
* Load balancing
* Observability
* Retries and circuit breaking

---

# 31. What monitoring tools have you used?

### Interview Answer

I have worked with:

* Prometheus for metrics collection
* Grafana for dashboards
* Azure Monitor
* Log Analytics
* Container Insights

These tools help monitor application health, resource usage, and alert on issues.

---

# 32. How do you integrate Prometheus and Grafana for Kubernetes cluster monitoring?

### Interview Answer

Steps:

1. Install Prometheus using Helm.
2. Install Grafana.
3. Configure Prometheus as Grafana's data source.
4. Import Kubernetes dashboards.
5. Configure alert rules.

Prometheus collects metrics, and Grafana visualizes them.

---

# 33. How does Prometheus collect metrics from an application?

### Interview Answer

Applications expose metrics on an endpoint such as:

```
/metrics
```

Prometheus periodically **scrapes** this endpoint based on its configuration (`scrape_configs`) or Kubernetes service discovery.

---

# 34. How does a newly created Kubernetes Pod automatically become discoverable by Prometheus?

### Interview Answer

Prometheus uses Kubernetes Service Discovery.

If:

* the Pod has the correct labels,
* a Service exposes it,
* and a ServiceMonitor/PodMonitor (or scrape configuration) matches those labels,

Prometheus automatically starts scraping the new Pod.

---

# 35. What is the purpose of the `package.json` file in a Node.js application?

### Interview Answer

`package.json` is the project's metadata file.

It contains:

* Project name and version
* Dependencies
* Scripts
* Entry point
* Author information

Example:

```bash
npm install
npm start
```

These commands use information from `package.json`.

---

# 36. An Azure Application Gateway is not routing traffic correctly. How would you troubleshoot it?

### Interview Answer

I would check in this order:

1. Backend Health
2. Listener configuration
3. Routing Rules
4. Backend Pool
5. Health Probe
6. NSG rules
7. SSL Certificate
8. DNS resolution
9. Application Gateway logs

**Interview Tip:** Mention "Backend Health" first, as it's a common root cause.

---

# 37. A newly deployed Kubernetes Pod is not appearing in Prometheus. How would you investigate?

### Interview Answer

I would verify:

* Pod is running.
* Labels match the ServiceMonitor/PodMonitor.
* Metrics endpoint (`/metrics`) is accessible.
* Prometheus target status.
* Service exists.
* Network policies are not blocking traffic.
* Prometheus logs for scrape errors.

---

# 38. How would you configure a pipeline that runs automatically whenever a Pull Request is raised or merged?

```yaml
trigger:
- main

pr:
- main
```

* `pr:` triggers validation for Pull Requests.
* `trigger:` runs after code is merged into the target branch.

---

# 39. Explain how you would securely connect an Azure Function in one region to a database deployed in another region.

### Interview Answer

I would:

* Enable Managed Identity for the Azure Function.
* Use a Private Endpoint for the database.
* Integrate the Function with a VNet if needed.
* Store secrets in Azure Key Vault.
* Use Private DNS for name resolution.
* Restrict public network access on the database.

This ensures secure communication over Azure's private network.

---

# L1 Rapid-Fire Questions (30-Second Answers)

### Difference between VM and AKS?

* VM is infrastructure where you manage the OS.
* AKS is a managed Kubernetes service where Azure manages the control plane.

### What is ACR?

Azure Container Registry stores Docker images privately.

### What is a Helm Chart?

A package containing Kubernetes manifests for deploying an application.

### Difference between ReplicaSet and Deployment?

Deployment manages ReplicaSets and supports rolling updates and rollbacks.

### What is Terraform State?

A file that tracks the current infrastructure managed by Terraform.

### What is `terraform plan`?

Shows the changes Terraform will make before applying them.

### What is `kubectl describe` used for?

Displays detailed information about a Kubernetes resource.

### Difference between ClusterIP and LoadBalancer?

* ClusterIP: Internal access only.
* LoadBalancer: Exposes the service externally.

### What is an NSG?

A Network Security Group controls inbound and outbound traffic to Azure resources.

### What is Azure Key Vault?

A secure service for storing secrets, certificates, and encryption keys.

---

# Final Interview Tips

* Keep answers concise (1–2 minutes).
* Whenever possible, relate your answer to your current project.
* Mention best practices (e.g., using Managed Identity instead of storing credentials, preferring `for_each` over `count` when appropriate, avoiding Terraform provisioners unless necessary).
* If you don't know an answer completely, explain the concept and describe how you would approach learning or troubleshooting it.
* For scenario questions, always describe your troubleshooting steps in a logical order rather than jumping to conclusions.

This guide covers the common L1 Azure DevOps, Azure, Terraform, Docker, Kubernetes, and monitoring questions you listed in an interview-ready format.

