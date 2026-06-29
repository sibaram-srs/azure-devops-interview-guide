## Interview Ready Answers (Architecture + AKS + Terraform + Security)

These questions are asked at a **Senior/Lead DevOps Engineer level**, so answer from an architectural and production perspective.

---

# 3. What is an Azure Landing Zone and Why Is It Important?

### Interview Ready Answer

"An Azure Landing Zone is a pre-configured cloud environment built according to Microsoft's Cloud Adoption Framework. It provides a standardized foundation for deploying workloads securely and consistently.

It typically includes:

* Management Groups
* Subscriptions
* Networking
* RBAC
* Policies
* Security Baselines
* Monitoring
* Identity Integration

The main benefit is that teams can deploy workloads quickly while maintaining governance, security, compliance, and operational standards."

---

# 4. How Do You Design a High Availability Solution?

### Interview Ready Answer

"For a production application, I design HA at multiple layers.

Example:

Users
↓

Azure Front Door

↓

Application Gateway

↓

AKS Cluster (Multiple Node Pools)

↓

Azure SQL Database (Zone Redundant)

↓

Geo Backup

Key principles:

* No single point of failure
* Multi-AZ deployment
* Load balancing
* Auto Scaling
* Health Probes
* Automated Failover

This ensures application availability even if a node, zone, or service fails."

---

# 5. How Do You Implement Security While Designing a Solution?

### Interview Ready Answer

"I follow a security-by-design approach.

Key controls include:

* Least Privilege Access (RBAC)
* Private Endpoints
* NSGs
* WAF
* Key Vault
* Managed Identity
* Encryption at Rest and in Transit
* Network Segmentation
* Security Monitoring
* Vulnerability Scanning

Security requirements are considered from the architecture phase rather than added later."

---

# 6. How Do You Secure Your System?

### Interview Answer

Multiple layers:

### Identity

* Entra ID (Azure AD)
* MFA
* Conditional Access

### Infrastructure

* NSGs
* Private Networking
* Azure Firewall

### Application

* WAF
* API Security

### Secrets

* Azure Key Vault

### Monitoring

* Defender for Cloud
* Sentinel
* Azure Monitor

### Compliance

* Azure Policy
* RBAC

---

# 7. Have You Enabled Managed Identity?

### Interview Ready Answer

"Yes.

Managed Identity is preferred over storing credentials.

Example:

AKS Pod
↓

Managed Identity

↓

Azure Key Vault

↓

Retrieve Secrets

No username, password, or service principal secret is stored in code."

---

# 8. Subscription Cost Increased by 40%

### Interview Ready Answer

First, I use:

* Cost Analysis
* Advisor
* Azure Monitor

Then investigate:

### Compute

* Oversized VMs
* VMSS Scaling

### AKS

* Excess Nodes
* Resource Requests

### Storage

* Snapshots
* Unused Disks

### Networking

* Data Transfer Costs

### Databases

* Overprovisioned Tiers

Finally:

* Right-size resources
* Enable Reservations
* Remove unused assets
* Implement Budgets and Alerts

---

# 9. Strategy for Subscription Coverage/Uptime

### Interview Ready Answer

"We ensure high availability through:

* Multi-AZ Architecture
* Auto Scaling
* Backup Strategy
* Disaster Recovery
* Monitoring
* SLA-based Services
* Geo-redundancy

The goal is to minimize downtime and meet business-defined SLAs."

---

# 10. How Do You Upgrade AKS?

### Interview Answer

Check current version:

```bash
az aks show --resource-group rg --name aks
```

Upgrade:

```bash
az aks upgrade \
--resource-group rg \
--name aks \
--kubernetes-version 1.xx.x
```

Always validate compatibility before upgrading."

---

# 11. AKS Upgrade with Zero Downtime

### Interview Ready Answer

AKS uses rolling upgrades.

Process:

1. New node created.
2. Pods drained from old node.
3. Workloads moved.
4. Old node removed.

Additional controls:

* Multiple Replicas
* Pod Disruption Budget (PDB)
* Readiness Probes
* Rolling Updates

This ensures application availability during upgrades."

---

# 12. How Do You Implement Zero Trust?

### Interview Ready Answer

"Zero Trust means 'Never Trust, Always Verify.'

Implementation:

* MFA
* Conditional Access
* Least Privilege Access
* RBAC
* Managed Identity
* Private Endpoints
* Network Segmentation
* Continuous Monitoring
* Device Compliance Checks

Every request must be authenticated and authorized."

---

# 13. Production Cluster Goes Down – RCA Process

### Interview Ready Answer

My approach:

### Step 1

Restore Service

* Failover
* Scale Recovery
* Rollback

### Step 2

Collect Evidence

* Events
* Logs
* Metrics
* Alerts

### Step 3

Identify Root Cause

Examples:

* Node Failure
* Misconfiguration
* Resource Exhaustion
* Application Bug

### Step 4

Create RCA

Include:

* Timeline
* Impact
* Root Cause
* Resolution
* Preventive Actions

### Step 5

Gap Closure

Examples:

* Better Monitoring
* Additional Alerts
* Capacity Planning
* Runbooks
* DR Testing

---

# 14. Taints and Tolerations

### Interview Ready Answer

Taints prevent pods from being scheduled on specific nodes.

Example:

Dedicated Node for Monitoring.

Taint:

```bash
kubectl taint nodes node1 monitoring=true:NoSchedule
```

Only pods with matching toleration can run there.

Use Cases:

* GPU Nodes
* Monitoring Nodes
* Database Nodes
* Dedicated Workloads

---

# 15. Log File Shared Across Pods

### Interview Ready Answer

Use:

### Persistent Volume (PV)

### Persistent Volume Claim (PVC)

Example:

Azure File Share

Multiple Pods
↓

Shared PVC
↓

Same Log File Access

For centralized logging, however, I would recommend:

* Fluent Bit
* Loki
* Elasticsearch

instead of shared files."

---

# 16. How Do You Monitor Kubernetes?

### Interview Ready Answer

Tools:

* Prometheus
* Grafana
* Azure Monitor
* Container Insights
* Log Analytics

Monitoring covers:

* Cluster Health
* Nodes
* Pods
* Network
* Storage
* Applications

---

# 17. Key Parameters Monitored

### Interview Answer

Infrastructure:

* CPU
* Memory
* Disk
* Network

Kubernetes:

* Pod Restarts
* Pending Pods
* Node Health
* Replica Count
* HPA Metrics

Application:

* Response Time
* Error Rate
* Throughput
* Availability

---

# 18. ConfigMap vs Secret

| ConfigMap          | Secret            |
| ------------------ | ----------------- |
| Non-sensitive Data | Sensitive Data    |
| Plain Text         | Base64 Encoded    |
| URLs, Configs      | Passwords, Tokens |
| Less Secure        | More Secure       |

Example:

ConfigMap:

* Application URL

Secret:

* Database Password

---

# 19. Integrated Vault with AKS?

### Interview Ready Answer

"Yes.

We integrated Azure Key Vault using:

* Managed Identity
* CSI Secrets Store Driver

Flow:

AKS Pod
↓

Managed Identity

↓

Key Vault

↓

Secret Mounted into Pod

This removes the need to store secrets in Kubernetes."

---

# 20. Multi-Environment Terraform Structure

### Interview Answer

```text
terraform/
├── modules/
│   ├── aks
│   ├── vnet
│   └── storage

├── environments/
│   ├── dev
│   ├── test
│   └── prod
```

Each environment uses the same modules with different variable values."

---

# 21. Terraform for AWS and Azure

### Interview Ready Answer

Structure:

```text
modules/
├── azure
├── aws

environments/
├── dev
├── prod
```

Separate providers:

```hcl
provider "azurerm"
provider "aws"
```

Shared module strategy improves maintainability."

---

# 22. Provisioners in Terraform

### Interview Answer

Provisioners execute scripts after resource creation.

Example:

```hcl
provisioner "remote-exec" {
 inline = [
   "sudo apt update"
 ]
}
```

Types:

* local-exec
* remote-exec

However, provisioners should be used sparingly because they are not fully idempotent."

---

# 23. Handling Sensitive Information

### Interview Answer

Best practices:

* Azure Key Vault
* Managed Identity
* Sensitive Variables

Example:

```hcl
sensitive = true
```

Never:

* Hardcode Passwords
* Store Secrets in Git

---

# 24. Modular Project Structure

### Interview Answer

```text
modules/
├── network
├── aks
├── storage

environments/
├── dev
├── qa
├── prod
```

Benefits:

* Reusability
* Standardization
* Easier Maintenance
* Scalability

---

# 25. Terraform Security Scanning

### Interview Answer

Tools:

* tfsec
* Checkov
* Terrascan

Pipeline Flow:

```text
Terraform Code
↓
Security Scan
↓
Plan
↓
Apply
```

Security issues are detected before deployment."

---

# 26. What Does tfsec Capture?

### Interview Ready Answer

Examples:

* Public Storage Accounts
* Open NSG Rules (0.0.0.0/0)
* Missing Encryption
* Missing Logging
* Weak Security Configurations
* Public Databases

It validates Terraform code against security best practices."

---

# 27. Git Fetch vs Pull vs Push

| Command   | Purpose               |
| --------- | --------------------- |
| git fetch | Download changes only |
| git pull  | Fetch + Merge         |
| git push  | Upload changes        |

Example:

```bash
git fetch
```

Downloads changes but does not merge.

```bash
git pull
```

Downloads and merges changes.

```bash
git push
```

Sends local commits to remote repository."

---

# 28. Developer Committed Secrets – How Do You Remove Them?

### Interview Ready Answer

### Immediate Actions

1. Revoke Secret
2. Rotate Credentials

### Remove from Repository

Using:

```bash
git filter-repo
```

or

```bash
BFG Repo-Cleaner
```

### Force Push Clean History

```bash
git push --force
```

### Additional Steps

* Remove from CI/CD Variables
* Check Logs
* Review Artifacts
* Scan Entire Repository

Finally, implement:

* Git Secrets
* Gitleaks
* TruffleHog

to prevent future secret exposure.

---
