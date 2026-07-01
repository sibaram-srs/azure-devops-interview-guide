# Senior DevOps Engineer
---

# 1. Tell Me About Yourself

### Interview Ready Answer

"Hi, my name is SR. I have around 8 years of experience in Cloud and DevOps Engineering with expertise in Azure Cloud, Azure DevOps, Terraform, Kubernetes (AKS), Docker, CI/CD, Infrastructure as Code, Monitoring, Security, and Production Support.

Currently, I work as a Senior DevOps Engineer supporting enterprise-scale microservices applications hosted on AKS. My responsibilities include infrastructure provisioning through Terraform, CI/CD automation using Azure DevOps, container orchestration, cloud security implementation, monitoring, incident management, troubleshooting production issues, and ensuring high availability and reliability of applications.

I work closely with development, QA, security, and architecture teams to deliver secure and scalable cloud solutions."

---

# 2. Current Roles & Responsibilities

### Interview Ready Answer

"My day-to-day responsibilities include:

* Azure Infrastructure Provisioning using Terraform
* AKS Administration and Support
* CI/CD Pipeline Design and Maintenance
* Production Deployments
* Monitoring and Alerting
* Security Integration
* Incident and RCA Management
* Cost Optimization
* Troubleshooting Application and Infrastructure Issues
* Supporting Disaster Recovery and High Availability Strategies

My primary objective is to automate processes and ensure platform stability."

---

# 3. Have You Worked with AI Tools in DevOps?

### Interview Ready Answer

"Yes.

I use AI tools to improve productivity and operational efficiency.

Examples:

* Generating Terraform modules and scripts
* Creating Kubernetes manifests
* Troubleshooting errors
* Log analysis
* Pipeline optimization
* Documentation generation
* Root cause investigation support

However, all generated outputs are reviewed and validated before production implementation."

---

# 4. Explain Kubernetes Architecture

### Interview Ready Answer

"Kubernetes follows a control plane and worker node architecture.

### Control Plane Components

* API Server
* Scheduler
* Controller Manager
* ETCD

### Worker Node Components

* Kubelet
* Container Runtime
* Kube Proxy

Architecture:

```text
Users
  ↓
API Server
  ↓
Control Plane
  ↓
Worker Nodes
  ↓
Pods
  ↓
Containers
```

The control plane manages the cluster state while worker nodes run application workloads."

---

# 5. Main Kubernetes Components & Objects

### Interview Ready Answer

Core Objects:

### Workload Objects

* Pod
* Deployment
* ReplicaSet
* StatefulSet
* DaemonSet
* Job
* CronJob

### Networking

* Service
* Ingress
* Network Policies

### Configuration

* ConfigMap
* Secret

### Storage

* PV
* PVC
* StorageClass

### Security

* RBAC
* Service Accounts

### Scaling

* HPA
* Cluster Autoscaler

---

# 6. ReplicaSet vs StatefulSet

### Interview Ready Answer

| ReplicaSet             | StatefulSet                 |
| ---------------------- | --------------------------- |
| Stateless Applications | Stateful Applications       |
| Pods are identical     | Pods have unique identities |
| No persistent identity | Stable hostname and storage |
| Web Applications       | Databases                   |

### Example

ReplicaSet:

```text
Frontend Application
```

StatefulSet:

```text
MongoDB
MySQL
PostgreSQL
```

---

# 7. Command to Increase Number of Pods

### Interview Ready Answer

Manual scaling:

```bash
kubectl scale deployment nginx --replicas=5
```

Verify:

```bash
kubectl get pods
```

In production, we usually prefer HPA for automatic scaling.

---

# 8. Different Ways to Check Logs

### Interview Ready Answer

### Pod Logs

```bash
kubectl logs pod-name
```

### Previous Container Logs

```bash
kubectl logs pod-name --previous
```

### Multi-Container Pods

```bash
kubectl logs pod-name -c container-name
```

### Follow Logs

```bash
kubectl logs -f pod-name
```

### Enterprise Monitoring

* Azure Monitor
* Log Analytics
* Grafana
* Prometheus

---

# 9. Deploy and Configure Secrets

### Interview Ready Answer

Create Secret:

```bash
kubectl create secret generic db-secret \
--from-literal=password=Password123
```

Reference in Deployment:

```yaml
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

Production Best Practice:

* Azure Key Vault
* CSI Secret Store Driver

Avoid storing secrets directly in YAML.

---

# 10. CMD vs ENTRYPOINT

### Interview Ready Answer

| CMD               | ENTRYPOINT            |
| ----------------- | --------------------- |
| Default Command   | Fixed Command         |
| Can Be Overridden | Difficult to Override |
| Optional          | Usually Mandatory     |

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Execution:

```bash
python app.py
```

---

# 11. How to Reduce Docker Image Size?

### Interview Ready Answer

Best practices:

### Use Lightweight Base Images

```dockerfile
FROM alpine
```

### Multi-Stage Builds

```dockerfile
Build Stage
↓
Runtime Stage
```

### Remove Unnecessary Packages

### Combine Layers

### Use .dockerignore

### Avoid Development Tools in Final Image

This significantly reduces image size and improves deployment speed.

---

# 12. Azure Services Worked On

### Interview Ready Answer

* AKS
* Azure DevOps
* ACR
* Key Vault
* VNET
* NSG
* Route Tables
* Azure Firewall
* Application Gateway
* Azure Front Door
* Load Balancer
* Azure SQL
* Storage Account
* Managed Identity
* Azure Monitor
* Log Analytics
* VMSS

---

# 13. Types of Managed Identities

### Interview Ready Answer

### System Assigned Managed Identity

* Tied to one Azure resource
* Deleted automatically with resource

### User Assigned Managed Identity

* Independent resource
* Can be shared across multiple resources

### Best Interview Line

"System Assigned is one-to-one, while User Assigned is reusable across multiple resources."

---

# 14. Private Endpoint vs Service Endpoint

### Interview Ready Answer

| Private Endpoint        | Service Endpoint            |
| ----------------------- | --------------------------- |
| Private IP Assigned     | Uses Public Endpoint        |
| Traffic Remains Private | Traffic Uses Azure Backbone |
| More Secure             | Less Secure                 |
| Preferred Design        | Legacy Approach             |

### Example

Private Endpoint:

```text
AKS → SQL Database (Private IP)
```

---

# 15. Bicep vs ARM Templates

### Interview Ready Answer

| Bicep              | ARM               |
| ------------------ | ----------------- |
| Simpler Syntax     | JSON Based        |
| Easier Readability | Verbose           |
| Modern Approach    | Legacy Approach   |
| Compiles to ARM    | Native ARM Format |

### Interview Statement

"Bicep is Microsoft's modern abstraction layer over ARM templates."

---

# 16. Where Do You Store Artifacts?

### Interview Ready Answer

Application Artifacts:

* Azure Artifacts
* Nexus
* JFrog Artifactory

Container Images:

* Azure Container Registry (ACR)

For AKS deployments, images are stored in ACR.

---

# 17. How Do You Configure Azure Front Door?

### Interview Ready Answer

Steps:

1. Create Front Door Profile
2. Create Front-End Endpoint
3. Configure Backend Pool
4. Configure Health Probes
5. Configure Routing Rules
6. Enable WAF
7. Associate Custom Domain

Architecture:

```text
Users
↓
Azure Front Door
↓
Application Gateway
↓
AKS
```

---

# 18. SQL Database Private Endpoint Not Working

### Interview Ready Answer

I follow this approach:

### Verify Private Endpoint Status

```text
Approved
Connected
```

### Verify DNS Resolution

```bash
nslookup sqlserver.database.windows.net
```

Should resolve to private IP.

### Verify NSG Rules

### Verify Route Tables

### Verify Firewall Rules

### Verify VNET Connectivity

### Test Connectivity

```bash
telnet <private-ip> 1433
```

Most common issues are DNS misconfiguration or NSG blocking traffic.

---

# 19. YAML and Classic Pipelines?

### Interview Ready Answer

"Yes, I have worked with both.

However, most enterprise projects now use YAML pipelines because:

* Version Controlled
* Reusable
* Easier Maintenance
* Supports Infrastructure as Code

Classic pipelines are mainly used in older projects."

---

# 20. Deployment Stage Fails After Successful Build

### Interview Ready Answer

My troubleshooting approach:

### Check Pipeline Logs

### Validate Service Connection

### Verify Deployment Credentials

### Verify AKS Connectivity

```bash
kubectl cluster-info
```

### Verify Kubernetes Manifests

### Verify Image Exists in ACR

### Verify Namespace and Permissions

### Check Deployment Events

```bash
kubectl describe deployment app
```

Root cause is usually one of:

* Incorrect Image Tag
* Authentication Failure
* Missing Secrets
* Invalid YAML

---

# 21. Production Deployment Experience?

### Interview Ready Answer

"Yes.

I regularly handle production deployments for critical applications.

Activities include:

* Release Planning
* Change Validation
* Deployment Execution
* Monitoring
* Rollback Planning
* Post-Deployment Verification

All deployments follow change management and approval processes."

---

# 22. Production Challenges Faced?

### Interview Ready Answer (Strong Answer)

"One major issue occurred during a production deployment where new pods continuously entered a CrashLoopBackOff state.

### Investigation

```bash
kubectl logs
kubectl describe pod
```

revealed a missing environment variable from Key Vault.

### Resolution

* Updated the secret configuration
* Redeployed the application
* Verified pod health
* Restored service

### Prevention

* Added pre-deployment validation checks
* Implemented automated secret verification in the pipeline

This helped prevent similar failures in future releases."

---
