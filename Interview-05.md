## High-Probability Interview Answers (Senior Level)

---

# 1. Introduce Yourself

### Interview Ready Answer

"Hi, my name is SR. I have around 5 years of experience in DevOps and Cloud Engineering with expertise in Azure Cloud, Azure DevOps, Terraform, Docker, Kubernetes, Jenkins, CI/CD, Infrastructure Automation, Monitoring, and DevSecOps.

Currently, I am working on an Azure-based microservices platform hosted on AKS. My responsibilities include managing cloud infrastructure, Terraform automation, CI/CD pipeline implementation, Kubernetes administration, production deployments, monitoring, troubleshooting, and security integration.

I work closely with development teams to ensure reliable, secure, and automated software delivery while maintaining high availability and scalability."

---

# 2. Jenkins or Azure DevOps?

"We primarily use Azure DevOps, but I also have experience with Jenkins.

In Jenkins, I have worked on:

* Freestyle Jobs
* Declarative Pipelines
* Shared Libraries
* Webhooks
* Agent Configuration

The core CI/CD concepts remain the same regardless of the tool."

---

# 3. Python or Shell Scripting?

"I use both.

### Bash/Shell

* Server automation
* Deployment automation
* Health checks
* Log analysis

### Python

* API automation
* Reporting
* Infrastructure automation
* Data processing

Bash is used daily, while Python is generally used for advanced automation."

---

# 4. Mutable vs Immutable

### Interview Answer

| Mutable                       | Immutable                    |
| ----------------------------- | ---------------------------- |
| Can change after creation     | Cannot change after creation |
| List, Dictionary, Set         | String, Tuple, Integer       |
| Memory reference remains same | New object created           |

Example:

```python
list1.append("new")
```

List changes directly because it is mutable.

---

# 5. Zero Downtime Deployment Strategy

### Interview Answer

"I prefer Blue-Green Deployment.

Process:

Current Production → Blue

New Version → Green

Steps:

1. Deploy new version to Green.
2. Validate functionality.
3. Shift traffic gradually.
4. Monitor health metrics.
5. Keep Blue available for rollback.

This ensures zero downtime and quick rollback capability."

---

# 6. What is Canary Deployment?

### Interview Answer

"Canary deployment releases a new version to a small percentage of users before rolling it out completely.

Example:

* 10% users → New Version
* 90% users → Old Version

If monitoring looks healthy:

10% → 25% → 50% → 100%

This minimizes risk during production releases."

---

# 7. Optimize Slow CI/CD Pipeline

### Interview Answer

My approach:

### Analyze Pipeline Stages

Identify bottlenecks.

### Enable Parallel Jobs

Run:

* Unit Tests
* Security Scans
* Linting

simultaneously.

### Use Build Caching

Cache:

* Maven Dependencies
* NPM Packages
* Docker Layers

### Incremental Builds

Build only changed components.

### Optimize Docker Builds

Use multi-stage builds.

### Scale Build Agents

Increase build agent capacity if required.

---

# 8. Parallel Jobs & Cache Dependencies

### Interview Answer

"Yes.

Parallelization reduces execution time significantly.

Caching generally improves performance, but incorrect cache configurations can:

* Use stale dependencies
* Cause cache corruption
* Increase restore time

Therefore cache strategies must be reviewed periodically."

---

# 9. Ensure Deployment Reliability

### Interview Answer

I use multiple controls:

* Automated Testing
* Infrastructure as Code
* Canary/Blue-Green Deployments
* Health Checks
* Approval Gates
* Monitoring
* Rollback Mechanisms
* Immutable Artifacts

The goal is predictable and repeatable deployments."

---

# 10. Deployment Success but Application Fails

### Interview Answer

My troubleshooting approach:

### Check Logs

```bash
kubectl logs pod-name
```

### Verify

* Environment Variables
* Secrets
* ConfigMaps
* Database Connectivity
* Third-party Integrations

### Compare

Previous Working Version vs New Version

### Rollback

If impact is significant:

```bash
kubectl rollout undo deployment/app
```

Restore service first, then investigate."

---

# 11. Pipeline Success But Version Not Reflected

### Interview Answer

I verify:

### Image Tag

Correct image pushed?

### Deployment

Correct deployment updated?

```bash
kubectl get deployment
```

### Pod Image

```bash
kubectl describe pod
```

### Browser Cache

### CDN Cache

### Rollout Status

```bash
kubectl rollout status deployment/app
```

Most common issue is image tag reuse."

---

# 12. Pipeline Stuck

### Interview Answer

Check:

* Agent Availability
* Resource Limits
* Waiting Approval Gates
* Network Issues
* External Dependencies
* Build Logs

In Azure DevOps, logs usually reveal the exact stage causing blockage."

---

# 13. Declarative vs Scripted Pipeline

| Declarative         | Scripted          |
| ------------------- | ----------------- |
| Easy to read        | Flexible          |
| Structured syntax   | Full Groovy       |
| Easier maintenance  | More coding       |
| Enterprise standard | Complex workflows |

Most organizations prefer Declarative Pipelines."

---

# 14. How Do You Handle Secrets?

### Interview Answer

"We store secrets in Azure Key Vault.

Examples:

* Database Passwords
* API Keys
* Certificates
* Connection Strings

Applications access secrets dynamically using Managed Identity."

---

# 15. Fetch Secrets Without Exposure

### Interview Answer

Best practice:

* Store in Key Vault
* Use Managed Identity
* Inject at runtime
* Never hardcode secrets

Example:

```bash
az keyvault secret show
```

The secret remains encrypted and controlled through RBAC."

---

# 16. Highly Available Azure Architecture

### Interview Answer

Components:

* Availability Zones
* Load Balancer
* VM Scale Set
* AKS
* Azure SQL HA
* Geo-redundant Storage

Traffic Flow:

User
↓

Application Gateway
↓

Load Balancer
↓

Multiple Zones

This eliminates single points of failure."

---

# 17. Availability Set vs Availability Zone

| Availability Set        | Availability Zone        |
| ----------------------- | ------------------------ |
| Same Datacenter         | Separate Datacenter      |
| Fault Domain Protection | Physical Zone Protection |
| Lower HA                | Higher HA                |
| Legacy Approach         | Recommended              |

---

# 18. VMSS (VM Scale Set)

### Interview Answer

"VMSS automatically increases or decreases VM instances based on demand.

Scaling triggers:

* CPU
* Memory
* Custom Metrics

Benefits:

* Auto Scaling
* High Availability
* Reduced Operational Effort"

---

# 19. Disaster Recovery Strategy

### Interview Answer

DR Components:

* Backup Strategy
* Geo Replication
* Multi-region Deployment
* Terraform Code Backup
* Database Replication
* Recovery Runbooks

Recovery objectives:

* RTO
* RPO

must be clearly defined."

---

# 20. Monitoring & Log Analytics

### Interview Answer

Tools:

* Azure Monitor
* Log Analytics
* Application Insights
* Prometheus
* Grafana

Log Analytics allows querying logs using KQL and creating alerts, dashboards, and reports."

---

# 21. Bulk VM Start/Stop

### Interview Answer

Using PowerShell:

```powershell
Get-AzVM | Start-AzVM
```

Or Azure CLI:

```bash
az vm start
```

Usually automated through scripts or Automation Accounts."

---

# 22. Auto Cleanup Old Snapshots

### Interview Answer

"Yes.

We automate cleanup using:

* Azure Automation
* PowerShell
* Azure Functions

Retention policies automatically remove old snapshots to reduce storage costs."

---

# 23. Restart Web Apps Not Running

### Interview Answer

Azure CLI:

```bash
az webapp restart
```

Or:

Azure Portal → Web App → Restart

Usually automated through monitoring alerts."

---

# 24. Cloud Cost Suddenly Increased

### Interview Answer

I check:

### Azure Cost Analysis

### Recently Created Resources

### VM Usage

### Storage Growth

### Data Transfer

### AKS Scaling

### Snapshot Growth

### Load Testing Activities

Cost spikes are usually caused by overprovisioned resources or uncontrolled scaling."

---

# 25. Infrastructure Lifecycle with Terraform

### Interview Answer

Lifecycle:

```text
Write Code
↓
terraform init
↓
terraform plan
↓
terraform apply
↓
Monitor
↓
terraform destroy
```

Everything remains version controlled."

---

# 26. Remote State & Locking

### Interview Answer

"We store Terraform state remotely in Azure Storage Account.

Benefits:

* Centralized State
* Team Collaboration
* State Locking
* Backup Protection

Locking prevents simultaneous modifications."

---

# 27. Terraform Plan vs Apply

### Interview Answer

### Plan

```bash
terraform plan
```

Shows proposed changes.

### Apply

```bash
terraform apply
```

Executes changes.

Plan = Preview

Apply = Execution

---

# 28. Multiple Environments

### Interview Answer

Preferred Enterprise Structure:

```text
modules/
dev/
test/
prod/
```

Workspaces are useful for small setups.

Modular structure is preferred for enterprise environments."

---

# 29. Count vs For_Each

### Interview Answer

### Count

```hcl
count = 3
```

Index-based.

### For_Each

```hcl
for_each = var.vms
```

Key-based.

For_each is safer because resource identity remains stable."

---

# 30. Break Terraform Lock

```bash
terraform force-unlock LOCK_ID
```

Used only after confirming no active Terraform operation exists."

---

# 31. Remote State in GCS

### Interview Answer

"GCS stores Terraform state centrally similar to Azure Storage or S3.

Benefits:

* Shared State
* Backup
* Collaboration
* Security"

---

# 32. State Drift

### Interview Answer

"State drift occurs when infrastructure is modified outside Terraform."

Example:

Engineer changes VM manually.

Terraform state differs from actual infrastructure.

Fix:

```bash
terraform import
```

or

```bash
terraform refresh
```

---

# 33. What Happens During Terraform Apply?

### Interview Answer

1. Read State File
2. Compare Desired State
3. Build Dependency Graph
4. Generate Execution Plan
5. Create/Update/Delete Resources
6. Update State File

---

# 34. Lifecycle Rules

### Interview Answer

```hcl
lifecycle {
 create_before_destroy = true
 prevent_destroy = true
}
```

### create_before_destroy

Creates new resource first.

### prevent_destroy

Prevents accidental deletion.

---

# 35. Terraform Wants to Recreate Everything

### Interview Answer

Possible reasons:

* State File Corruption
* Resource Renamed
* Backend Changed
* Manual Modifications
* Force Replacement Attributes

Always review:

```bash
terraform plan
```

before applying."

---

# 36. Terraform Taint

### Interview Answer

```bash
terraform taint resource-name
```

Marks resource for recreation.

Used when resource becomes unhealthy."

---

# 37. Optimize Terraform Performance

### Interview Answer

* Use Modules
* Reduce Dependencies
* Use Remote State
* Avoid Large Monolithic Configurations
* Run Parallel Operations
* Minimize Data Sources

---

# 38. Pod Autoscaling

### Interview Answer

Use HPA:

```bash
kubectl autoscale deployment app
```

Scaling based on:

* CPU
* Memory
* Custom Metrics

ReplicaSets maintain desired pod count."

---

# 39. What Happens If Node Fails?

### Interview Answer

Kubernetes detects failure.

Controller Manager:

1. Marks node NotReady.
2. Evicts pods.
3. Scheduler places pods on healthy nodes.
4. Service continues if capacity exists."

---

# 40. Secure Kubernetes Cluster

### Interview Answer

* RBAC
* Private Cluster
* Network Policies
* Key Vault Integration
* Pod Security Standards
* Image Scanning
* Ingress WAF
* Audit Logging

---

# 41. EKS vs AKS vs GKE

| EKS             | AKS               | GKE             |
| --------------- | ----------------- | --------------- |
| AWS             | Azure             | Google          |
| Managed K8s     | Managed K8s       | Managed K8s     |
| AWS Integration | Azure Integration | GCP Integration |

Concepts remain the same."

---

# 42. Master-Slave Architecture

### CI/CD

Master:

* Orchestrates builds

Agent:

* Executes builds

### Kubernetes

Control Plane:

* Scheduling
* API Management

Worker Nodes:

* Run Pods

---

# 43. Deploy to AKS

```bash
kubectl apply -f deployment.yaml
```

Verify:

```bash
kubectl rollout status deployment/app
```

---

# 44. Deployment vs StatefulSet vs DaemonSet

| Deployment | StatefulSet   | DaemonSet         |
| ---------- | ------------- | ----------------- |
| Stateless  | Stateful Apps | One Pod Per Node  |
| Web Apps   | Databases     | Monitoring Agents |

---

# 45. Rolling Update & Rollback

Update:

```bash
kubectl set image deployment/app
```

Rollback:

```bash
kubectl rollout undo deployment/app
```

---

# 46. CrashLoopBackOff

Pod starts, crashes, and continuously restarts.

Main causes:

* Bad Configuration
* Missing Secrets
* Application Errors
* DB Connectivity Issues

---

# 47. Traffic Increased But Nodes Not Scaling

Check:

```bash
kubectl get hpa
```

Verify:

* Metrics Server
* Cluster Autoscaler
* Resource Requests
* Scaling Policies

---

# 48. Pods Stuck in Pending

Check:

```bash
kubectl describe pod
```

Common reasons:

* Insufficient Resources
* Node Selector Issue
* Taints/Tolerations
* PVC Issues

---

# 49. Load Balancer Not Getting External IP

Check:

```bash
kubectl describe svc
```

Verify:

* Cloud Controller Manager
* Service Type = LoadBalancer
* Azure Quotas
* Permissions
* Events

This is one of the most common AKS troubleshooting questions in senior DevOps interviews.
