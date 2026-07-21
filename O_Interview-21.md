# AWS Client Round 2 – Expert DevOps Answers

---

# 1. How will you secure an S3 Bucket?

To secure an S3 bucket, I follow AWS security best practices:

- Enable **Block Public Access** at the bucket level.
- Apply the **Principle of Least Privilege** using IAM policies.
- Use **Bucket Policies** to allow access only to required users or roles.
- Enable **Server-Side Encryption (SSE-S3 or SSE-KMS)**.
- Enable **Versioning** to protect against accidental deletion.
- Enable **MFA Delete** for sensitive buckets.
- Use **AWS CloudTrail** to audit bucket access.
- Enable **S3 Access Logging**.
- Restrict access using **VPC Endpoints** for private communication.
- Use **AWS Config** and **Security Hub** for compliance monitoring.

---

# 2. How can you connect two subnets?

If the subnets are in the **same VPC/VNet**, they communicate automatically using the local route, provided:

- Network Security Groups (NSGs/Security Groups) allow the traffic.
- Network ACLs don't block it.
- OS firewall allows communication.

If the subnets are in **different VNets/VPCs**, use:

- Azure VNet Peering / AWS VPC Peering
- VPN Gateway
- Transit Gateway (AWS)
- ExpressRoute / Direct Connect for hybrid connectivity

---

# 3. What is a Management Group?

A **Management Group** is a logical container in Azure that helps organize multiple subscriptions.

It allows centralized management of:

- RBAC
- Azure Policies
- Budgets
- Compliance
- Governance

Hierarchy:

```
Management Group
        │
Subscriptions
        │
Resource Groups
        │
Resources
```

It is mainly used in enterprise environments for centralized governance.

---

# 4. How would you configure App Registration so that it connects to the pipeline?

App Registration provides a **Service Principal** for authentication.

Steps:

1. Create an App Registration in Microsoft Entra ID (Azure AD).
2. Generate a Client Secret or use a Certificate.
3. Assign the required Azure RBAC role (Contributor, Reader, etc.).
4. Note:
   - Client ID
   - Tenant ID
   - Subscription ID
   - Client Secret
5. Store the secret securely in:
   - Azure Key Vault
   - GitHub Secrets
   - Azure DevOps Service Connection
6. Configure the CI/CD pipeline to authenticate using these credentials.
7. The pipeline uses the Service Principal to deploy Azure resources securely.

---

# 5. What is a Transit Gateway?

AWS Transit Gateway is a **centralized network hub** used to connect multiple VPCs, VPNs, and Direct Connect gateways.

Instead of creating multiple VPC peerings (mesh topology), all VPCs connect to the Transit Gateway.

Benefits:

- Simplifies network architecture
- Reduces routing complexity
- Supports hybrid connectivity
- Easier scalability

Example:

```
          Transit Gateway
          /      |      \
      VPC1     VPC2     VPC3
          \      |      /
        On-Prem via VPN/DC
```

---

# 6. When do we use an Application Load Balancer (ALB)?

An **Application Load Balancer (Layer 7)** is used for HTTP/HTTPS traffic.

Use cases:

- Host-based routing
- Path-based routing
- Microservices
- Kubernetes Ingress
- SSL termination
- Web applications
- APIs
- WebSocket support

Example:

```
example.com/api   → API Server
example.com/app   → Web Server
example.com/admin → Admin Service
```

---

# 7. What is Terraform Drift?

Terraform Drift occurs when the **actual infrastructure differs from the Terraform state file** due to manual changes outside Terraform.

Example:

Terraform created:

```
VM Size = Standard_B2s
```

An administrator manually changes it in Azure Portal to:

```
VM Size = Standard_D2s_v3
```

Terraform state still expects `Standard_B2s`, so the infrastructure has drifted.

Detection:

```bash
terraform plan
```

Remediation:

- Revert manual changes.
- Update Terraform code.
- Import resources if needed.
- Avoid manual portal changes.

---

# 8. What is a Data Block in Terraform?

A **Data Block** is used to **read existing resources** without creating or modifying them.

Example:

```hcl
data "azurerm_resource_group" "rg" {
  name = "prod-rg"
}
```

Use cases:

- Reference existing VNets
- Existing Resource Groups
- Existing Key Vaults
- Existing Subnets
- Existing Storage Accounts

Difference:

- `resource` → Creates or manages infrastructure.
- `data` → Reads existing infrastructure.

---

# 9. How would you use Ansible?

I use Ansible for configuration management and application deployment.

Common tasks:

- Install software packages
- Configure web servers (Nginx, Apache)
- User and group management
- Service management
- Patch management
- Configuration updates
- Application deployment
- VM provisioning (cloud modules)

Typical workflow:

```
Inventory
      │
Playbook
      │
Tasks
      │
Target Servers
```

Example:

```bash
ansible-playbook deploy.yml
```

---

# 10. Which type of DNS record is used to map the public IP of an Application Gateway?

Typically:

- **A Record** → Maps a domain name to a Public IPv4 address.
- **AAAA Record** → Maps to an IPv6 address.
- **CNAME Record** → Maps one domain name to another DNS name (commonly used if the gateway exposes a DNS name instead of a fixed IP).

For an Application Gateway with a static public IP:

```
app.company.com
        │
        ▼
A Record
        │
Public IP
        │
Application Gateway
```

---

# 11. What is a Trust Policy?

A **Trust Policy** is an AWS IAM policy attached to a role that defines **who is allowed to assume the role**.

It specifies the trusted principal, such as:

- IAM User
- IAM Role
- AWS Service (e.g., EC2, Lambda)
- Another AWS Account

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Permission policies define **what the role can do**, while the trust policy defines **who can use the role**.

---

# 12. Why are IaC tools like Terraform idempotent?

Terraform is **idempotent** because running the same configuration multiple times produces the same desired infrastructure state.

Terraform compares:

- Desired State (Terraform code)
- Current State (Terraform state + cloud resources)

It only performs the necessary changes.

Example:

First run:

```bash
terraform apply
```

Creates:

- VNet
- Subnet
- VM

Second run (no changes):

```bash
terraform apply
```

Output:

```
No changes.
Infrastructure matches the configuration.
```

Benefits:

- Safe repeated executions
- Prevents duplicate resources
- Ensures consistent environments
- Enables reliable automation in CI/CD pipelines



# Client Round – Expert DevOps Answers

---

# 1. How will you troubleshoot if your application is down?

I follow a systematic troubleshooting approach:

### Step 1: Check Application Availability
- Verify whether the application is accessible.
- Check monitoring alerts.
- Check user impact.

### Step 2: Check Infrastructure Health
- VM/Container status
- CPU, Memory, Disk utilization
- Network connectivity
- Load Balancer health checks

### Step 3: Check Application Logs

Examples:

```bash
kubectl logs <pod-name>

journalctl -u application-service

docker logs <container>
```

### Step 4: Check Database and Dependencies
- Database connectivity
- API dependencies
- External services

### Step 5: Check Recent Changes
- Latest deployment
- Configuration changes
- Infrastructure changes

### Step 6: Mitigation
- Rollback deployment
- Scale resources
- Restart failed services

### Step 7: Root Cause Analysis
- Identify root cause
- Implement permanent fix
- Document incident

---

# 2. Explain what happens when end users hit flipkart.com. Explain network traffic flow.

The request flow is:

```
User Browser
      |
      |
DNS Resolution
      |
      |
Route53 / DNS Provider
      |
      |
Public IP / CDN
      |
      |
AWS ALB / Azure Application Gateway
      |
      |
Web Server / Kubernetes Ingress
      |
      |
Application Layer
      |
      |
Backend Services
      |
      |
Database
```

Detailed flow:

1. User enters:

```
flipkart.com
```

2. DNS resolves the domain name to the public IP.

3. Traffic reaches:
- CDN (if configured)
- Load Balancer

4. Load Balancer performs:
- SSL termination
- Health checks
- Traffic distribution

5. Request goes to application servers.

6. Application communicates with:
- Microservices
- Cache layer
- Database

7. Response travels back to the user.

---

# 3. How will you implement FinOps strategy in cloud environment?

FinOps is a cloud cost management practice combining engineering, finance, and business teams.

Implementation:

### Visibility

- Enable cost monitoring.
- Configure Cost Explorer/Azure Cost Management.
- Create dashboards.

### Optimization

- Right-size VMs.
- Remove unused resources.
- Use Reserved Instances/Savings Plans.
- Use Auto Scaling.
- Optimize storage tiers.
- Delete unused snapshots.

### Governance

- Apply tagging strategy:

Example:

```
Environment=Production
Application=Payment
Owner=TeamA
CostCenter=Finance
```

- Set budgets.
- Configure cost alerts.
- Implement approval processes.

### Continuous Improvement

- Regular cost reviews.
- Identify optimization opportunities.
- Track savings.

---

# 4. How would you set IOPS for an EBS volume?

IOPS depends on the EBS volume type.

### General Purpose SSD (gp3)

IOPS can be configured independently.

Example:

```
Volume Type: gp3

Size: 500 GB

IOPS: 10000

Throughput: 500 MB/s
```

Using AWS Console:

1. Select EBS Volume.
2. Modify Volume.
3. Change IOPS value.
4. Apply modification.

Using CLI:

```bash
aws ec2 modify-volume \
--volume-id vol-xxxx \
--iops 10000
```

For high-performance workloads:

- io2/io2 Block Express is used.

---

# 5. How would you leverage costing in S3 bucket's data?

I optimize S3 cost using:

### Storage Classes

Move data based on access patterns:

- S3 Standard → Frequently accessed data
- S3 Intelligent-Tiering → Unknown access patterns
- S3 Standard-IA → Infrequent access
- Glacier → Archive data

### Lifecycle Policies

Example:

```
After 30 days:
Move to Standard-IA

After 90 days:
Move to Glacier
```

### Additional Optimization

- Enable Compression
- Remove old versions
- Delete incomplete multipart uploads
- Monitor with S3 Storage Lens
- Use Cost Explorer

---

# 6. A machine in private subnet wants to access Internet. How would you do that?

A private subnet cannot directly access the Internet.

Solution:

Use **NAT Gateway**.

Traffic flow:

```
Private Instance

        |

Private Route Table

        |

NAT Gateway

        |

Internet Gateway

        |

Internet
```

Steps:

1. Create NAT Gateway in Public Subnet.
2. Attach Elastic IP.
3. Update Private Route Table.

Example:

```
0.0.0.0/0 → NAT Gateway
```

4. Ensure Security Groups allow outbound traffic.

---

# 7. How would you connect Terraform to CI/CD pipeline?

Terraform is integrated as pipeline stages.

Example:

```
Developer Commit

        |

CI/CD Pipeline

        |

Terraform Init

        |

Terraform Validate

        |

Terraform Plan

        |

Approval

        |

Terraform Apply
```

Example pipeline commands:

```bash
terraform init

terraform validate

terraform plan

terraform apply
```

Authentication:

- AWS IAM Role
- Azure Service Principal
- OIDC Authentication
- Environment Variables

State management:

- S3 + DynamoDB (AWS)
- Azure Storage Account (Azure)

---

# 8. Why is Ansible Agentless?

Ansible is agentless because it does not require any software installation on managed servers.

It uses:

- SSH for Linux
- WinRM for Windows

Architecture:

```
Ansible Control Node

        |

        SSH

        |

Managed Servers
```

Benefits:

- Less maintenance
- No agent upgrades
- Lightweight architecture
- Easy adoption

---

# 9. How do you use secrets in Terraform?

I never hardcode secrets in Terraform code.

Methods:

### Environment Variables

Example:

```bash
export ARM_CLIENT_SECRET=value
```

### Azure Key Vault

Store secrets securely and retrieve them during deployment.

### AWS Secrets Manager

For AWS credentials.

### Terraform Variables

Sensitive variables:

```hcl
variable "password" {
 type = string
 sensitive = true
}
```

### CI/CD Secret Management

Examples:

- GitHub Secrets
- Azure DevOps Variable Groups
- Jenkins Credentials

---

# 10. You executed a change in production environment and it caused an outage. What would be your next steps?

My approach:

### Immediate Response

1. Identify impact.
2. Check monitoring alerts.
3. Inform stakeholders.
4. Stop further changes.

### Recovery

5. Rollback the change.

Examples:

- Previous application version
- Previous Terraform state
- Previous configuration

### Investigation

6. Analyze logs.
7. Check deployment history.
8. Identify root cause.

### Prevention

9. Perform RCA.
10. Add preventive controls.

Examples:

- Better testing
- Approval gates
- Change management
- Automated validation
- Canary deployment

---

# 11. Why are you looking for a job change?

I am looking for an opportunity where I can work on larger-scale cloud and DevOps implementations, enhance my technical skills, and contribute my experience in automation, cloud infrastructure, CI/CD, and DevSecOps practices.

I am looking for a role that provides challenging projects and continuous learning opportunities while allowing me to add value to the organization.
