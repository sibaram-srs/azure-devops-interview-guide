# T DevOps Interview – Expert Answers

---

# 1. Once you design an Azure Landing Zone, what are the components?

An Azure Landing Zone is a standardized cloud environment built using the Cloud Adoption Framework (CAF). Key components include:

- Management Groups
- Azure Subscriptions
- Resource Groups
- Virtual Networks (VNets)
- Subnets
- Network Security Groups (NSGs)
- Azure Firewall
- Application Gateway / Load Balancer
- Azure DNS
- Azure Key Vault
- Azure Policy
- RBAC (Role-Based Access Control)
- Log Analytics Workspace
- Azure Monitor
- Microsoft Defender for Cloud
- Recovery Services Vault
- Storage Accounts
- CI/CD Integration (Terraform/GitHub Actions/Azure DevOps)

---

# 2. After the Landing Zone is designed and created, what is the next step before deployment?

Before deploying applications:

1. Validate Azure Policies.
2. Configure RBAC and IAM.
3. Configure Networking (VNet, Subnets, NSGs).
4. Configure Azure Key Vault for secrets.
5. Configure Monitoring (Azure Monitor, Log Analytics).
6. Configure CI/CD pipeline.
7. Run Terraform Plan.
8. Perform Security Validation.
9. Deploy Infrastructure using Terraform Apply.
10. Deploy the Application.

---

# 3. What is Azure Application Gateway?

Azure Application Gateway is a **Layer 7 (Application Layer) Load Balancer** that routes HTTP/HTTPS traffic based on URL, Host Header, or Path.

Features:
- SSL Termination
- Web Application Firewall (WAF)
- URL-based Routing
- Path-based Routing
- Cookie-based Session Affinity
- End-to-End SSL
- Autoscaling

---

# 4. What are the steps to configure Azure Application Gateway?

1. Create Virtual Network.
2. Create Dedicated Application Gateway Subnet.
3. Create Public IP.
4. Create Backend Pool.
5. Configure Backend Targets (VM/VMSS/AKS/App Service).
6. Configure Health Probe.
7. Configure HTTP Settings.
8. Create Listener (HTTP/HTTPS).
9. Upload SSL Certificate (HTTPS).
10. Configure Routing Rules.
11. Associate WAF Policy (Optional).
12. Test Application Access.

---

# 5. What are the types of Azure Load Balancer?

### Public Load Balancer
- Internet-facing
- Uses Public IP
- External traffic

### Internal Load Balancer
- Private IP only
- Internal communication
- Backend services

---

# 6. What are the types of Azure Storage Account?

- General Purpose v2 (GPv2) **(Recommended)**
- General Purpose v1
- Blob Storage
- BlockBlobStorage (Premium)
- FileStorage (Premium)

Storage Services:
- Blob Storage
- File Share
- Queue Storage
- Table Storage

---

# 7. What is the difference between `terraform plan` and `terraform apply`?

### terraform plan
- Dry run
- Shows changes
- No infrastructure changes

Example:
```bash
terraform plan
```

### terraform apply
- Executes changes
- Creates/Updates infrastructure
- Requires confirmation (unless auto-approved)

Example:
```bash
terraform apply
```

---

# 8. What is `provider.tf`? What is configured in it?

`provider.tf` defines the cloud provider and authentication details.

Example:

```hcl
provider "azurerm" {
  features {}
  subscription_id = "xxxxx"
  tenant_id       = "xxxxx"
}
```

Typically contains:
- Cloud Provider
- Authentication
- Region
- Provider Features

---

# 9. Is Public IP associated at the Subnet level or the VNet level?

Neither.

A Public IP is associated with:
- VM NIC
- Load Balancer
- Application Gateway
- Azure Firewall
- NAT Gateway

It is **not** attached directly to a VNet or Subnet.

---

# 10. What is a branching strategy in DevOps?

A branching strategy defines how code is managed and merged.

Common strategies:
- GitFlow
- GitHub Flow
- Trunk-Based Development

Typical branches:
- main
- develop
- feature/*
- release/*
- hotfix/*

---

# 11. What is the difference between CI and CD?

### CI (Continuous Integration)
- Code build
- Unit tests
- Static code analysis
- Artifact creation

### CD (Continuous Delivery/Deployment)
- Deploy artifacts
- Infrastructure provisioning
- Configuration updates
- Production release

CI validates code.
CD delivers applications.

---

# 12. How do you configure a Subnet in Azure?

1. Create Resource Group.
2. Create Virtual Network.
3. Define Address Space (e.g., 10.0.0.0/16).
4. Create Subnet (e.g., 10.0.1.0/24).
5. Associate NSG.
6. Configure Route Table (if required).
7. Enable Service Endpoints/Private Endpoints (if needed).
8. Deploy VMs into the Subnet.

---

# 13. What is an External Load Balancer?

An External (Public) Load Balancer distributes internet traffic to backend resources.

Use cases:
- Public Websites
- APIs
- Internet-facing Applications

Uses:
- Public IP
- Layer 4 (TCP/UDP)

---

# 14. What is an Internal Load Balancer? When do you use it?

An Internal Load Balancer distributes traffic within a private network.

Use cases:
- Database Tier
- Internal APIs
- Backend Services
- Microservices

Uses:
- Private IP
- No Internet Access

---

# 15. If you need to modify an application running inside a VM, what precautions should you take?

- Take VM Backup/Snapshot.
- Verify Deployment Window.
- Check Current Version.
- Stop Incoming Traffic (if required).
- Take Configuration Backup.
- Validate Changes in Test Environment.
- Deploy using CI/CD.
- Perform Smoke Testing.
- Monitor Logs.
- Have a Rollback Plan.

---

# 16. If a VM cannot communicate with the Internet, what could be the possible reasons?

Check:

- NSG Rules
- Route Table (UDR)
- Public IP Association
- Azure Firewall
- Proxy Configuration
- DNS Resolution
- NIC Configuration
- Default Route
- VM OS Firewall
- Internet Connectivity
- Load Balancer Rules
- NAT Gateway

---

# 17. Scenario-based Docker questions

### Docker container exits immediately.

Possible reasons:
- Main process finished
- Wrong ENTRYPOINT/CMD
- Application crash
- Missing dependencies

Commands:

```bash
docker logs <container>

docker inspect <container>

docker exec -it <container> bash
```

---

### Docker image is too large.

Solutions:
- Multi-stage builds
- Alpine image
- Remove unnecessary packages
- Clean cache
- Optimize layers

---

### Container cannot reach another container.

Check:
- Docker Network
- Container DNS
- Port Mapping
- Firewall

---

# 18. Scenario-based Kubernetes questions

### Pod is Pending.

Check:
- Insufficient CPU/Memory
- Node Selector
- Taints/Tolerations
- PVC Issues

Command:

```bash
kubectl describe pod
```

---

### Pod CrashLoopBackOff.

Check:
- Application logs
- Wrong command
- Missing ConfigMap
- Missing Secret

Commands:

```bash
kubectl logs

kubectl describe pod
```

---

### Service not accessible.

Check:
- Service Type
- Selector
- Endpoints
- Ingress
- Network Policies

---

# 19. CI/CD pipeline hands-on and scenario-based questions

Pipeline fails after code merge.

Approach:

- Check Pipeline Logs.
- Verify Build.
- Verify Unit Tests.
- Verify SonarQube.
- Verify Docker Build.
- Verify Terraform.
- Verify Deployment.
- Rollback if required.

---

# 20. DevOps hands-on implementation questions

Daily tasks:

- Create Terraform Modules.
- Build GitHub Actions/Jenkins Pipelines.
- Deploy Docker Containers.
- Manage AKS.
- Configure Monitoring.
- Troubleshoot Deployments.
- Security Scanning.
- Infrastructure Automation.

---

# 21. Day-to-day DevOps project implementation questions

- Code Review
- Terraform Deployment
- AKS Deployment
- CI/CD Maintenance
- Secrets Management
- Image Scanning
- Monitoring
- Incident Resolution
- Pipeline Optimization

---

# 22. Troubleshooting scenarios related to CI/CD pipelines

Common failures:

- Git Authentication
- Build Failure
- Unit Test Failure
- SonarQube Quality Gate Failure
- Docker Build Failure
- Terraform Error
- Deployment Failure
- Permission Issues

Approach:
1. Review logs.
2. Identify failed stage.
3. Validate configuration.
4. Fix issue.
5. Rerun pipeline.

---

# 23. Troubleshooting scenarios related to Docker and Kubernetes

Docker:
- Image build issues
- Registry authentication
- Volume mounts
- Network issues

Kubernetes:
- Pod Pending
- CrashLoopBackOff
- ImagePullBackOff
- Failed Scheduling
- Service unavailable
- Ingress issues

Useful commands:

```bash
kubectl get pods

kubectl describe pod

kubectl logs

kubectl get events

docker logs

docker inspect
```

---

# 24. Azure networking scenario-based questions

### VM cannot communicate with another VM.

Check:
- Same VNet or VNet Peering
- NSG Rules
- Route Tables
- Firewall
- NIC Configuration
- DNS

---

### Application not accessible from Internet.

Check:
- Public IP
- NSG
- Load Balancer
- Application Gateway
- DNS
- Health Probe
- Backend Pool

---

### AKS cannot pull images from ACR.

Check:
- ACR Permissions
- Managed Identity
- Image Name
- Network Connectivity
- DNS
- Authentication

---

# 25. Terraform deployment and infrastructure troubleshooting scenarios

### Terraform Apply fails.

Check:
- Syntax Errors
- Authentication
- State File
- Resource Lock
- Provider Version
- Dependency Issues
- Existing Resources

Commands:

```bash
terraform fmt

terraform validate

terraform plan

terraform apply

terraform state list

terraform refresh
```

---

# 26. Interview Tip (What Interviewers Want to Hear)

For every scenario-based question, answer in this order:

1. **Identify the issue** (logs, metrics, symptoms)
2. **Verify configuration** (network, IAM, code, pipeline)
3. **Check logs and monitoring** (Azure Monitor, Kubernetes, Jenkins, GitHub Actions)
4. **Implement the fix**
5. **Validate the solution**
6. **Document the root cause and preventive actions**

This structured troubleshooting approach demonstrates real-world DevOps experience and is highly valued in interviews.
