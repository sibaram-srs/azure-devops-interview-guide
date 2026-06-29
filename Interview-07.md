
## Terraform + Azure + Azure DevOps (Interview Ready Answers)

PWC interviewers usually go deep into **Terraform internals, Azure networking, IaC design patterns, Azure DevOps, and Azure administration.**

---

# 2. Have You Worked on Infrastructure Administration?

### Interview Answer

"Yes. Apart from infrastructure provisioning using Terraform, I have also worked on Azure administration activities such as:

* Virtual Machine management
* VNET and NSG administration
* Storage Accounts
* Azure Key Vault
* AKS administration
* Azure Monitor
* RBAC management
* Backup and Recovery
* Troubleshooting production issues

So my experience includes both Infrastructure Engineering and Azure Administration."

---

# 3. Have You Worked on Azure Migration?

### Interview Answer

"Yes. I have been involved in migration projects where workloads were moved from on-premises environments to Azure.

The migration included:

* Assessment
* Discovery
* Dependency Mapping
* Infrastructure Provisioning
* Data Migration
* Cutover Planning
* Validation
* Post-Migration Support"

---

# 4. Azure Migrate or Something Else?

### Interview Answer

"Primarily Azure Migrate.

Azure Migrate helped us with:

* Server Discovery
* Dependency Analysis
* Cost Estimation
* Readiness Assessment
* Migration Planning

For databases we also used:

* Azure Database Migration Service (DMS)

Depending on workload requirements."

---

# 5. PowerShell Experience?

### Interview Answer

"Yes.

I use PowerShell for:

* Azure Automation
* Bulk VM Operations
* Resource Reporting
* User Management
* Storage Operations
* Scheduled Tasks

Example:

```powershell
Get-AzVM
Start-AzVM
Stop-AzVM
```

PowerShell is especially useful for Windows and Azure administration."

---

# 6. Fetch Subnet ID at Runtime & Pass to VM Module

### Interview Answer

"In Terraform, the preferred approach is to expose the subnet ID from the VNET module using outputs and consume it in the VM module.

This creates a clean dependency relationship between modules."

---

# 7. What Replaces subnet_id = ?

### Interview Answer (Very Important)

### VNET Module Output

```hcl
output "subnet1_id" {
  value = azurerm_subnet.subnet1.id
}
```

### VM Module Call

```hcl
module "vm" {
  source = "../../vm"

  subnet_id = module.vnet.subnet1_id
}
```

### Correct Answer

```hcl
module.vnet.subnet1_id
```

### Interviewer Trap

❌ var.subnet_id

❌ data.subnet_id

✅ module.vnet.subnet1_id

Because the subnet is created by the VNET module and must be exposed through an output.

---

# 8. Dynamic Lock?

### Interview Answer

"Yes.

Terraform State Locking is the most common dynamic lock mechanism I have worked with.

When using Azure Storage backend, Terraform automatically locks the state file during:

```bash
terraform apply
```

This prevents concurrent modifications by multiple engineers."

---

# 9. Deploy 5 VMs – Call Module 5 Times?

### Interview Answer

"No.

Calling the module five times is not scalable.

I would use:

```hcl
for_each
```

or

```hcl
count
```

to create multiple instances dynamically."

---

# 10. Input Data Type?

### Interview Answer

For multiple VMs:

```hcl
variable "vms" {
  type = map(object({
    size = string
    location = string
  }))
}
```

Not a string.

Generally:

* String → Single Value
* List → Multiple Values
* Map/Object → Complex Structures

For enterprise deployments, map(object) is preferred."

---

# 11. Optimize Multi-VM Deployment

### Interview Answer

"I would create a reusable VM module and use:

```hcl
for_each
```

Benefits:

* Less Code
* Better Maintainability
* Easier Scaling
* Cleaner Design

This is the preferred enterprise Terraform pattern."

---

# 12. Configure Self-Hosted Agent

### Interview Answer

Steps:

### Create VM

Linux or Windows VM.

### Download Agent

From Azure DevOps Agent Pool.

### Register Agent

```bash
./config.sh
```

Provide:

* Organization URL
* Agent Pool
* Authentication Method

### Start Agent

```bash
./run.sh
```

Agent becomes available in Azure DevOps."

---

# 13. Authenticate Self-Hosted Agent to Azure

### Interview Answer

Best Practice:

### Managed Identity

or

### Service Principal

Used to authenticate Azure resources securely.

Avoid storing credentials directly in pipelines."

---

# 14. What is PAT?

### Interview Answer

"PAT stands for Personal Access Token.

It is used to authenticate:

* Azure DevOps Repositories
* Pipelines
* REST APIs

without using a username/password."

---

# 15. PAT or IAM?

### Interview Answer

"For Azure resource access, I prefer Managed Identity or Service Principal.

For Azure DevOps integration, PAT may still be required in specific scenarios.

Current best practice is:

Managed Identity > Service Principal > PAT"

---

# 16. Manual Approval Before Apply Stage

### Interview Answer

In Azure DevOps:

Use Environment Approval.

Pipeline:

```yaml
Plan
↓
Manual Approval
↓
Apply
```

Production deployment pauses until approval is granted.

This is standard Terraform deployment governance."

---

# 17. Execute Script Without Logging Into VM

### Interview Answer

Several options exist:

### Run Command

Azure Portal

VM
→ Run Command

Execute Bash or PowerShell remotely.

---

### Azure CLI

```bash
az vm run-command invoke
```

---

### Azure Automation

Runbooks

---

### Custom Script Extension

Deploy scripts remotely.

---

### Serial Console

Used mainly for troubleshooting boot and connectivity issues.

### Interview Answer

"Yes, I have used both Run Command and Serial Console.

Run Command is generally used for administrative tasks without SSH access.

Serial Console is used when the VM is inaccessible due to network or OS-level issues."

---

# 18. NSG Priority Question

### Scenario

Priority 100:

```text
Deny All
```

Need:

```text
Allow HTTPS
```

### Interview Answer

Create:

```text
Allow HTTPS
Priority = 90
Port = 443
```

Reason:

Azure processes NSG rules from lowest number to highest number.

Therefore:

```text
90 Allow HTTPS
100 Deny All
```

Traffic on 443 will be allowed.

### Important

Don't increase the Deny Rule.

Create a higher-priority Allow Rule (lower number).

---

# 19. Usable IPs in /28 Subnet

### Interview Answer

### Formula

```text
2^(32-28)
= 16 IPs
```

Azure reserves 5 IPs.

Therefore:

```text
16 - 5 = 11 usable IPs
```

### Final Answer

✅ 11 usable IP addresses

This is a very common Azure networking question.

---

# 20. Connectivity Between On-Prem and Azure

### Interview Answer

### Site-to-Site VPN

* Internet Based
* IPSec Tunnel
* Cost Effective

Used for smaller workloads.

---

### ExpressRoute

* Private Dedicated Connection
* Lower Latency
* Higher Bandwidth
* Higher Reliability

Used for enterprise workloads.

---

### Point-to-Site VPN

Individual user connectivity.

---

### VNET Peering

Azure-to-Azure connectivity.

---

### Interview Closing Answer

"For enterprise environments, ExpressRoute is generally preferred because it provides private, reliable, and high-performance connectivity between on-premises data centers and Azure."

---
