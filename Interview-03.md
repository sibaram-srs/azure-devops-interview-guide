# The Placement House / EHCI Technologies – Senior DevOps Engineer

## Interview Ready Answers (Speak Like a Senior DevOps Engineer)

---

## 1. Tell Me About Yourself

"Hi, my name is SR. I have around 5 years of experience in Cloud and DevOps engineering with expertise in Azure Cloud, Azure DevOps, Terraform, Docker, Kubernetes, CI/CD, Infrastructure as Code, Monitoring, and DevSecOps practices.

Currently, I am working on Azure-based cloud infrastructure where I manage CI/CD pipelines, AKS clusters, infrastructure provisioning through Terraform, application deployments, monitoring, and production support.

My primary focus is automation, scalability, security, and reliability of cloud environments. I have hands-on experience in designing and maintaining enterprise-grade Azure infrastructure and Kubernetes platforms."

---

## 2. Day-to-Day Activities

"In my current role, my daily responsibilities include:

* Monitoring production environments.
* Managing Azure infrastructure.
* Supporting AKS clusters.
* Maintaining CI/CD pipelines.
* Terraform code development and enhancements.
* Troubleshooting deployment issues.
* Managing container images and registries.
* Handling incidents and service requests.
* Monitoring application and infrastructure health.
* Security and vulnerability remediation.
* Collaborating with development and QA teams for releases.

Additionally, I participate in root cause analysis and continuous improvement activities."

---

## 3. Rate Yourself in Azure

"I would rate myself around 8 out of 10 in Azure.

I am comfortable working with:

* Virtual Machines
* Storage Accounts
* VNETs
* NSGs
* Load Balancers
* Application Gateway
* Azure Key Vault
* Azure Monitor
* AKS
* Azure Container Registry
* Azure DevOps
* Managed Identities

I have strong hands-on experience in deploying and managing cloud-native workloads."

---

## 4. Azure DevOps or Jenkins?

"We primarily use Azure DevOps for CI/CD.

Azure DevOps provides:

* Repositories
* Pipelines
* Artifacts
* Boards
* Release Management

Since our infrastructure is hosted on Azure, Azure DevOps integrates seamlessly with our ecosystem."

---

## 5. Terraform Experience

"I have around X years of hands-on experience with Terraform.

I use Terraform for provisioning:

* Resource Groups
* VNETs
* Subnets
* Virtual Machines
* AKS
* Storage Accounts
* Load Balancers
* Key Vaults
* Application Gateways

I also create reusable modules and manage environments using separate tfvars files."

---

## 6. Jenkins Experience

"Yes, I have worked with Jenkins.

I have experience with:

* Freestyle Jobs
* Pipeline Jobs
* Declarative Pipelines
* Shared Libraries
* Webhooks
* Credentials Management
* Jenkins Agents

Although my current project uses Azure DevOps, understanding Jenkins helps because CI/CD concepts remain the same."

---

## 7. Scripting Experience

"Yes.

I primarily use:

### Bash

For:

* Linux administration
* Automation
* Monitoring scripts
* Deployment scripts

### Python

For:

* Automation tasks
* API integrations
* Data processing
* Infrastructure reporting

Among both, Bash is used more frequently in my daily DevOps activities."

---

## 8. What is ARM Template?

"ARM stands for Azure Resource Manager.

ARM Templates are Microsoft's native Infrastructure as Code solution for Azure.

They are JSON-based templates used to provision Azure resources automatically.

Today, Terraform is more commonly used because it is cloud-agnostic and easier to maintain."

---

## 9. What is Availability Set?

"Availability Set is an Azure feature that increases VM availability.

Azure distributes VMs across:

### Fault Domains

Protect against hardware failures.

### Update Domains

Protect against planned maintenance.

If one server fails, the other VM continues serving traffic."

---

## 10. Application Deployment Options in Azure

"Applications can be deployed on multiple Azure services depending on requirements.

### Virtual Machines

Traditional applications.

### Azure App Service

Web applications.

### Azure Kubernetes Service (AKS)

Containerized microservices.

### Azure Container Apps

Serverless containers.

### Azure Functions

Event-driven workloads.

### Azure Container Instances

Lightweight container execution."

---

## 11. How Do You Monitor?

"We use:

### Prometheus

Metrics collection.

### Grafana

Visualization.

### Azure Monitor

Infrastructure monitoring.

### Application Insights

Application performance monitoring.

### Log Analytics

Centralized log analysis."

---

## 12. Multiple Application Log Monitoring

"Yes.

Logs from applications, AKS, and infrastructure are sent to Log Analytics Workspace.

For Azure environments, Azure Monitor and Log Analytics are generally the best options because they provide:

* Centralized logging
* Querying through KQL
* Alerting
* Dashboarding
* Long-term retention"

---

## 13. Azure Only or On-Prem?

"My primary responsibility is Azure Cloud.

However, I also support hybrid environments where on-premises systems integrate with Azure services through VPNs, ExpressRoute, or private networking."

---

## 14. AKS Alone or Team?

"I work as part of a DevOps team.

However, I independently handle:

* AKS deployments
* Troubleshooting
* Namespace management
* Ingress configuration
* Monitoring
* Scaling
* Upgrades

For major architecture decisions, we collaborate as a team."

---

## 15. How Do You Deploy on AKS?

### Deployment Flow

Developer Commit
↓

Azure DevOps Pipeline
↓

Docker Build
↓

Push to ACR
↓

AKS Deployment

We use:

```bash
kubectl apply -f deployment.yaml
```

or

```bash
helm upgrade --install
```

depending on project standards.

---

## 16. Pod Down – Troubleshooting

First verify pod status:

```bash
kubectl get pods -A
```

Check details:

```bash
kubectl describe pod <pod-name>
```

Check events:

```bash
kubectl get events
```

Check node health:

```bash
kubectl get nodes
```

Check logs:

```bash
kubectl logs <pod-name>
```

This usually identifies the root cause quickly.

---

## 17. How to Check Pod Logs?

Standard command:

```bash
kubectl logs <pod-name>
```

Multiple containers:

```bash
kubectl logs <pod-name> -c <container-name>
```

If pod restarted:

```bash
kubectl logs --previous <pod-name>
```

This is a favorite interview question.

---

## 18. CrashLoopBackOff but CPU & Memory Fine

My troubleshooting approach:

### Check Previous Logs

```bash
kubectl logs --previous <pod-name>
```

### Check Events

```bash
kubectl describe pod <pod-name>
```

### Verify

* Wrong Environment Variables
* Secret Issues
* ConfigMap Issues
* Application Startup Failure
* Database Connectivity
* Wrong Image Version
* Port Misconfiguration

Most CrashLoopBackOff issues are application-related rather than infrastructure-related.

---

## 19. Verify Correct Image

Check deployment image:

```bash
kubectl describe deployment <deployment-name>
```

or

```bash
kubectl get deployment <deployment-name> -o yaml
```

Check image in ACR:

```bash
az acr repository show-tags \
--name myacr \
--repository myapp
```

Verify deployed image tag matches the expected release version.

---

## 20. Simple YAML for Pod

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-pod

spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

Apply:

```bash
kubectl apply -f pod.yaml
```

---

## 21. Who Writes Pipeline YAML?

"As a DevOps Engineer, I write and maintain pipeline YAML files myself.

Developers provide build requirements, but pipeline design, optimization, security integration, approvals, deployments, and release automation are usually handled by the DevOps team."

---

## 22. Git Operations

"We use Git through Azure Repos and VS Code.

Common activities:

```bash
git clone
git pull
git add
git commit
git push
git branch
git checkout
git merge
git rebase
git tag
```

I also participate in pull request reviews and branch strategy management."

---

## 23. How Many Environments?

"We typically manage:

### Development

### QA/Test

### UAT

### Staging

### Production

Each environment has separate configurations, secrets, and deployment approvals."

---

## 24. Production Down – Recovery Process

"My approach is:

### Step 1

Acknowledge incident immediately.

### Step 2

Check monitoring dashboards and alerts.

### Step 3

Identify impact and affected services.

### Step 4

Review application logs and infrastructure metrics.

### Step 5

Rollback deployment if required.

### Step 6

Restore service.

### Step 7

Perform RCA.

### Step 8

Implement preventive actions.

The priority is always service restoration first and RCA second."

---

## 25. Jira Usage

"Yes.

We use Jira extensively for:

* Incident Tracking
* Change Requests
* User Stories
* Sprint Planning
* Bug Tracking
* Production Issues
* Release Management

Every production change and operational activity is tracked through Jira tickets."

---

# Questions That Create a Strong Impression

At the end ask:

**"How mature is your Kubernetes and Infrastructure as Code adoption?"**

**"What are the biggest DevOps challenges the team is trying to solve today?"**

**"What would be the expectations from this role in the first 90 days?"**

These questions make you sound like a Senior DevOps Engineer rather than someone only looking for a job.
