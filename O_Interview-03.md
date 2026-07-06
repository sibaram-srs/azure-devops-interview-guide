# DevOps / DevSecOps Interview Questions & Expert Answers (Azure + Terraform + Kubernetes)

---

# 1. A developer wants to bypass a security scan. As a DevSecOps Engineer, how would you prevent this?

As a DevSecOps Engineer, I would ensure that security scans are **mandatory** and cannot be bypassed.

## Approach

- Configure security scans as mandatory pipeline stages.
- Enforce branch protection rules so code can only be merged after successful security checks.
- Configure Quality Gates (SonarQube) to fail the build if vulnerabilities exceed the defined threshold.
- Make SAST, dependency scanning, IaC scanning, and container scanning blocking stages.
- Restrict permissions so developers cannot modify pipeline security stages without approval.
- Use Pull Request policies requiring security validation before merge.
- Educate developers on secure coding practices to reduce repeated issues.

### Example Pipeline

```
Developer Commit
        ↓
Build
        ↓
SAST (SonarQube)
        ↓
Dependency Scan (Snyk)
        ↓
Terraform Scan (tfsec/Checkov)
        ↓
Docker Build
        ↓
Container Scan (Trivy)
        ↓
Quality Gate
        ↓
Deployment
```

If any mandatory security stage fails, deployment is automatically blocked.

---

# 2. What were the most common issues you handled in your project?

Some of the most common issues included:

## Infrastructure Issues
- Terraform state locking conflicts
- Resource drift
- Failed deployments
- Azure quota limitations
- Network connectivity issues

## Kubernetes Issues
- CrashLoopBackOff
- ImagePullBackOff
- Pending Pods
- PVC mounting failures
- Ingress routing issues
- HPA scaling problems

## CI/CD Issues
- Failed builds
- Pipeline permission issues
- Artifact publishing failures
- Service Connection authentication issues

## Security Issues
- Expired secrets
- Expired certificates
- Critical vulnerabilities reported by SonarQube
- Container image vulnerabilities
- Hardcoded secrets detected by GitLeaks

## Monitoring Issues
- High CPU/Memory utilization
- Application latency
- Database connection exhaustion
- Disk space alerts

---

# 3. Walk me through a P1/P2 incident you handled.

## P1 Incident Example

**Issue:**
Production application became unavailable.

### Investigation

- Received alerts from New Relic and Azure Monitor.
- Checked Azure Front Door health probes.
- Verified Application Gateway backend health.
- Checked AKS pod status.
- Observed CrashLoopBackOff due to database connection timeout.

### Resolution

- Increased database connection pool.
- Restarted affected pods.
- Scaled replicas from 3 to 6.
- Monitored application health.
- Confirmed recovery.

### Root Cause

A sudden traffic spike exhausted the database connection pool.

### Preventive Actions

- Tuned connection pool settings.
- Configured Horizontal Pod Autoscaler (HPA).
- Added proactive alerts for connection pool utilization.
- Performed load testing before future releases.

---

# 4. What strategies do you use for environment-specific variables and secrets?

## Variables

Store environment-specific values in separate variable files.

```
terraform.tfvars

dev.tfvars
uat.tfvars
preprod.tfvars
prod.tfvars
```

Example values:

- Resource names
- VM sizes
- CIDR ranges
- Replica counts

## Secrets

Never store secrets in Git.

Secrets are stored in:

- Azure Key Vault

Applications retrieve secrets using Managed Identity.

---

# 5. What versioning strategy do you use in your project?

## Git Versioning

- Feature Branches
- Pull Requests
- Main Branch
- Release Branches
- Hotfix Branches

## Application Versioning

Semantic Versioning (SemVer)

```
Major.Minor.Patch

2.4.1
```

Example:

- Major → Breaking changes
- Minor → New features
- Patch → Bug fixes

## Docker Images

```
app:2.4.1

app:latest
```

## Terraform Modules

```
v1.0.0
v1.1.0
v2.0.0
```

---

# 6. Which monitoring tools did you use? How did you integrate OpenCost for cost optimization?

## Monitoring Tools

- New Relic
- Azure Monitor
- Log Analytics
- Prometheus
- Grafana

## OpenCost Integration

```
AKS
   ↓
Prometheus
   ↓
OpenCost
   ↓
Grafana Dashboard
```

OpenCost helped identify:

- Idle workloads
- Overprovisioned CPU
- Excessive memory requests
- Namespace-wise costs
- Pod-wise costs
- Node utilization

This enabled right-sizing and reduced AKS costs.

---

# 7. How do you design Disaster Recovery (DR) for a multi-tier application?

Typical architecture:

```
Users
      ↓
Azure Front Door
      ↓
Application Gateway
      ↓
AKS
      ↓
Azure SQL
      ↓
Storage Account
```

## DR Strategy

- Deploy resources in two Azure regions.
- Enable Azure SQL Geo-Replication.
- Use Geo-Redundant Storage (GRS).
- Store Terraform state in a replicated Storage Account.
- Back up AKS workloads using Velero.
- Replicate container images to secondary Azure Container Registry (ACR).
- Configure Azure Front Door for automatic failover.

Recovery sequence:

1. Fail traffic to secondary region.
2. Restore databases if required.
3. Deploy infrastructure using Terraform.
4. Deploy applications.
5. Validate health checks.

---

# 8. Explain the current CI/CD pipeline in your project.

```
Developer Commit
       ↓
Pull Request
       ↓
Build
       ↓
Unit Tests
       ↓
SonarQube (SAST)
       ↓
Dependency Scan
       ↓
Terraform Validation
       ↓
tfsec / Checkov
       ↓
Docker Build
       ↓
Trivy Scan
       ↓
Push Image to ACR
       ↓
Deploy to AKS
       ↓
Smoke Tests
       ↓
OWASP ZAP (DAST)
       ↓
Approval
       ↓
Production Deployment
```

---

# 9. Explain reusable Terraform modules. How do you handle versioning and sharing across environments?

## Repository Structure

```
terraform/

modules/
    vnet/
    aks/
    sql/
    keyvault/

environments/
    dev/
    uat/
    preprod/
    prod/
```

## Versioning

Modules are tagged using Git.

Example:

```
v1.0.0
v1.1.0
v2.0.0
```

Environment configuration references a specific module version.

Example:

```terraform
source = "git::https://repo/modules/vnet?ref=v1.2.0"
```

Benefits:

- Consistency
- Controlled upgrades
- Easy rollback

---

# 10. How do you isolate Dev, Staging, and Production environments in Terraform?

Each environment has:

- Separate backend state
- Separate Resource Groups
- Separate subscriptions (preferred)
- Separate Key Vaults
- Separate networking
- Separate variable files

Example:

```
dev/
uat/
prod/
```

Each environment maintains its own Terraform state.

---

# 11. What are the risks of using a shared Terraform state for Production?

Risks include:

- Accidental modification of production resources.
- State corruption affecting multiple environments.
- Permission management becomes difficult.
- Reduced auditability.
- Higher chance of human error.
- Resource dependency conflicts.

Best practice is to maintain **separate Terraform state files and backends** for each environment.

---

# 12. How do you handle secrets in Terraform and ensure they are not exposed?

Best practices:

- Store secrets in Azure Key Vault.
- Retrieve secrets using `data` sources.
- Mark variables as `sensitive = true`.
- Never hardcode secrets.
- Exclude `.tfvars` containing secrets from Git.
- Encrypt Terraform state.
- Restrict access using RBAC.
- Use Managed Identity where possible.

---

# 13. Do you have experience with Terratest?

Yes.

Terratest is used for infrastructure testing.

Typical validations include:

- Resource creation
- Network configuration
- VM provisioning
- AKS deployment
- Storage Account validation
- Output verification

Terratest is integrated into the CI pipeline after `terraform apply` in test environments.

Benefits:

- Automated infrastructure validation
- Early defect detection
- Increased deployment confidence

---

# 14. Your resume mentions a Zero Public Architecture. How did you achieve it?

Implemented a private-by-default architecture.

Measures included:

- Private Endpoints for PaaS services.
- Disabled Public Network Access.
- Azure Bastion for secure VM access.
- Private AKS cluster.
- Private Azure SQL.
- Private Storage Account.
- Private Azure Container Registry.
- Azure Firewall.
- NSGs.
- Hub-and-Spoke networking.
- VPN/ExpressRoute connectivity.

No production resources were directly accessible from the internet.

---

# 15. How do you secure communication to Azure App Service?

Security measures:

- HTTPS only.
- TLS 1.2 or higher.
- Private Endpoint.
- Azure Front Door with WAF.
- Managed Identity for backend access.
- Azure Key Vault integration.
- IP Restrictions.
- Authentication using Microsoft Entra ID (Azure AD).
- Client certificates where required.

---

# 16. Difference between Azure Application Gateway and Azure Front Door

| Azure Front Door | Azure Application Gateway |
|------------------|---------------------------|
| Global service | Regional service |
| Layer 7 load balancer | Layer 7 load balancer |
| Global traffic routing | Regional traffic routing |
| Multi-region failover | Single-region backend routing |
| CDN integration | No CDN |
| Global WAF | Regional WAF |
| Internet-facing | Typically within a region |

Typical architecture:

```
Internet
      ↓
Azure Front Door
      ↓
Application Gateway
      ↓
AKS / App Service
```

---

# 17. Explain your branching strategy.

We followed **GitFlow**.

```
main
│
├── develop
│
├── feature/*
│
├── release/*
│
└── hotfix/*
```

Workflow:

- Developers create feature branches.
- Pull Requests require reviews.
- CI pipeline runs automatically.
- Merge into `develop`.
- Release branches are created for production.
- Hotfix branches handle production issues.

---

# 18. Have you used Azure Managed Identities in your pipeline?

Yes.

Managed Identity eliminates the need to store credentials.

Common use cases:

- Access Azure Key Vault.
- Access Azure Storage.
- Authenticate to Azure SQL.
- Pull images from Azure Container Registry.
- Access Azure Monitor.

Benefits:

- No credential management.
- Automatic secret rotation.
- Improved security.

---

# 19. Have you worked on Azure Landing Zone? What are the core governance policies?

Yes.

Core components include:

- Management Groups
- Azure Policy
- RBAC
- Subscription strategy
- Resource naming standards
- Mandatory tagging
- Cost management
- Logging and monitoring
- Network segmentation
- Security baselines
- Defender for Cloud
- Backup policies

Landing Zones establish a secure and standardized Azure environment.

---

# 20. Which CI/CD tool did you use?

Primary CI/CD tool:

- Azure DevOps Pipelines

Other tools used:

- GitHub Actions (POCs)
- Jenkins (Legacy projects)

Azure DevOps handled:

- CI/CD
- Release management
- Approvals
- Artifact management
- Environment promotion

---

# 21. Your resume mentions reducing the release process from 4 days to 4 hours. How did you achieve this?

The reduction was achieved through automation.

Key improvements:

- Infrastructure as Code using Terraform.
- Reusable Terraform modules.
- YAML-based Azure DevOps pipelines.
- Automated testing.
- SonarQube Quality Gates.
- Security scanning.
- Automated Docker image creation.
- Automated AKS deployment.
- Parallel pipeline execution.
- Automated approvals for non-production.
- Blue-Green and Rolling deployments.
- Standardized deployment templates.

Results:

- Release time reduced from **4 days to approximately 4 hours**.
- Fewer manual errors.
- Faster rollback capability.
- Consistent deployments across environments.

---

# 22. How would you integrate tools like tfsec, TFLint, Terratest, and OpenCost into a CI/CD pipeline?

A typical DevSecOps pipeline would look like this:

```
Developer Commit
        ↓
Pull Request
        ↓
Terraform Format (terraform fmt)
        ↓
Terraform Validate
        ↓
TFLint
        ↓
tfsec / Checkov
        ↓
Terraform Plan
        ↓
Manual Approval
        ↓
Terraform Apply (Non-Production)
        ↓
Terratest
        ↓
Docker Build
        ↓
Trivy Scan
        ↓
Deploy to AKS
        ↓
Smoke Test
        ↓
OWASP ZAP (DAST)
        ↓
Production Approval
        ↓
Production Deployment
        ↓
Prometheus + OpenCost Monitoring
        ↓
Grafana Cost Dashboard
```

### Role of Each Tool

| Tool | Purpose |
|------|---------|
| `terraform fmt` | Formats Terraform code |
| `terraform validate` | Validates Terraform syntax |
| `TFLint` | Detects Terraform best practice issues |
| `tfsec` / `Checkov` | Scans Infrastructure as Code for security vulnerabilities |
| `Terratest` | Tests deployed infrastructure automatically |
| `Trivy` | Scans container images for vulnerabilities |
| `SonarQube` | Static Application Security Testing (SAST) |
| `OWASP ZAP` | Dynamic Application Security Testing (DAST) |
| `OpenCost` | Monitors and optimizes Kubernetes resource costs |
| `Prometheus + Grafana` | Monitoring, metrics, dashboards, and alerts |

This pipeline ensures code quality, security, infrastructure validation, automated testing, controlled deployments, and continuous cost optimization.
