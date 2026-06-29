
## Interview Ready Answers (Microservices + AKS + Azure DevOps + Docker + Grafana)

Nagarro generally focuses on practical DevOps implementation. Expect deep questions on **CI/CD, Docker, Kubernetes, Monitoring, and Production Deployments**.

---

# 1. Introduction, Roles & Responsibilities

### Interview Ready Answer

"Hi, my name is SR. I have around 5 years of experience in DevOps and Cloud Engineering with expertise in Azure Cloud, Azure DevOps, Terraform, Docker, Kubernetes, CI/CD, Monitoring, and DevSecOps.

Currently, I work on Azure-based microservices applications deployed on AKS. My responsibilities include:

* Infrastructure provisioning using Terraform
* CI/CD pipeline development using Azure DevOps
* Docker image management
* Kubernetes deployments
* Monitoring using Prometheus and Grafana
* Production support and troubleshooting
* Security integration and automation

My primary focus is automation, scalability, reliability, and operational excellence."

---

# 2. What Technologies Are You Working With?

### Interview Answer

"I work extensively with:

### Cloud

* Azure

### CI/CD

* Azure DevOps
* Jenkins (previous experience)

### Containers

* Docker
* AKS

### IaC

* Terraform

### Monitoring

* Prometheus
* Grafana
* Azure Monitor

### Scripting

* Bash
* PowerShell
* Python

### Version Control

* Git
* Azure Repos
* GitHub"

---

# 3. Are You Using Azure DevOps?

### Interview Answer

"Yes.

Azure DevOps is our primary CI/CD platform.

We use:

* Azure Repos
* Azure Pipelines
* Azure Artifacts
* Boards
* Release Approvals

Most pipelines are YAML-based."

---

# 4. How Is Your CI/CD Flow Created?

### Interview Answer

### CI Flow

Developer Commit
↓

Pull Request
↓

Code Review
↓

Build
↓

Unit Test
↓

SonarQube Scan
↓

Security Scan
↓

Docker Build
↓

Push to ACR

### CD Flow

Deploy Dev
↓

Testing
↓

Deploy QA/UAT
↓

Approval
↓

Production Deployment

All stages are automated through Azure DevOps YAML pipelines."

---

# 5. Infrastructure Pipelines or Application Pipelines?

### Interview Answer

"Both.

### Infrastructure Pipeline

Terraform Plan
↓

Approval
↓

Terraform Apply

### Application Pipeline

Build
↓

Test
↓

Docker Image Creation
↓

Push to Registry
↓

AKS Deployment

Both pipelines are maintained separately but integrated within the release process."

---

# 6. End-to-End Microservices CI/CD Flow

### Interview Answer

This is a favorite interview question.

### Step 1

Developer pushes code.

### Step 2

CI Pipeline triggers.

### Step 3

Compile application.

### Step 4

Run unit tests.

### Step 5

Run SonarQube quality checks.

### Step 6

Run security scanning.

### Step 7

Build Docker image.

### Step 8

Tag image.

### Step 9

Push image to ACR.

### Step 10

CD Pipeline deploys image to AKS.

### Step 11

Run health checks.

### Step 12

Monitor deployment.

### Step 13

Approve Production Release.

### Step 14

Deploy Production.

### Step 15

Continuous Monitoring.

---

# 7. YAML or Classic Pipelines?

### Interview Answer

"We use YAML pipelines.

Benefits:

* Version Controlled
* Reusable
* Easier Maintenance
* GitOps Friendly

Classic pipelines are mostly used in older projects."

---

# 8. How Are Docker Tags Maintained?

### Interview Answer

"We use unique version tags generated during pipeline execution.

Common tagging strategy:

```text
myapp:1.0.5
myapp:20260621.15
myapp:<BuildID>
```

This ensures every image is uniquely identifiable."

---

# 9. What Image Tagging Strategy Do You Follow?

### Interview Answer

Best practice:

```text
Application Name
+
Build Number
+
Git Commit ID
```

Example:

```text
orderservice:v1.2.15-abc123
```

This provides complete traceability between source code and deployment."

---

# 10. Docker Build Command

### Interview Answer

```bash
docker build -t myapp:v1 .
```

### Breakdown

```bash
-t
```

Tag image.

```bash
.
```

Current directory containing Dockerfile.

---

# 11. Have You Worked on Dockerfiles?

### Interview Answer

"Yes.

I create and maintain Dockerfiles regularly.

Typical activities:

* Multi-stage builds
* Base image selection
* Security hardening
* Layer optimization
* Environment configuration"

---

# 12. Is Dockerfile Required?

### Interview Answer

"Yes.

Docker build uses the Dockerfile as a blueprint.

Example:

```dockerfile
FROM openjdk:17

COPY app.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

When Docker build runs, it reads instructions from the Dockerfile and creates the image."

---

# 13. How Do You Ensure Same Image Gets Deployed?

### Interview Answer

"We use immutable image tags.

Pipeline:

Build Image
↓

Tag Image
↓

Push to ACR
↓

Store Artifact Information
↓

Deploy Same Tag

Example:

```text
orderservice:v1.2.15
```

The deployment YAML references the exact tag generated during build.

We never deploy using:

```text
latest
```

because it can lead to inconsistencies."

---

# 14. Kubernetes Service Types

### Interview Answer

### ClusterIP

Internal communication.

### NodePort

Exposes service via node port.

### LoadBalancer

External access through cloud load balancer.

### ExternalName

Maps service to external DNS.

Most production AKS workloads use:

* ClusterIP
* LoadBalancer
* Ingress"

---

# 15. ConfigMaps and Secrets

### Interview Answer

ConfigMaps store:

* URLs
* Environment Variables
* Configuration Data

Secrets store:

* Passwords
* API Keys
* Tokens

Deployment Example:

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

and

```yaml
envFrom:
  - secretRef:
      name: app-secret
```

---

# 16. Kubernetes End-to-End Use Case

### Interview Answer

"One project involved migrating a monolithic application to microservices on AKS.

Responsibilities included:

* Dockerization
* ACR Setup
* AKS Deployment
* Ingress Configuration
* Monitoring Setup
* CI/CD Automation
* Auto Scaling

The complete deployment lifecycle was managed through Azure DevOps."

---

# 17. What is Grafana?

### Interview Answer

"Grafana is a visualization and dashboarding platform.

It displays metrics from data sources such as:

* Prometheus
* Azure Monitor
* Loki
* Elasticsearch

Grafana itself doesn't collect data. It visualizes data collected by monitoring systems."

---

# 18. How Is Logging Done with Grafana?

### Interview Answer

"Grafana does not directly store logs.

Logs are usually stored in:

* Loki
* Elasticsearch
* Azure Log Analytics

Grafana connects to these sources and displays logs through dashboards."

---

# 19. How Can Logs Be Embedded in Grafana Dashboard?

### Interview Answer

"Grafana can integrate with Loki or Log Analytics.

Once connected:

Dashboard
↓

Add Panel
↓

Select Log Data Source

Examples:

* Loki
* Elasticsearch
* Azure Monitor Logs

Logs can then be visualized alongside metrics."

---

# 20. Which Versions Are You Using?

### Interview Answer

Never memorize exact versions.

Say:

> "Versions change frequently across environments. Currently we are using recent stable enterprise-supported versions of AKS, Docker, Prometheus, Grafana, Terraform, and Azure DevOps. I generally verify exact versions from the environment whenever needed."

This is the safest answer.

---

# 21. How Prometheus Integrates with Grafana?

### Interview Answer

### Step 1

Prometheus scrapes metrics.

### Step 2

Grafana adds Prometheus as a Data Source.

### Step 3

Grafana queries Prometheus using PromQL.

### Step 4

Dashboards visualize metrics.

Flow:

```text
Application
↓
Prometheus
↓
Grafana
```

This is the most common monitoring architecture."

---

# 22. Any Other Way to Connect?

### Interview Answer

"Yes.

Prometheus doesn't require special Grafana configuration.

Grafana independently connects to Prometheus through its endpoint.

Example:

```text
http://prometheus-server:9090
```

Once Grafana can reach Prometheus, it can query metrics directly.

Grafana and Prometheus are loosely coupled, which makes the architecture flexible and scalable."

---
