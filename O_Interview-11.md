## Dt. 03-07-26 #DI #DSRC

# DevOps & Azure Interview Questions with Interview-Ready Answers

---

# 1. What is Azure Load Balancer?

Azure Load Balancer is a Layer 4 (TCP/UDP) load balancing service that distributes incoming network traffic across multiple backend servers or VMs to provide high availability and fault tolerance.

### Types
- Public Load Balancer
- Internal Load Balancer

**Use Case:** Balancing traffic across Virtual Machines.

---

# 2. Difference Between Azure Application Gateway and Azure Load Balancer

| Azure Load Balancer | Azure Application Gateway |
|----------------------|--------------------------|
| Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| Network traffic | Web traffic |
| No SSL termination | Supports SSL termination |
| No URL routing | Supports URL path routing |
| No WAF | Supports Web Application Firewall (WAF) |
| Used for VMs | Used for Web Applications |

**Interview Line:**
Load Balancer works at Layer 4, whereas Application Gateway works at Layer 7 with advanced web routing features.

---

# 3. Difference Between VNet Peering and VPN Gateway

| VNet Peering | VPN Gateway |
|--------------|------------|
| Connects Azure VNets | Connects Azure to Azure or On-Premises |
| Uses Azure backbone network | Uses encrypted VPN tunnel |
| Low latency | Slightly higher latency |
| No gateway required | VPN Gateway required |
| High bandwidth | Lower than peering |

**Interview Line:**
VNet Peering is preferred for Azure-to-Azure communication, while VPN Gateway is used for hybrid connectivity.

---

# 4. How Can We Set Private Subnet?

- Create a subnet without a Public IP.
- Deploy VMs without Public IP addresses.
- Use NSG rules to block internet access.
- Access VMs using Azure Bastion or VPN.
- Configure private endpoints if required.

---

# 5. How Can We Configure Public Subnet?

- Create a subnet.
- Assign Public IP to VM/NIC.
- Allow required ports in NSG.
- Configure Route Table if necessary.

---

# 6. SSH Port Number

Default SSH Port:

```
22
```

---

# 7. HTTPS and HTTP Port Numbers

| Protocol | Port |
|----------|------|
| HTTP | 80 |
| HTTPS | 443 |

---

# 8. Apache vs Nginx

| Apache | Nginx |
|---------|-------|
| Process-based | Event-driven |
| Better for dynamic content | Better for static content |
| Higher memory usage | Lower memory usage |
| .htaccess supported | No .htaccess |
| Moderate performance | High performance |

**Interview Line:**
Apache is process-based, while Nginx is event-driven and handles high concurrent traffic more efficiently.

---

# 9. VM Deployment - How Do You Perform?

Steps:

1. Create Resource Group
2. Create Virtual Network
3. Create Subnet
4. Configure NSG
5. Create Public IP (if required)
6. Create Virtual Machine
7. Configure SSH/RDP
8. Install required software
9. Verify connectivity

---

# 10. Linux Command to Check Memory Usage

```bash
free -h
```

Other commands:

```bash
top
```

```bash
htop
```

```bash
vmstat
```

---

# 11. What is State Locking in Terraform?

State Locking prevents multiple users from modifying the Terraform state file simultaneously.

It avoids:
- State corruption
- Concurrent deployments
- Resource conflicts

Azure Storage uses blob lease for state locking.

---

# 12. Difference Between Count and For_Each in Terraform

| Count | for_each |
|--------|----------|
| Uses index | Uses key/value |
| Suitable for identical resources | Suitable for unique resources |
| Numeric indexing | Named resources |
| Less flexible | More flexible |

Example:

```hcl
count = 3
```

```hcl
for_each = {
dev = "EastUS"
prod = "CentralIndia"
}
```

---

# 13. Difference Between Resource and Data Source

| Resource | Data Source |
|-----------|------------|
| Creates infrastructure | Reads existing infrastructure |
| Managed by Terraform | Not managed by Terraform |

Example Resource:

```hcl
resource "azurerm_resource_group" "rg" {}
```

Example Data Source:

```hcl
data "azurerm_resource_group" "existing" {}
```

---

# 14. Explain Docker Layers

Each Docker instruction creates a layer.

Example:

```Dockerfile
FROM ubuntu
RUN apt update
RUN apt install nginx
COPY . /app
CMD ["nginx","-g","daemon off;"]
```

Layers:

- Base Image
- Package Installation
- Software Installation
- Copy Files
- Startup Command

Benefits:
- Faster builds
- Layer caching
- Reduced image size

---

# 15. Difference Between CMD and ENTRYPOINT

| CMD | ENTRYPOINT |
|------|------------|
| Default command | Fixed executable |
| Can be overridden | Hard to override |
| Flexible | Mandatory execution |

Example:

CMD

```dockerfile
CMD ["nginx"]
```

ENTRYPOINT

```dockerfile
ENTRYPOINT ["python"]
```

---

# 16. What is Docker Networking?

Docker Networking enables communication:

- Container to Container
- Container to Host
- Container to Internet

Network Types:

- Bridge
- Host
- None
- Overlay
- Macvlan

Default:

```
Bridge
```

---

# 17. How Do You Optimize Docker Images?

- Use Alpine images
- Multi-stage builds
- Remove unnecessary packages
- Combine RUN commands
- Use .dockerignore
- Minimize image layers
- Use specific image versions

---

# 18. How Do You Deploy an Application into AKS?

Steps:

1. Build Docker Image
2. Push Image to ACR
3. Create Deployment YAML
4. Create Service YAML
5. Deploy using kubectl
6. Verify Pods
7. Expose using LoadBalancer or Ingress

Commands:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
kubectl get svc
```

---

# 19. How Do You Restart Kubernetes Pods?

Restart Deployment:

```bash
kubectl rollout restart deployment <deployment-name>
```

Delete Pod:

```bash
kubectl delete pod <pod-name>
```

---

# 20. Difference Between Permission and Contributor Permission (Azure RBAC)

| Reader | Contributor |
|---------|-------------|
| View resources only | Create, Update, Delete resources |
| Cannot modify | Cannot assign roles |
| Read-only | Full resource management |

Owner has all Contributor permissions plus Role Assignment capability.

---

# 21. How Can You Give Least Privilege Access to Developers to Stop VMs?

Create a **Custom RBAC Role** with only:

- Microsoft.Compute/virtualMachines/start/action
- Microsoft.Compute/virtualMachines/deallocate/action
- Microsoft.Compute/virtualMachines/read

Assign the custom role only to the required Resource Group or VM.

---

# 22. Explain Azure Backup and Recovery for Virtual Machines

Azure Backup provides agentless VM backup.

Features:

- Scheduled Backup
- Incremental Backup
- Recovery Points
- File-level Restore
- Full VM Restore
- Cross-region Restore

Recovery options:

- Restore Entire VM
- Restore Disk
- Restore Individual Files

---

# 23. What Storage Options Have You Used for Azure Virtual Machines?

- Managed Disk
- Premium SSD
- Standard SSD
- Standard HDD
- Ultra Disk (High Performance)

---

# 24. Explain Azure Blob Storage Lifecycle Management

Lifecycle Management automatically moves or deletes blobs.

Rules include:

- Hot → Cool
- Cool → Archive
- Archive → Delete

Benefits:

- Cost Optimization
- Automated Data Management
- Compliance

---

# 25. What is Azure App Service?

Azure App Service is a fully managed Platform as a Service (PaaS) for hosting web applications, REST APIs, and mobile backends.

Supports:

- .NET
- Java
- Node.js
- Python
- PHP

Features:

- Auto Scaling
- CI/CD
- SSL
- Custom Domains
- Deployment Slots

---

# 26. What Are Grafana and Prometheus?

### Prometheus

- Monitoring tool
- Collects metrics
- Time-series database
- Alerting support

### Grafana

- Visualization tool
- Creates dashboards
- Connects to Prometheus
- Displays real-time metrics

**Interview Line:**
Prometheus collects metrics, and Grafana visualizes them through dashboards.

---

# 27. What is CPM?

**CPM (Cloud Performance Monitoring)** is the process of monitoring cloud infrastructure and applications to ensure performance, availability, and reliability.

It helps monitor:

- CPU Usage
- Memory Usage
- Disk Usage
- Network Performance
- Application Response Time
- Availability

Common Tools:

- Azure Monitor
- Prometheus
- Grafana
- Log Analytics
- Application Insights

**Interview Line:**
CPM helps proactively monitor cloud resources, detect performance issues, and ensure high availability through alerts and dashboards.
