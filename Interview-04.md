## Interview Ready Answers (Senior Level)

---

# 2. Do You Have Ansible Experience?

### Interview Answer

"Yes, I have hands-on experience with Ansible for configuration management and automation.

I have used Ansible for:

* Server provisioning
* Package installation
* User management
* Application deployment
* Configuration management
* Patch management

I have worked with:

* Playbooks
* Inventories
* Roles
* Variables
* Templates (Jinja2)
* Ansible Vault

In production, I generally use Terraform for infrastructure provisioning and Ansible for configuration management."

---

# 3. What About Scripting? PowerShell or Python?

### Interview Answer

"Yes, scripting is part of my daily responsibilities.

### Bash

* Linux automation
* Monitoring scripts
* Deployment automation

### Python

* API automation
* Reporting
* Infrastructure automation
* Data processing

### PowerShell

* Azure administration
* Windows VM management
* Azure resource automation

Among these, Bash and PowerShell are used most frequently in my current environment."

---

# 4. Which OS Do You Use?

### Interview Answer

"I work with both Linux and Windows, but Linux is my primary operating system because most containerized workloads and Kubernetes environments run on Linux.

Linux distributions I have worked on:

* Ubuntu
* RHEL
* CentOS

For Windows, I mostly handle:

* Azure Windows VMs
* IIS
* RDP administration"

---

# 5. What Pod Errors Have You Handled?

### Interview Answer

"I have handled several Kubernetes pod issues, including:

* CrashLoopBackOff
* ImagePullBackOff
* ErrImagePull
* Pending Pods
* OOMKilled
* CreateContainerConfigError
* CreateContainerError
* FailedMount
* FailedScheduling
* NodeNotReady related issues

Most issues are identified using:

```bash
kubectl describe pod
kubectl logs
kubectl get events
```

"

---

# 6. CrashLoopBackOff – How Do You Fix It?

### Interview Answer

"My troubleshooting process is:

### Check Previous Logs

```bash
kubectl logs <pod-name> --previous
```

### Check Pod Details

```bash
kubectl describe pod <pod-name>
```

### Verify

* Environment variables
* Secrets
* ConfigMaps
* Database connectivity
* Startup scripts
* Application configuration
* Image version

If infrastructure resources are healthy, CrashLoopBackOff is usually caused by application startup failure."

---

# 7. Service Not Reachable Inside Cluster

### Interview Answer

"I follow a systematic approach:

### Verify Service

```bash
kubectl get svc
```

### Check Endpoints

```bash
kubectl get endpoints
```

### Verify Pods

```bash
kubectl get pods
```

### DNS Validation

```bash
nslookup service-name
```

### Connectivity Testing

```bash
curl service-name
```

### Check Labels

Most issues occur because Service selectors don't match Pod labels."

---

# 8. External Traffic Not Reaching Through Ingress

### Interview Answer

"I troubleshoot layer by layer.

### Verify Ingress

```bash
kubectl get ingress
```

### Describe Ingress

```bash
kubectl describe ingress
```

### Check Ingress Controller

```bash
kubectl get pods -n ingress-nginx
```

### Verify Backend Service

```bash
kubectl get svc
```

### Verify Endpoints

```bash
kubectl get endpoints
```

### Check DNS Resolution

Ensure DNS points to the Ingress Load Balancer IP.

I always start from Ingress and move backward toward the application."

---

# 9. How Will You Secure Kubernetes Cluster?

### Interview Answer

"Cluster security requires multiple layers.

### Authentication

* Azure AD Integration

### Authorization

* RBAC

### Network Security

* Network Policies
* NSGs

### Secrets

* Azure Key Vault

### Image Security

* Image Scanning
* Trusted Registries

### Pod Security

* Run as non-root
* Read-only filesystem

### Cluster Security

* Disable anonymous access
* Restrict API access
* Enable Audit Logs

### Monitoring

* Defender for Cloud
* Prometheus
* Azure Monitor"

---

# 10. Have You Done Deployment Using YAML?

### Interview Answer

"Yes.

I regularly create and modify:

* Deployment YAML
* Service YAML
* Ingress YAML
* ConfigMap YAML
* Secret YAML
* PVC YAML

Most AKS deployments in our environment are YAML-based and integrated with Azure DevOps pipelines."

---

# 11. Create a PVC YAML

### Interview Answer

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: app-pvc

spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 5Gi
```

Apply:

```bash
kubectl apply -f pvc.yaml
```

---

# 12. Docker Commands Used

### Interview Answer

Most common commands:

```bash
docker build -t app:v1 .
```

```bash
docker images
```

```bash
docker ps
```

```bash
docker ps -a
```

```bash
docker run -d nginx
```

```bash
docker logs container-id
```

```bash
docker exec -it container-id bash
```

```bash
docker stop container-id
```

```bash
docker rm container-id
```

```bash
docker push image-name
```

```bash
docker pull image-name
```

---

# 13. What Is Ansible Role?

### Interview Answer

"Ansible Role is a reusable structure used to organize automation tasks.

Instead of writing everything in one playbook, we divide configurations into roles.

For example:

* Webserver Role
* Database Role
* Monitoring Role

This improves reusability, maintainability, and standardization."

---

# 14. Structure of Ansible Role

### Interview Answer

```text
roles/
└── nginx/
    ├── tasks/
    │   └── main.yml

    ├── handlers/
    │   └── main.yml

    ├── templates/

    ├── files/

    ├── vars/

    ├── defaults/

    └── meta/
```

### Important Components

* tasks → execution steps
* handlers → service restart actions
* templates → Jinja2 templates
* vars → variables
* defaults → default values

---

# 15. Create Container From Image

### Interview Answer

If image already exists:

```bash
docker run -d --name nginx-container nginx:latest
```

### Breakdown

* run → create container
* -d → detached mode
* --name → container name

Verify:

```bash
docker ps
```

---

# 16. Difference Between COPY and ADD

| COPY                            | ADD                                    |
| ------------------------------- | -------------------------------------- |
| Copies files from local machine | Copies files and supports URL download |
| Simple file copy                | Additional functionality               |
| Preferred in production         | Use only when extra features needed    |
| More predictable                | Less predictable                       |

### Example

COPY

```dockerfile
COPY app.jar /app/
```

ADD

```dockerfile
ADD https://example.com/file.zip /app/
```

**Best Practice:** Use COPY whenever possible.

---

# 17. Only Azure or Other Clouds?

### Interview Answer

"My primary expertise is Azure Cloud.

I have worked extensively with:

* Azure Compute
* Networking
* AKS
* Monitoring
* Security

I also understand AWS concepts and services, but Azure is my strongest cloud platform."

---

# 18. Which Azure Services Have You Used?

### Interview Answer

Frequently used services:

* Resource Groups
* Virtual Machines
* VNETs
* NSGs
* Load Balancers
* Application Gateway
* Azure Firewall
* Azure Key Vault
* Storage Accounts
* Azure Monitor
* Log Analytics
* Azure DevOps
* Azure Container Registry
* AKS
* Azure SQL Database

---

# 19. Join Linux VM to Domain

### Interview Answer

Install required packages:

```bash
sudo apt install realmd sssd
```

Discover domain:

```bash
realm discover company.com
```

Join domain:

```bash
sudo realm join company.com
```

Verify:

```bash
realm list
```

or

```bash
id username
```

If domain details appear, VM is successfully joined.

---

# 20. Azure VM Not Reachable Through SSH/RDP

### Interview Answer

I troubleshoot layer by layer.

### Step 1

Verify VM status:

```bash
az vm list -d
```

### Step 2

Verify NSG Rules:

* SSH 22
* RDP 3389

### Step 3

Check Public IP assignment.

### Step 4

Check OS Firewall.

### Step 5

Use Azure Serial Console.

### Step 6

Review Boot Diagnostics.

### Step 7

Verify VPN/VNET routing.

### Step 8

Restart VM if necessary.

This structured approach quickly identifies connectivity issues."

---

# 21. SSH and RDP Ports

### Interview Answer

| Protocol | Port |
| -------- | ---- |
| SSH      | 22   |
| RDP      | 3389 |

Additional commonly used ports:

| Service        | Port |
| -------------- | ---- |
| HTTP           | 80   |
| HTTPS          | 443  |
| Kubernetes API | 6443 |
| MySQL          | 3306 |
| PostgreSQL     | 5432 |
| SQL Server     | 1433 |

---


For Senior DevOps rounds, focus heavily on:

✅ AKS Troubleshooting
✅ Terraform
✅ Azure Networking
✅ Docker Commands
✅ Ansible Roles & Playbooks
✅ CI/CD Pipelines
✅ Linux Troubleshooting
✅ Production Incident Handling
✅ Monitoring & Logging

These are the areas where interviewers usually go deepest.
