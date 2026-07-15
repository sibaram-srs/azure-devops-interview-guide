# Cogn DI izant L2/L3 DevOps Interview – Interview Ready Answers

---

# 1. What are your day-to-day responsibilities in your current project?

- Build and maintain Azure DevOps CI/CD pipelines.
- Provision Azure infrastructure using Terraform.
- Deploy applications to AKS using Helm.
- Manage Azure resources (VMs, Storage, Networking, Key Vault).
- Monitor infrastructure using Azure Monitor and Log Analytics.
- Troubleshoot production issues.
- Perform release deployments and rollbacks.
- Implement DevSecOps practices (SonarQube, Trivy).
- Support developers and automate manual tasks.

---

# 2. Which CI/CD tool do you use?

"I primarily use **Azure DevOps** for CI/CD. I have also worked with **GitHub Actions** and have basic exposure to Jenkins."

---

# 3. Have you created custom Terraform modules?

"Yes. I have built reusable Terraform modules for Virtual Machines, Virtual Networks, NSGs, Storage Accounts, Key Vaults, Recovery Services Vaults, and AKS clusters."

---

# 4. How do you design reusable Terraform modules for multiple environments?

"I create one reusable child module and keep environment-specific values in separate **tfvars** files.

Structure:

```
modules/
    vm/
    vnet/
    keyvault/

environments/
    dev.tfvars
    uat.tfvars
    prod.tfvars

main.tf
variables.tf
outputs.tf
```

The same module is reused by passing different variables for Dev, UAT, and Production."

---

# 5. What are Terraform Workspaces?

"Terraform Workspaces allow multiple state files for the same configuration.

Benefits:
- Separate state per environment
- No code duplication
- Easy environment switching

I prefer **separate folders + remote backend** for Production because it's easier to manage and safer. Workspaces are useful for lightweight environments."

---

# 6. Terraform version upgrade?

1. Read release notes.
2. Verify provider compatibility.
3. Update required_version.
4. Update provider versions.
5. Run terraform init -upgrade.
6. Execute terraform validate.
7. Run terraform plan.
8. Test in Dev.
9. Deploy to Production after validation.

---

# 7. Purpose of null_resource?

"It executes scripts or automation that Terraform cannot directly manage."

Example:

- Run Ansible after VM creation.
- Execute PowerShell or Bash scripts.
- Trigger API calls.

---

# 8. What is a data block?

"Data sources fetch existing Azure resources without creating them."

Example:

```terraform
data "azurerm_resource_group" "rg" {
  name = "Prod-RG"
}
```

Used when referencing existing resources.

---

# 9. Redeploy only one corrupted VM?

"I would use:

```bash
terraform taint azurerm_linux_virtual_machine.vm
terraform apply
```

or

```bash
terraform apply -replace="azurerm_linux_virtual_machine.vm"
```

Only that VM will be recreated."

---

# 10. Recover corrupted Terraform state?

- Restore from remote backend versioning.
- Restore from backup.
- Verify state consistency.
- Run terraform refresh or terraform plan.

Best Practices

- Store state in Azure Storage.
- Enable versioning.
- Enable state locking.
- Encrypt storage.
- Restrict RBAC access.

---

# 11. Secure secrets in Terraform?

- Azure Key Vault
- Managed Identity
- Service Principal
- Sensitive variables
- Remote backend encryption
- Never hardcode passwords

Terraform reads secrets using Key Vault data source.

---

# 12. Import existing Azure resources?

```bash
terraform import azurerm_storage_account.sa /subscriptions/.../storageAccounts/demo
```

Then update Terraform code to match the imported resource.

---

# 13. Terraform Drift?

"Drift occurs when someone manually changes Azure resources outside Terraform."

Detection

```bash
terraform plan
```

Handling

- Review changes.
- Decide whether Azure or Terraform is correct.
- Update Terraform code or revert manual changes.
- Apply after approval.

---

# 14. Azure Verified Modules (AVM) or CAF?

"Yes.

I have worked with Azure Verified Modules for standardized deployments and followed Cloud Adoption Framework principles while designing enterprise Azure environments."

---

# 15. Root vs Child Modules?

Root Module

- Calls child modules
- Environment-specific configuration

Child Module

- Reusable resource definitions

I mostly build reusable modules and reuse official Azure modules whenever appropriate.

---

# 16. Infrastructure and Application repositories?

"I prefer separate repositories.

Infrastructure Repository
- Terraform
- Networking
- Azure Resources

Application Repository
- Source Code
- Docker
- CI Pipeline

This provides better security, lifecycle management, and independent releases."

---

# 17. Application team requests new VM?

1. Receive requirements.
2. Select VM module.
3. Update tfvars.
4. Run Terraform Plan.
5. Approval.
6. Terraform Apply.
7. Validate VM.
8. Configure monitoring.
9. Store secrets in Key Vault.
10. Handover with documentation.

---

# 18. DRY Principle?

I use:

- Root Module
- Child Modules
- locals.tf
- variables.tf
- outputs.tf
- for_each
- tfvars

Shared resources like:

- Resource Group
- Key Vault
- Recovery Services Vault
- Log Analytics

are created once and referenced by multiple VM modules.

---

# 19. for_each vs count vs map?

for_each

- Best for named resources
- Uses key-value pairs

Example

```terraform
for_each = var.vms
```

count

- Fixed number of resources

```terraform
count = 3
```

Map

```terraform
{
dev="Standard_B2s"
prod="Standard_D4s"
}
```

Nested Map

Used for multiple VM properties.

---

# 20. Azure experience?

"I have hands-on experience with Azure and working knowledge of AWS.

Azure:
- AKS
- Azure DevOps
- Key Vault
- Storage
- Networking
- VM
- App Gateway
- Monitor

AWS:
- EC2
- IAM
- VPC
- S3
- EKS
- CloudWatch
- Terraform"

---

# 21. Cloud Migration Strategy?

Follow CAF

1. Assess
2. Plan
3. Ready
4. Adopt
5. Govern
6. Manage

Architecture

- Landing Zone
- Hub-Spoke Network
- ExpressRoute/VPN
- Azure Migrate
- Site Recovery
- Terraform Automation
- CI/CD

Migration

- Pilot
- Test
- Incremental Migration
- Validation
- Cutover

Use defined RPO/RTO for business continuity.

---

# 22. Temporary Internet without Public IP?

"I would use Azure NAT Gateway.

Benefits

- Secure outbound internet
- No Public IP on VM
- Centralized outbound connectivity
- Easy to control using NSGs and Firewall."

---

# 23. Recent automation?

"I automated VM provisioning, Azure resource inventory, backup validation, tagging compliance, and deployment processes using Terraform, Bash, PowerShell, Python, Azure CLI, and GitHub Copilot."

---

# 24. Python case-insensitive resource matching?

Convert both strings to lowercase.

Example

```python
if resource_name.lower() == input_name.lower():
    print("Matched")
```

---

# 25. VM RDP/SSH not working?

Check

- VM Status
- NSG
- Azure Firewall
- Route Table
- Public IP/Bastion
- Boot Diagnostics
- Network Watcher
- VM Agent
- Disk Space
- SSH/RDP Service
- Authentication
- Serial Console

---

# 26. Azure Policy hierarchy?

Definition

→ Policy Rule

↓

Initiative

→ Collection of Policies

↓

Assignment

→ Apply Policy

↓

Remediation

→ Fix Existing Resources

Custom Policy Example

Restrict deployment only to **Central India** using location policy.

---

# 27. Hundreds of monitoring alerts?

1. Check recent deployments.
2. Identify common resource.
3. Remove duplicate alerts.
4. Verify thresholds.
5. Check dependencies.
6. Correlate logs.
7. Validate genuine incidents.
8. Perform RCA.
9. Tune alert rules.
10. Document findings.

---

# 28. Storage Private Endpoint not working?

Check

- Private Endpoint status
- Private DNS Zone
- DNS Resolution

```bash
nslookup storageaccount.privatelink.blob.core.windows.net
```

- NSG
- UDR
- Firewall
- Port 443
- VNet Peering
- Storage Firewall
- Network Watcher
- Terraform changes

---

# 29. Access Storage without RBAC?

"I would request a **Shared Access Signature (SAS) Token**.

Use SAS when:

- Temporary access is required.
- Fine-grained permissions are needed.
- RBAC cannot be granted.

RBAC is preferred for long-term managed access, while SAS is best for short-term delegated access."
