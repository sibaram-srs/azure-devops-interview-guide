# WiDIpro L3 DevOps Engineer Interview Answers (Interview Ready)

---

# Introduction

## 1. Tell me about yourself.

"I'm a DevOps Engineer with experience in Azure, AWS, Kubernetes, Docker, Terraform, Azure DevOps, GitHub, Linux, and monitoring tools. I primarily work on automating infrastructure provisioning, building CI/CD pipelines, container orchestration, infrastructure as code, monitoring, and production support. My focus is reducing deployment time, improving reliability, and ensuring secure, highly available infrastructure."

---

## 2. What is your current role?

"I work as a DevOps Engineer responsible for CI/CD automation, Azure infrastructure provisioning using Terraform, Kubernetes deployments, monitoring production environments, troubleshooting incidents, and implementing DevSecOps practices."

---

## 3. What project are you working on?

"I'm working on a cloud-native application hosted on Azure Kubernetes Service (AKS). We use Azure DevOps for CI/CD, Terraform for Infrastructure as Code, Helm for Kubernetes deployments, Azure Monitor for monitoring, and SonarQube/Trivy for security scanning."

---

## 4. Which organization are you working for?

"I'm currently working with **<Your Company Name>** as a DevOps Engineer supporting enterprise cloud infrastructure and CI/CD automation."

---

## 5. Is your project Azure-based?

"Yes. Our infrastructure is hosted on Azure. We use AKS, Azure DevOps, Azure Key Vault, Azure Monitor, Azure Storage, Azure Application Gateway, Azure Container Registry, and Terraform."

---

## 6. Do you have AWS experience?

"Yes. I have worked with EC2, VPC, IAM, S3, ALB, Auto Scaling, CloudWatch, Route53, EKS, Terraform, and CodePipeline. I'm comfortable working in both Azure and AWS environments."

---

# Azure DevOps / CI-CD

## 1. Explain your CI/CD pipeline.

1. Developer pushes code to Git.
2. Build pipeline triggers automatically.
3. Run code quality scan (SonarQube).
4. Execute unit tests.
5. Build Docker image.
6. Scan image using Trivy.
7. Push image to ACR.
8. Publish build artifacts.
9. Release pipeline deploys infrastructure (Terraform if required).
10. Deploy application to AKS using Helm.
11. Run smoke tests.
12. Monitor deployment.

---

## 2. What are the stages in Azure DevOps Pipeline?

- Source
- Build
- Test
- Security Scan
- Artifact Publish
- Infrastructure Provisioning
- Deployment
- Validation
- Monitoring

---

## 3. How does pipeline start after developer pushes code?

"The Git repository webhook triggers Azure DevOps pipeline automatically based on branch triggers defined in YAML."

---

## 4. Difference between Classic Pipeline and YAML Pipeline

| Classic | YAML |
|----------|------|
| GUI based | Code based |
| Less version control | Stored in Git |
| Manual changes | Easily reusable |
| Hard to maintain | Supports CI/CD as Code |

---

## 5. What are Azure DevOps Agents?

"Agents are machines that execute pipeline jobs."

---

## 6. Microsoft-hosted vs Self-hosted Agent

Microsoft Hosted

- Managed by Microsoft
- Fresh VM every run
- Easy maintenance

Self Hosted

- Managed by us
- Faster builds
- Custom software
- Better for private networks

---

## 7. What are Variable Groups?

"Variable Groups store common variables and secrets centrally across multiple pipelines. Secrets are usually integrated with Azure Key Vault."

---

## 8. How do you integrate Terraform with Azure DevOps?

- Checkout code
- Terraform Init
- Terraform Validate
- Terraform Plan
- Manual Approval (Production)
- Terraform Apply

---

## 9. Pipeline fails during deployment. How do you troubleshoot?

- Check pipeline logs
- Verify service connection
- Check Terraform logs
- Verify Kubernetes events
- Validate credentials
- Check permissions
- Review application logs
- Rollback if necessary

---

## 10. Pipeline suddenly becomes slow.

Check:

- Agent utilization
- Network latency
- Build cache
- Artifact size
- Terraform execution
- External API delays
- Recent pipeline changes

---

# Terraform

## 1. What is Terraform State File?

"The Terraform state file stores the mapping between Terraform configuration and actual infrastructure."

---

## 2. Why store state remotely?

- Team collaboration
- State locking
- Backup
- Security
- Avoid conflicts

---

## 3. Terraform workflow

```
Write Code
↓

terraform init

↓

terraform validate

↓

terraform plan

↓

terraform apply

↓

terraform destroy (if required)
```

---

## 4. Manual changes to Terraform-managed resource?

"Terraform detects configuration drift during plan and shows the differences."

---

## 5. Configuration Drift

"When actual infrastructure differs from Terraform state or configuration."

---

## 6. Terraform Apply fails after creating resources.

"Terraform keeps successfully created resources in state. After fixing the issue, rerun Terraform Apply to continue."

---

## 7. Recover deleted Terraform State File

- Restore from remote backend versioning
- Restore from backup
- Use state recovery if available

---

## 8. Command to recover state

```
terraform import
```

(or restore backend backup)

---

## 9. Plan vs Apply

Plan

- Preview changes

Apply

- Executes changes

---

## 10. Terraform Taint

"Terraform taint marks a resource for recreation during the next apply."

---

# Azure Scenarios

## 1. Website slow but Azure Monitor is green.

Check:

- Application logs
- Database latency
- Network latency
- Application Gateway
- Dependency failures
- Slow queries

---

## 2. CPU reaches 100%

- Identify process
- Scale out
- Optimize application
- Restart if needed
- Enable Autoscaling

---

## 3. HTTP 503 after deployment

Check:

- Pod health
- Readiness probes
- Ingress
- Backend service
- Application startup logs

---

## 4. Production DB slow

Check:

- Slow queries
- Locks
- CPU
- Connections
- Storage IOPS
- Index fragmentation

---

## 5. OS slow but CPU and RAM normal

Check:

- Disk IO
- File system
- Network
- Zombie processes
- Kernel logs

---

## 6. Backend inaccessible during deployment

Check:

- Backend storage
- Firewall
- NSG
- DNS
- Service endpoint
- Credentials

---

# Linux

## 1. What is Cron Job?

"Cron is a Linux scheduler used to execute commands automatically at scheduled times."

---

## 2. Cron Job not running for 3 days

Check:

- cron service
- crontab entries
- logs
- permissions
- script execution
- disk space

---

# Docker / Kubernetes

## 1. Deploy application to 500+ servers

"I would use Kubernetes with multiple regional clusters, Helm, ArgoCD GitOps, Load Balancers, CDN, and rolling updates."

---

## 2. CrashLoopBackOff

"A pod repeatedly crashes after starting."

---

## 3. Troubleshoot CrashLoopBackOff

```
kubectl describe pod

kubectl logs

Check events

Check probes

Verify ConfigMap

Verify Secret

Check resource limits

Check image
```

---

## 4. DaemonSet vs Deployment

Deployment

- Fixed number of Pods

DaemonSet

- One Pod per Node

---

## 5. HPA

"HPA automatically scales Pods based on CPU or custom metrics."

---

## 6. Cluster Autoscaler

"Automatically adds or removes worker nodes based on workload."

---

## 7. Blue-Green Deployment

"Two identical environments exist. Switch traffic to the new environment after validation."

---

## 8. Canary Deployment

"Release to a small percentage of users first, monitor, then gradually increase traffic."

---

## 9. Helm Rollback

```
helm rollback <release> <revision>
```

---

## 10. Namespace deleted in Production

- Stop deployments
- Restore from backup
- Restore manifests
- Recover persistent volumes
- Validate workloads

---

## 11. Pods unhealthy after deployment

Check:

- Logs
- Events
- Readiness probe
- Liveness probe
- Image
- Secrets
- ConfigMaps

Rollback if required.

---

## 12. Ingress not working

Check:

- Ingress Controller
- Rules
- Backend Service
- DNS
- TLS
- Load Balancer

---

# Monitoring

## 1. Which monitoring tool have you used?

"I have worked with Azure Monitor, Prometheus, Grafana, Log Analytics, Application Insights, Nagios, and ELK."

---

## 2. Azure Monitor?

"Yes. I use Azure Monitor for metrics, alerts, logs, dashboards, and application monitoring."

---

## 3. Nagios?

"Yes. Mainly for infrastructure and server health monitoring."

---

## 4. Monitor deployments

- Azure Monitor
- Grafana dashboards
- Prometheus
- Application Insights
- Logs
- Alerts
- Smoke Tests

---

# Security

## 1. Secure Azure DevOps

- RBAC
- MFA
- Azure Key Vault
- Branch Policies
- Service Connections
- Least Privilege
- Secret Scanning

---

## 2. Secure Azure

- NSG
- Azure Firewall
- Key Vault
- Defender
- RBAC
- Private Endpoints
- Encryption
- Backup

---

## 3. Security Group vs NACL

| Security Group | NACL |
|---------------|------|
| Instance level | Subnet level |
| Stateful | Stateless |
| Allow rules only | Allow & Deny |
| Default deny inbound | Ordered rules |

---

# Vulnerability / DevSecOps

## 1. Critical vulnerability found

- Block deployment
- Verify severity
- Upgrade package
- Rebuild image
- Re-scan
- Deploy after approval

---

## 2. Handle vulnerabilities during deployment

- SonarQube
- Trivy
- Dependency scan
- Image scan
- Fail pipeline for Critical/High vulnerabilities

---

# Production Scenarios

## 1. Production incident handled

"A deployment caused application failures due to an incorrect ConfigMap. We identified the issue through Kubernetes events and application logs, rolled back using Helm, restored service within minutes, and fixed the configuration before redeployment."

---

## 2. Have you rolled back production?

"Yes."

---

## 3. Reason?

- Application crash
- Configuration issue
- Failed health checks

---

## 4. Troubleshooting steps

- Check logs
- Events
- Metrics
- Rollback
- RCA
- Permanent fix

---

# GitOps / ArgoCD

## 1. Worked on ArgoCD?

"Yes. I have used ArgoCD for GitOps-based Kubernetes deployments, automated synchronization, drift detection, rollback, and multi-cluster management."

---

## 2. Helm chart impacts all clusters

- Stop sync
- Rollback Helm release
- Revert Git commit
- Validate staging
- Deploy after testing

---

# AI

## 1. Have you worked on LangChain?

"Yes, I have basic exposure to LangChain for integrating LLMs with external data sources and building AI workflows. My primary expertise is DevOps, MLOps, containerization, CI/CD, and cloud infrastructure."

---

# AWS

## 1. Manual AWS resource change managed by Terraform

"Terraform detects drift during terraform plan and shows the differences. I would review the change and either update Terraform code or reapply the desired configuration."

---

## 2. Security Groups vs NACL

| Security Group | NACL |
|---------------|------|
| Stateful | Stateless |
| Instance level | Subnet level |
| Allow only | Allow & Deny |
| Evaluates all rules | Evaluates rules in order |
| Recommended for instance security | Recommended for subnet security |
