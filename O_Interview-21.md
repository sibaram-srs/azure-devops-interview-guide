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
