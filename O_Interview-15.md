## Dt. 14-07-26

# DevOps Interview Handbook
## Section 1: Introduction & Terraform Fundamentals

---

# 1. Introduction & Project Experience

## Q1. Introduce Yourself.

### Interview Ready Answer

Hello, my name is **<Your Name>**, and I have around **X years of experience** as a DevOps Engineer with hands-on experience in Azure Cloud, Terraform, Azure DevOps, Git, Docker, Kubernetes, and CI/CD automation.

In my current project, I am responsible for Infrastructure as Code using Terraform, creating and maintaining Azure DevOps YAML pipelines, managing Azure resources across multiple subscriptions, implementing CI/CD pipelines, and supporting infrastructure deployments across Development, QA, UAT, and Production environments.

I have worked extensively with remote Terraform backends, reusable modules, service connections, Azure Key Vault, Azure Storage Accounts, Virtual Machines, Networking, and monitoring deployments. I also collaborate with developers and cloud architects to automate infrastructure provisioning while following security and DevOps best practices.

---

## Q2. Explain your current project and your responsibilities.

### Interview Ready Answer

Currently, I am working on an enterprise Azure cloud project where our team manages infrastructure through Terraform and deployments through Azure DevOps.

My responsibilities include:

- Developing reusable Terraform modules.
- Managing infrastructure across multiple Azure subscriptions.
- Creating and maintaining Azure DevOps YAML pipelines.
- Configuring Service Connections using Workload Identity Federation.
- Managing remote Terraform state in Azure Storage.
- Performing code reviews and Pull Requests.
- Troubleshooting pipeline failures.
- Supporting production deployments.
- Implementing Infrastructure as Code best practices.
- Collaborating with developers and cloud architects.

---

## Q3. Explain the end-to-end CI/CD pipeline you have worked on.

### Interview Ready Answer

Our pipeline starts when a developer raises a Pull Request.

1. Code Review is performed.
2. PR validation pipeline runs.
3. After approval, the code is merged into the main branch.
4. CI Pipeline is triggered.
5. Terraform formatting and validation are executed.
6. TFLint and security scans are performed.
7. Terraform Plan is generated.
8. Manual approval is taken for Production.
9. Terraform Apply deploys infrastructure.
10. Smoke tests and validation are executed.
11. Notifications are sent through Teams or Email.

The entire pipeline is written in YAML and follows Infrastructure as Code best practices.

---

## Q4. Describe a recent Terraform template or module upgrade you performed.

### Interview Ready Answer

Recently, I upgraded one of our Virtual Machine modules.

The improvements included:

- Migrating from hardcoded values to input variables.
- Adding lifecycle rules.
- Supporting Availability Zones.
- Adding diagnostic settings.
- Introducing dynamic blocks.
- Improving module reusability.
- Version controlling the module.
- Maintaining backward compatibility.

This reduced code duplication and allowed multiple teams to use the same module.

---

## Q5. Which tools have you used in your DevOps project?

### Interview Ready Answer

The primary tools I have worked with are:

- Azure Cloud
- Terraform
- Azure DevOps
- Git
- GitHub
- Docker
- Kubernetes (AKS)
- Helm
- Azure Key Vault
- Azure Monitor
- Log Analytics
- Bash
- PowerShell
- TFLint
- SonarQube
- YAML
- Azure CLI

---

## Q6. Have you worked with Bash or PowerShell scripting?

### Interview Ready Answer

Yes.

I use Bash scripts mainly on Linux agents for automation tasks such as installing packages, file manipulation, environment setup, and Azure CLI operations.

I use PowerShell primarily for Windows-based automation, Active Directory integration, VM configuration, Azure resource management, and post-deployment tasks.

---

# 2. Terraform

---

# Terraform Basics

---

## Q1. What is Terraform?

### Interview Ready Answer

Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp.

It allows us to provision, update, and manage cloud infrastructure using declarative configuration files written in HCL (HashiCorp Configuration Language).

Terraform follows a desired state approach, where it compares the current infrastructure with the desired configuration and generates an execution plan before applying changes.

It supports multiple cloud providers such as Azure, AWS, GCP, VMware, Kubernetes, and many others.

---

## Q2. How do you set up Terraform from scratch?

### Interview Ready Answer

The typical setup process is:

1. Install Terraform.
2. Configure the cloud provider credentials.
3. Create Terraform configuration files.
4. Define the provider block.
5. Configure the backend if using remote state.
6. Run:

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```

In production, we always use a remote backend and reusable modules.

---

## Q3. What are Providers in Terraform?

### Interview Ready Answer

Providers are plugins that allow Terraform to interact with external platforms such as Azure, AWS, GCP, Kubernetes, or GitHub.

They expose resource types and data sources.

Example:

```hcl
provider "azurerm" {
  features {}
}
```

Terraform downloads the required provider during `terraform init`.

---

## Q4. What is a Resource Block?

### Interview Ready Answer

A Resource Block defines the infrastructure that Terraform should create or manage.

Every resource has:

- Resource Type
- Resource Name
- Configuration

Example:

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "prod-rg"
  location = "Central India"
}
```

Resources become part of the Terraform state.

---

## Q5. What is a Data Block?

### Interview Ready Answer

A Data Block is used to read information about existing infrastructure without creating or modifying it.

It allows Terraform to reference resources that already exist.

Example:

```hcl
data "azurerm_resource_group" "rg" {
  name = "shared-rg"
}
```

---

## Q6. Does a Data Block become part of the Terraform State File?

### Interview Ready Answer

Yes.

Terraform stores metadata about data sources in the state file so it can evaluate dependencies and maintain consistency during future operations.

However, Terraform does **not** manage the lifecycle of those existing resources.

---

## Q7. What is a Null Resource?

### Interview Ready Answer

A Null Resource is a special Terraform resource that doesn't create any cloud infrastructure.

It is commonly used to execute scripts using provisioners or trigger actions based on changes.

Example use cases:

- Execute PowerShell scripts
- Execute Bash scripts
- Run configuration tasks
- Trigger automation

---

## Q8. What are Terraform Functions?

### Interview Ready Answer

Functions are built-in utilities that manipulate data inside Terraform expressions.

Examples include:

- `length()`
- `join()`
- `lookup()`
- `merge()`
- `contains()`
- `format()`
- `lower()`
- `upper()`
- `file()`

They simplify configuration and improve code readability.

---

## Q9. What are Type Constraints?

### Interview Ready Answer

Type Constraints define the expected data type of a variable.

Supported types include:

- string
- number
- bool
- list
- map
- set
- object
- tuple
- any

They improve validation and reduce configuration errors.

---

## Q10. What data types are supported in Terraform?

### Interview Ready Answer

Terraform supports:

- string
- number
- bool
- list
- map
- set
- object
- tuple
- any

Among these, string, list, map, and object are most commonly used in enterprise projects.

---

## Q11. What is the difference between a Resource and a Data Source?

### Interview Ready Answer

| Resource | Data Source |
|----------|-------------|
| Creates or manages infrastructure | Reads existing infrastructure |
| Lifecycle managed by Terraform | Lifecycle not managed by Terraform |
| Appears in State | Metadata stored in State |
| Can create/update/delete | Read-only |

---

# Variables

---

## Q12. What is a Variable?

### Interview Ready Answer

Variables make Terraform code reusable and configurable.

Instead of hardcoding values, variables allow different environments to use the same module by supplying different inputs.

---

## Q13. Difference between `variable.tf`, `variables.tf`, and `terraform.tfvars`.

### Interview Ready Answer

`variable.tf`

Contains variable declarations.

```hcl
variable "location" {
  type = string
}
```

---

`variables.tf`

There is **no functional difference** between `variable.tf` and `variables.tf`.

The filename is only a naming convention.

Terraform loads every `.tf` file automatically.

---

`terraform.tfvars`

Contains actual values.

```hcl
location = "Central India"
```

This separates code from configuration.

---

## Q14. How do you assign a default value to a variable?

### Interview Ready Answer

Using the `default` argument.

Example:

```hcl
variable "location" {
  type = string
  default = "Central India"
}
```

If no value is provided, Terraform uses the default value.

---

## Q15. What is a Sensitive Variable?

### Interview Ready Answer

Sensitive variables are used for secrets such as:

- Passwords
- Tokens
- API Keys
- Connection Strings

They prevent values from being displayed in CLI output.

Example:

```hcl
variable "password" {
  sensitive = true
}
```

---

## Q16. If a variable is marked as sensitive, how can you view its value?

### Interview Ready Answer

Sensitive values are hidden from normal CLI output.

They can still be accessed:

- Through the Terraform state file (if authorized)
- Using `terraform output -json`
- Using `terraform console`
- By reading the backend state (with appropriate permissions)

In production, secrets should always be stored in Azure Key Vault or another secure secret management solution instead of hardcoding them.

---

# Modules

---

## Q17. What are Terraform Modules?

### Interview Ready Answer

Modules are reusable collections of Terraform resources.

They help standardize infrastructure, reduce duplication, improve maintainability, and enforce organizational best practices.

Every Terraform configuration has at least one root module, and additional reusable child modules can be created for common infrastructure components.

---

## Q18. What is the difference between a Root Module and a Child Module?

### Interview Ready Answer

**Root Module**
- The main Terraform configuration where execution starts.
- Contains provider configuration, backend configuration, and calls child modules.

**Child Module**
- A reusable module invoked by the root module.
- Contains resources for a specific purpose, such as creating a Virtual Machine, Storage Account, or Virtual Network.

In enterprise projects, infrastructure is typically built using reusable child modules managed by a root module.

---

## Q19. What is the difference between a Pattern Module and a Standard Module?

### Interview Ready Answer

A **Standard Module** provisions a single type of resource, such as a Virtual Machine or Storage Account.

A **Pattern Module** combines multiple related resources into a complete deployment pattern. For example, a VM pattern module may create a Resource Group, Virtual Network, Network Interface, Virtual Machine, Diagnostics, Monitoring, and Backup configuration together.

Pattern modules are commonly used to enforce organizational standards and reduce deployment complexity.

---

## Q20. What is a Resource Module?

### Interview Ready Answer

A Resource Module is a reusable Terraform module designed to create a specific infrastructure resource, such as a Virtual Machine, Storage Account, or Virtual Network.

Its purpose is to standardize deployments, reduce code duplication, and improve maintainability across projects.

---

## Q21. Write Terraform code using Modules and `for_each`.

### Interview Ready Answer

```hcl
module "storage_accounts" {
  source   = "./modules/storage"
  for_each = var.storage_accounts

  name     = each.key
  location = each.value.location
}
```

This example creates multiple Storage Accounts using the same reusable module, where each instance is configured with different input values provided through the `storage_accounts` map.

---

# State Management

---

## Q22. What is a Terraform State File?

### Interview Ready Answer

The Terraform State File is a JSON file that stores the current state of the infrastructure managed by Terraform.

It maps the resources defined in the Terraform configuration to the actual resources deployed in the cloud. Terraform uses this file during `plan`, `apply`, and `destroy` operations to determine what changes are required.

In enterprise environments, the state file is stored in a remote backend to enable collaboration, state locking, versioning, and encryption.

---

## Q23. When is the Terraform State File created?

### Interview Ready Answer

The state file is created after the first successful `terraform apply`.

If a remote backend is configured, the state file is created and stored in the configured backend instead of the local machine.

---

## Q24. Where should the Terraform State File be stored?

### Interview Ready Answer

In production, the state file should always be stored in a remote backend.

Common options include:

- Azure Storage Account
- Amazon S3 with DynamoDB for locking
- Terraform Cloud
- Google Cloud Storage

Remote storage enables centralized state management, encryption, versioning, and team collaboration.

---

## Q25. What backend types are supported in Terraform?

### Interview Ready Answer

Common backend types include:

- local
- azurerm
- s3
- gcs
- remote (Terraform Cloud)
- consul
- http

In Azure environments, the `azurerm` backend is the most commonly used.

---

## Q26. How do you secure or encrypt the State File?

### Interview Ready Answer

The state file should be secured by:

- Using a remote backend
- Enabling encryption at rest
- Restricting access through RBAC
- Storing secrets in Azure Key Vault instead of Terraform variables
- Enabling versioning
- Using private endpoints where applicable
- Protecting access with managed identities or workload identity federation

---

## Q27. How can sensitive data be protected in the State File?

### Interview Ready Answer

Although variables can be marked as sensitive, their values may still exist in the state file.

To protect sensitive information:

- Use Azure Key Vault or another secret management solution.
- Store the state in an encrypted remote backend.
- Restrict backend access using RBAC.
- Avoid hardcoding secrets in Terraform code.
- Rotate secrets regularly.

---

## Q28. What is State Locking?

### Interview Ready Answer

State Locking prevents multiple users from modifying the same Terraform state simultaneously.

When a Terraform operation starts, the backend acquires a lock. Other users must wait until the lock is released before making changes.

This prevents state corruption and ensures consistency.

---

## Q29. Why is the `.terraform.lock.hcl` file created?

### Interview Ready Answer

The `.terraform.lock.hcl` file locks the versions of Terraform providers used by the project.

It ensures that all team members and CI/CD pipelines use the same provider versions, resulting in consistent and reproducible deployments across environments.


# Terraform - Dependencies, Provisioners, Lifecycle, Count & for_each, Workspaces, Moved Block

---

# Dependencies

---

## Q30. What are Terraform Dependencies?

### Interview Ready Answer

Terraform Dependencies define the order in which resources should be created, modified, or destroyed.

Terraform automatically builds a **Dependency Graph (DAG - Directed Acyclic Graph)** before execution. Based on this graph, it determines which resources can be created in parallel and which resources must wait for others to complete.

There are two types of dependencies:

- **Implicit Dependency** (Automatically detected)
- **Explicit Dependency** (Manually defined using `depends_on`)

Dependencies ensure resources are provisioned in the correct order.

> **Interview Tip:** Mention that Terraform creates a dependency graph internally. This is a keyword interviewers like to hear.

---

## Q31. Explain Implicit Dependencies with an example.

### Interview Ready Answer

An **Implicit Dependency** is automatically identified by Terraform when one resource references another resource's attribute.

Since Terraform understands the relationship, we do **not** need to use `depends_on`.

### Example

```hcl
resource "azurerm_resource_group" "rg" {
  name     = "prod-rg"
  location = "Central India"
}

resource "azurerm_storage_account" "storage" {
  name                     = "prodstorage123"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

Here, the Storage Account references the Resource Group.

Terraform automatically understands that:

```
Resource Group
      │
      ▼
Storage Account
```

No `depends_on` is required.

---

## Q32. Explain Explicit Dependencies using `depends_on`.

### Interview Ready Answer

An **Explicit Dependency** is used when Terraform cannot automatically identify the dependency.

In such cases, we use the `depends_on` meta-argument to force Terraform to create resources in a specific order.

This is commonly used when the dependency is logical rather than based on attribute references.

### Example

```hcl
resource "azurerm_virtual_machine" "vm" {

  depends_on = [
    azurerm_network_interface.nic
  ]

}
```

Terraform will always create the Network Interface before creating the Virtual Machine.

Common scenarios include:

- VM depends on Custom Script Extension
- Role Assignment before Resource Deployment
- Key Vault Access Policy before Secret Creation
- Diagnostic Settings
- Custom Provisioners

---

## Q33. Write the syntax for an Explicit Dependency.

### Interview Ready Answer

```hcl
resource "resource_type" "resource_name" {

  depends_on = [
    resource_type.resource_name
  ]

}
```

Example:

```hcl
resource "azurerm_linux_virtual_machine" "vm" {

  depends_on = [
    azurerm_network_interface.nic,
    azurerm_resource_group.rg
  ]

}
```

---

# Provisioners

---

## Q34. What are Terraform Provisioners?

### Interview Ready Answer

Provisioners are used to execute scripts or commands **after** a resource has been created or **before** it is destroyed.

They are generally considered a **last resort**, because Terraform is primarily an Infrastructure as Code tool rather than a configuration management tool.

Whenever possible, tools such as **Ansible, Chef, Puppet, or Cloud-init** are preferred over Provisioners.

Provisioners are mainly used for:

- Running Bash scripts
- Running PowerShell scripts
- Copying files
- Initial VM configuration

> **Interview Tip:** Mention that HashiCorp recommends avoiding Provisioners whenever possible.

---

## Q35. What are the different types of Provisioners?

### Interview Ready Answer

Terraform provides three commonly used Provisioners:

### 1. File Provisioner

Copies files from the local machine to the remote resource.

---

### 2. Local-exec Provisioner

Executes commands on the machine running Terraform.

Example:

```hcl
provisioner "local-exec" {
  command = "echo Deployment Completed"
}
```

---

### 3. Remote-exec Provisioner

Executes commands directly on the remote VM.

Supports:

- Linux (SSH)
- Windows (WinRM)

Example:

```hcl
provisioner "remote-exec" {
  inline = [
    "sudo apt update"
  ]
}
```

---

## Q36. What is a File Provisioner?

### Interview Ready Answer

A File Provisioner copies files or directories from the machine running Terraform to the target Virtual Machine.

It is commonly used to copy:

- Shell scripts
- PowerShell scripts
- Configuration files
- Certificates

Example:

```hcl
provisioner "file" {
  source      = "install.ps1"
  destination = "C:/Temp/install.ps1"
}
```

---

## Q37. What is the difference between a Provider and a Provisioner?

### Interview Ready Answer

| Provider | Provisioner |
|-----------|-------------|
| Used to communicate with cloud platforms | Used to execute scripts after deployment |
| Creates and manages infrastructure | Performs configuration tasks |
| Required for Terraform | Optional |
| Executes during infrastructure provisioning | Executes after resource creation or before destruction |
| Example: AzureRM Provider | Example: remote-exec, file |

**Simple interview explanation:**

- **Provider creates infrastructure.**
- **Provisioner configures infrastructure.**

---

## Q38. Which Provisioner would you use to run a PowerShell script after VM creation?

### Interview Ready Answer

The correct approach is:

- Use the **File Provisioner** to copy the PowerShell script to the VM.
- Use the **Remote-exec Provisioner** to execute the script.

Example flow:

```
Terraform
     │
     ▼
File Provisioner
(Copy Script)
     │
     ▼
Remote-exec
(Run Script)
```

For Azure Virtual Machines, the recommended production approach is to use the **Azure Custom Script Extension** or **Cloud-init** instead of Provisioners.

---

# Lifecycle

---

## Q39. What is the Lifecycle Block?

### Interview Ready Answer

The Lifecycle Block is a Terraform meta-argument used to control how Terraform creates, updates, or destroys resources.

It provides additional control over resource behavior during infrastructure changes.

Common Lifecycle arguments include:

- create_before_destroy
- prevent_destroy
- ignore_changes
- replace_triggered_by

---

## Q40. Explain the following Lifecycle arguments.

### 1. `create_before_destroy`

### Interview Ready Answer

Terraform creates the new resource first and deletes the old resource only after the new one is successfully created.

This helps avoid downtime.

Example use case:

- Virtual Machines
- Load Balancers
- Storage Accounts

---

### 2. `prevent_destroy`

### Interview Ready Answer

Prevents Terraform from accidentally deleting a resource.

If Terraform attempts to destroy the resource, it throws an error.

Commonly used for:

- Production Databases
- Key Vaults
- Storage Accounts

Example:

```hcl
lifecycle {
  prevent_destroy = true
}
```

---

### 3. `ignore_changes`

### Interview Ready Answer

Tells Terraform to ignore changes made to specific resource attributes outside Terraform.

Useful when another team or Azure Policy updates certain properties.

Example:

```hcl
lifecycle {
  ignore_changes = [
    tags
  ]
}
```

Terraform will not attempt to revert tag changes.

---

### 4. `replace_triggered_by`

### Interview Ready Answer

Forces Terraform to replace a resource when another specified resource changes.

Useful when resources are tightly coupled.

Example:

```hcl
lifecycle {
  replace_triggered_by = [
    azurerm_network_interface.nic
  ]
}
```

---

## Q41. Give a practical use case of the Lifecycle Block.

### Interview Ready Answer

One practical example is a Production Storage Account.

We configure:

- `prevent_destroy` to avoid accidental deletion.
- `ignore_changes` for tags managed by another team.
- `create_before_destroy` during storage migration to minimize downtime.

This ensures safe deployments while protecting critical production resources.

---

# Count & for_each

---

## Q42. What is the difference between `count` and `for_each`?

### Interview Ready Answer

| count | for_each |
|--------|----------|
| Uses numeric index | Uses unique key |
| Suitable for identical resources | Suitable for different resource configurations |
| Uses list | Uses map or set |
| Deleting one resource changes indexes | Stable resource mapping |
| Less preferred in enterprise projects | Recommended for production |

**Example using `count`:**

```hcl
count = 3
```

Creates:

```
VM[0]
VM[1]
VM[2]
```

---

**Example using `for_each`:**

```hcl
for_each = {
  vm1 = "East US"
  vm2 = "Central India"
}
```

Creates:

```
vm1
vm2
```

---

## Q43. Why is deleting resources more difficult with `count`?

### Interview Ready Answer

Resources created using `count` are identified by numeric indexes.

Example:

```
VM[0]
VM[1]
VM[2]
```

If `VM[1]` is removed, Terraform shifts the indexes:

```
VM[0]
VM[2] → VM[1]
```

Terraform interprets this as:

- Destroy old resource
- Create a new resource

This can lead to unnecessary resource replacement.

With `for_each`, resources are identified by unique keys, so deleting one resource does not affect the others.

---

## Q44. Is `count` limited to integer values?

### Interview Ready Answer

Yes.

`count` accepts only integer values because it specifies how many instances of a resource Terraform should create.

Example:

```hcl
count = 5
```

This creates five resource instances.

---

## Q45. Which approach would you use to create 10 Storage Accounts?

### Interview Ready Answer

I would use **`for_each`**.

It provides:

- Stable resource identities
- Better scalability
- Easier maintenance
- Cleaner updates
- Reduced risk during deletion

This is the preferred approach in enterprise environments.

---

# Workspaces

---

## Q46. What are Terraform Workspaces?

### Interview Ready Answer

Terraform Workspaces allow us to manage multiple state files using the same Terraform configuration.

Each workspace maintains an independent state, enabling us to deploy the same infrastructure to different environments such as Development, QA, UAT, and Production.

---

## Q47. Why do we use Terraform Workspaces?

### Interview Ready Answer

Workspaces help isolate environments while reusing the same codebase.

Benefits include:

- Separate state files
- Environment isolation
- Reduced code duplication
- Simplified management

However, for large enterprise projects, separate backend configurations are generally preferred over Workspaces for Production environments.

---

# Moved Block

---

## Q48. What is the `moved` block in Terraform?

### Interview Ready Answer

The `moved` block is used to tell Terraform that a resource has been renamed or moved to a different module without recreating the infrastructure.

It updates the resource address in the state file while preserving the existing resource.

This feature helps avoid unnecessary destroy-and-create operations during refactoring.

---

## Q49. Give an example of using a `moved` block.

### Interview Ready Answer

Suppose a resource was originally defined as:

```hcl
resource "azurerm_storage_account" "storage" {}
```

After refactoring, it is renamed:

```hcl
resource "azurerm_storage_account" "prod_storage" {}
```

We add:

```hcl
moved {
  from = azurerm_storage_account.storage
  to   = azurerm_storage_account.prod_storage
}
```

Terraform updates the state mapping without recreating the Storage Account.

---

## Q50. Does using a `moved` block impact existing infrastructure?

### Interview Ready Answer

No.

The `moved` block only updates the resource address in the Terraform state file.

The actual cloud resource remains unchanged.

As long as the configuration remains compatible, Terraform will not destroy or recreate the resource.

This makes the `moved` block a safe way to refactor Terraform code while preserving existing infrastructure.

> **Interview Tip:** Emphasize that the `moved` block modifies the **state mapping**, not the actual infrastructure. This distinction is often what interviewers are looking for.


# Terraform Commands, Scenario-Based Questions & Azure DevOps Basics

---

# Terraform Commands

---

## Q51. What does `terraform init` do?

### Interview Ready Answer

`terraform init` is the first command executed in any Terraform project. It initializes the working directory and prepares it for Terraform operations.

During initialization, Terraform:

- Downloads the required Provider plugins.
- Downloads Child Modules.
- Configures the Backend.
- Creates the `.terraform` directory.
- Generates or updates the `.terraform.lock.hcl` file.
- Verifies Provider versions.

It does **not** create or modify infrastructure.

**Typical execution flow:**

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```

> **Interview Tip:** Mention that `terraform init` is **safe to run multiple times**. Terraform only downloads new Providers or Modules if required.

---

## Q52. During which stage does Terraform download Providers and Modules?

### Interview Ready Answer

Terraform downloads Providers and Child Modules during the **`terraform init`** stage.

Specifically:

- **Providers** are downloaded from the Terraform Registry or configured source.
- **Modules** are downloaded from:
  - Terraform Registry
  - Git repositories
  - Local paths
  - Private registries

The downloaded files are stored in the `.terraform` directory.

---

## Q53. After which command is the State File created?

### Interview Ready Answer

The Terraform State File is created after the first successful execution of:

```bash
terraform apply
```

If a remote backend is configured, the state file is created directly in the remote backend instead of the local machine.

> **Interview Tip:** `terraform plan` **does not** create the state file.

---

## Q54. Explain the following commands.

### `terraform plan`

#### Interview Ready Answer

`terraform plan` compares the current infrastructure with the desired configuration and generates an execution plan showing:

- Resources to be created
- Resources to be modified
- Resources to be destroyed

It **does not** make any changes to infrastructure.

---

### `terraform apply`

#### Interview Ready Answer

`terraform apply` executes the plan and provisions, updates, or deletes infrastructure to match the desired configuration.

After a successful apply:

- Infrastructure is updated.
- The Terraform State File is created or updated.

---

### `terraform destroy`

#### Interview Ready Answer

`terraform destroy` removes all infrastructure managed by the current Terraform configuration.

Terraform consults the State File to identify and delete the managed resources.

---

## Q55. What does `terraform show` do?

### Interview Ready Answer

`terraform show` displays the current Terraform State in a human-readable format.

It can display:

- Current infrastructure
- Resource attributes
- Outputs
- State information

It can also display a saved Plan file.

Example:

```bash
terraform show
```

To display a plan file:

```bash
terraform show tfplan
```

---

## Q56. How do you generate Terraform output in JSON format?

### Interview Ready Answer

Terraform provides JSON output using:

### For Outputs

```bash
terraform output -json
```

### For State

```bash
terraform show -json
```

These commands are commonly used in CI/CD pipelines to integrate Terraform outputs with other tools.

---

## Q57. What does `terraform fmt` do?

### Interview Ready Answer

`terraform fmt` automatically formats Terraform configuration files according to HashiCorp's standard formatting conventions.

It improves:

- Readability
- Consistency
- Code quality

Example:

```bash
terraform fmt
```

In enterprise environments, it is typically executed as part of CI validation before running `terraform validate` or `terraform plan`.

---

## Q58. What logging levels are supported by Terraform?

### Interview Ready Answer

Terraform logging is controlled using the `TF_LOG` environment variable.

Common log levels include:

- `TRACE` – Most detailed logging
- `DEBUG` – Debugging information
- `INFO` – General operational messages
- `WARN` – Warning messages
- `ERROR` – Error messages
- `OFF` – Logging disabled (default)

Example:

```bash
export TF_LOG=DEBUG
```

For production troubleshooting, `INFO` or `DEBUG` are commonly used, while `TRACE` is reserved for deep diagnostics.

---

## Q59. What is TFLint?

### Interview Ready Answer

TFLint is a static analysis tool for Terraform that helps identify potential issues before deployment.

It can detect:

- Incorrect resource configurations
- Deprecated syntax
- Provider-specific best practice violations
- Unused declarations
- Invalid resource arguments

TFLint improves code quality and is commonly integrated into CI/CD pipelines before `terraform plan`.

---

## Q60. What is Terraform Linting?

### Interview Ready Answer

Terraform Linting is the process of analyzing Terraform code for syntax errors, style issues, security concerns, and best practice violations before deployment.

Common linting tools include:

- TFLint
- Checkov
- tfsec

Benefits:

- Improves code quality
- Detects errors early
- Enforces coding standards
- Enhances security

In enterprise projects, linting is usually part of the CI pipeline.

---

# Scenario-Based Questions

---

## Q61. If you install Microsoft Office manually on a VM created by Terraform and run `terraform apply` again, what will happen?

### Interview Ready Answer

In most cases, **Microsoft Office will remain installed**.

Terraform only manages the resources and attributes defined in its configuration and tracked in the State File. Since the manual software installation is not part of the Terraform configuration, `terraform apply` does not remove it.

However, if the VM is replaced due to a configuration change that requires recreation (for example, changing the VM size or image in a way that forces replacement), the new VM will not include the manually installed software unless the installation is automated.

> **Interview Tip:** Emphasize that Terraform manages infrastructure, not manual OS-level changes, unless those changes cause the resource to be recreated.

---

## Q62. If a VM is created using a custom image and the image is later updated, what happens after running `terraform apply`?

### Interview Ready Answer

Nothing happens automatically.

Terraform compares the configuration and the State File. If the image reference in the Terraform configuration has **not changed**, `terraform apply` detects no changes and the VM is left untouched.

If the configuration is updated to reference a **new image version**, Terraform evaluates whether the image change requires resource replacement. In most Azure VM scenarios, changing the image causes the VM to be recreated.

---

## Q63. How would you deploy 10 Storage Accounts across 10 Azure subscriptions using a single Azure DevOps pipeline?

### Interview Ready Answer

I would use a parameterized YAML pipeline combined with reusable Terraform modules.

A common approach is:

1. Store the list of subscriptions in a parameter file or variable group.
2. Create a Service Connection (or Workload Identity Federation) for each subscription.
3. Use a pipeline matrix or loop to iterate through each subscription.
4. Pass the subscription ID and Service Connection to the Terraform module.
5. Use `for_each` within the module to deploy Storage Accounts.
6. Store Terraform State separately for each subscription using different backend keys.

This approach keeps the pipeline reusable, scalable, and suitable for enterprise environments.

---

## Q64. How would you manage 1,500 Azure subscriptions using Azure DevOps?

### Interview Ready Answer

Managing a large number of subscriptions requires automation and standardization.

My approach would be:

- Use reusable Terraform modules.
- Use parameterized YAML pipelines.
- Use separate remote backend state files for each subscription.
- Authenticate using Workload Identity Federation or Managed Identity.
- Organize subscriptions using Management Groups.
- Store configuration in variable groups or configuration repositories.
- Automate deployments using pipeline templates and matrix strategies.
- Apply Azure Policy and RBAC consistently.

This minimizes code duplication and allows centralized management at scale.

---

## Q65. If a team member accidentally commits a password to Git, how would you remove it completely from the repository?

### Interview Ready Answer

The first step is to **rotate the exposed secret immediately**, as it should be considered compromised.

Then:

1. Remove the secret from the source code.
2. Rewrite the Git history using a tool such as **git-filter-repo** or **BFG Repo-Cleaner** to remove the secret from all commits.
3. Force-push the cleaned history (after coordinating with the team).
4. Invalidate or regenerate the exposed credential.
5. Store secrets in Azure Key Vault instead of hardcoding them.
6. Enable secret scanning and pre-commit checks to prevent future occurrences.

> **Interview Tip:** Always mention **rotating the secret first**. Cleaning the repository alone is not sufficient.

---

## Q66. As a Solution Architect, how would you design infrastructure deployment for a new customer?

### Interview Ready Answer

I would follow a modular, secure, and scalable Infrastructure as Code approach.

Key design considerations include:

- Gather functional and non-functional requirements.
- Design the Azure landing zone.
- Organize subscriptions using Management Groups.
- Define RBAC and security policies.
- Build reusable Terraform modules.
- Store Terraform State in a secure remote backend.
- Implement CI/CD with Azure DevOps.
- Use separate environments (Dev, QA, UAT, Prod).
- Integrate Azure Key Vault for secrets.
- Enable monitoring, logging, and backup.
- Enforce governance through Azure Policy.
- Perform code reviews and automated testing.

The goal is to create a secure, maintainable, and enterprise-ready deployment platform.

---

# Azure DevOps

---

# Basics

---

## Q67. What is Azure DevOps?

### Interview Ready Answer

Azure DevOps is Microsoft's DevOps platform that provides services for planning, source control, CI/CD, testing, and artifact management.

It enables teams to automate software delivery and infrastructure deployments while supporting Agile methodologies and collaboration.

It integrates seamlessly with Azure, GitHub, Terraform, Docker, Kubernetes, and many third-party tools.

---

## Q68. What are the main services available in Azure DevOps?

### Interview Ready Answer

Azure DevOps consists of five core services:

1. **Azure Repos** – Git repositories for source code management.
2. **Azure Pipelines** – CI/CD automation for applications and infrastructure.
3. **Azure Boards** – Agile planning, work items, and sprint management.
4. **Azure Test Plans** – Manual and exploratory testing.
5. **Azure Artifacts** – Package management for NuGet, npm, Maven, Python, and Universal Packages.

In most DevOps projects, Azure Repos, Pipelines, and Boards are the most frequently used services.

---

## Q69. What is a Stakeholder in Azure DevOps?

### Interview Ready Answer

A Stakeholder is a user role in Azure DevOps with limited access.

Stakeholders can:

- View work items.
- Create and update work items.
- Track project progress.
- Access dashboards.

They cannot perform advanced development tasks such as managing repositories or creating pipelines unless granted additional permissions.

This role is commonly assigned to Product Owners, Business Analysts, or Project Managers.

---

# Azure Boards

---

## Q70. What Agile methodology do you follow?

### Interview Ready Answer

Our team follows the **Scrum** framework.

We work in fixed-length sprints, maintain a prioritized product backlog, and conduct:

- Sprint Planning
- Daily Stand-up Meetings
- Sprint Review
- Sprint Retrospective

Work is tracked using Azure Boards, where each requirement is managed as a Work Item.

---

## Q71. What is a Sprint?

### Interview Ready Answer

A Sprint is a fixed-duration iteration in Scrum, typically lasting **2 to 3 weeks**, during which the team commits to completing a set of planned work items.

The objective is to deliver a potentially shippable increment of the product by the end of the sprint.

---

## Q72. What is a Work Item?

### Interview Ready Answer

A Work Item represents a unit of work tracked in Azure Boards.

Common Work Item types include:

- Epic
- Feature
- User Story
- Task
- Bug
- Issue

Each Work Item contains details such as description, priority, assignee, state, and acceptance criteria.

---

## Q73. What is the difference between a Sprint and a Work Item?

### Interview Ready Answer

| Sprint | Work Item |
|--------|-----------|
| A fixed time period for delivering work | A single task or requirement |
| Usually lasts 2–3 weeks | Represents a User Story, Task, Bug, etc. |
| Contains multiple Work Items | Assigned to a Sprint |
| Used for planning and tracking progress | Used to manage individual pieces of work |

**Simple explanation:**

- A **Sprint** is a time-box.
- A **Work Item** is the actual work completed within that time-box.



# Azure DevOps Pipelines, Service Connections, Git & GitHub, Microsoft Azure

---

# Azure DevOps Pipelines

---

## Q74. Explain the complete end-to-end YAML Pipeline flow.

### Interview Ready Answer

In my current project, we use a multi-stage YAML pipeline for Infrastructure as Code deployments. The typical flow is:

1. **Developer creates a feature branch** and makes code changes.
2. **A Pull Request (PR)** is raised to the main branch.
3. **PR Validation Pipeline** is triggered:
   - `terraform fmt`
   - `terraform validate`
   - TFLint
   - Security scans (e.g., Checkov/tfsec)
4. After approval, the PR is merged into the main branch.
5. **CI Pipeline** runs:
   - Checkout code
   - Install Terraform
   - `terraform init`
   - `terraform plan`
6. For Production, a **manual approval** is required.
7. **CD Stage** executes:
   - `terraform apply`
8. Post-deployment validation and smoke tests are performed.
9. Notifications are sent to Microsoft Teams or Email.

**Typical Pipeline Flow**

```
Developer
     │
     ▼
Pull Request
     │
     ▼
PR Validation
(fmt, validate, lint)
     │
     ▼
Merge
     │
     ▼
CI Pipeline
(terraform init)
(terraform plan)
     │
     ▼
Approval
     │
     ▼
CD Pipeline
(terraform apply)
     │
     ▼
Validation & Notification
```

> **Interview Tip:** Mention that your pipelines are **multi-stage, YAML-based, reusable, and parameterized**.

---

## Q75. Which Pipeline type have you worked on?

### Interview Ready Answer

I have primarily worked with **YAML Pipelines** in Azure DevOps.

They are version-controlled, reusable, and support Infrastructure as Code practices.

I have experience with:

- CI Pipelines
- CD Pipelines
- Multi-stage Pipelines
- Pipeline Templates
- Parameterized Pipelines
- Environment-based Deployments

Although I am familiar with Classic Pipelines, all our production deployments use YAML.

---

## Q76. What is a Deployment Group?

### Interview Ready Answer

A Deployment Group is a collection of target machines where Azure DevOps deploys applications.

Each machine has a Deployment Agent installed, allowing Azure DevOps to deploy software directly to those servers.

Deployment Groups are mainly used with **Classic Release Pipelines**.

For modern YAML pipelines, **Environments** are the recommended approach.

---

## Q77. What is a Task Group?

### Interview Ready Answer

A Task Group is a reusable collection of pipeline tasks.

Instead of repeating the same tasks across multiple pipelines, they can be grouped into a Task Group and reused.

Benefits include:

- Reduced duplication
- Standardized pipelines
- Easier maintenance
- Centralized updates

---

## Q78. What is an Agent Pool?

### Interview Ready Answer

An Agent Pool is a collection of build or deployment agents that execute pipeline jobs.

Azure DevOps supports:

- Microsoft-hosted Agents
- Self-hosted Agents

Common Microsoft-hosted agents include:

- Ubuntu
- Windows
- macOS

In enterprise projects, self-hosted agents are often used for better control, security, and access to private networks.

---

## Q79. Is macOS supported as an Azure DevOps Agent?

### Interview Ready Answer

Yes.

Azure DevOps supports **macOS** as both:

- Microsoft-hosted Agent
- Self-hosted Agent

macOS agents are commonly used for building and testing iOS and macOS applications.

---

## Q80. What are Deployment Gates?

### Interview Ready Answer

Deployment Gates are automated validation checks performed before or after a deployment stage.

They help ensure that predefined conditions are met before promoting the deployment.

Examples include:

- Manual Approval
- Azure Monitor Alerts
- REST API Checks
- Azure Policy Validation
- Business Hours Check
- Work Item Query
- Custom Validation Scripts

Deployment Gates improve deployment quality and reduce production risks.

---

## Q81. What are the security best practices for Azure DevOps Pipelines?

### Interview Ready Answer

Key security best practices include:

- Use Workload Identity Federation instead of Service Principal secrets.
- Store secrets in Azure Key Vault.
- Use Variable Groups for configuration.
- Apply least privilege (RBAC).
- Enable branch protection and mandatory PR reviews.
- Require approvals before Production deployments.
- Use Microsoft-hosted or hardened self-hosted agents.
- Enable pipeline permissions and environment approvals.
- Avoid hardcoding credentials.
- Rotate secrets regularly.
- Integrate security scanning (Checkov, tfsec, SonarQube).

> **Interview Tip:** Mention **Azure Key Vault**, **RBAC**, **Branch Policies**, and **Workload Identity Federation**.

---

## Q82. Which tools have you integrated into your CI/CD pipeline?

### Interview Ready Answer

In my projects, I have integrated:

- Terraform
- Azure CLI
- TFLint
- Checkov
- tfsec
- SonarQube
- Docker
- Kubernetes (AKS)
- Helm
- Azure Key Vault
- PowerShell
- Bash
- Git
- GitHub
- Microsoft Teams Notifications

---

# Service Connections

---

## Q83. What is a Service Connection?

### Interview Ready Answer

A Service Connection is an authentication mechanism that allows Azure DevOps to securely connect to external services such as Azure, GitHub, Docker Hub, Kubernetes, or AWS.

It enables pipelines to access and manage external resources without embedding credentials in the pipeline code.

---

## Q84. What information is required to create a Service Connection?

### Interview Ready Answer

For an Azure Resource Manager (ARM) Service Connection, the required information includes:

- Azure Subscription
- Subscription ID
- Tenant ID
- Authentication Method (Workload Identity Federation or Service Principal)
- Appropriate RBAC permissions
- Service Connection Name

For production environments, Workload Identity Federation is the recommended authentication method.

---

## Q85. How does Workload Identity Federation work in Azure DevOps?

### Interview Ready Answer

Workload Identity Federation enables Azure DevOps to authenticate to Azure **without storing client secrets**.

The process is:

1. Azure DevOps requests an OpenID Connect (OIDC) token.
2. Azure AD validates the token.
3. Azure AD issues a short-lived access token.
4. The pipeline uses this token to access Azure resources.

Benefits:

- No client secrets.
- Improved security.
- Automatic token rotation.
- Reduced credential management.

> **Interview Tip:** Mention **OIDC**, **short-lived tokens**, and **secretless authentication**.

---

## Q86. Which authentication method would you use to integrate Snowflake with Azure DevOps?

### Interview Ready Answer

The preferred approach is **Workload Identity Federation (OIDC)** if supported.

If OIDC is not available, I would use:

- Azure Key Vault to securely store credentials.
- A Service Connection or Service Principal with least privilege.

Hardcoded usernames and passwords should never be used in production pipelines.

---

# Pipeline Scenarios

---

## Q87. A multi-stage pipeline has no dependencies, but only one stage executes. How would you troubleshoot the issue?

### Interview Ready Answer

I would troubleshoot in the following order:

1. Check `condition` statements on each stage.
2. Verify stage triggers.
3. Review runtime parameters and variables.
4. Confirm environment approvals or deployment gates.
5. Check agent availability.
6. Verify pipeline permissions.
7. Review logs for skipped stages.
8. Validate YAML syntax.

Most issues are caused by incorrect conditions, approvals, or environment configurations.

---

## Q88. How would you configure automatic Pull Request creation in Azure DevOps?

### Interview Ready Answer

Azure DevOps does not provide a built-in feature for automatically creating Pull Requests.

A common solution is:

- Use Azure DevOps REST API or Azure CLI.
- Trigger a pipeline when code is pushed.
- Use a PowerShell or Bash script to call the REST API and create the Pull Request automatically.

Branch Policies can then enforce mandatory reviewers, builds, and approvals.

---

## Q89. How would you restrict a user so they can access only Repositories and Pipelines, but not Azure Boards?

### Interview Ready Answer

I would configure permissions at the project level:

1. Add the user to a custom security group.
2. Grant access to:
   - Azure Repos
   - Azure Pipelines
3. Remove or deny permissions for Azure Boards.
4. Configure RBAC using Azure DevOps security settings.

This follows the principle of least privilege.

---

# Git & GitHub

---

## Q90. What is the difference between Git Commit and Git Push?

### Interview Ready Answer

| Git Commit | Git Push |
|------------|----------|
| Saves changes to the local repository | Uploads commits to the remote repository |
| Local operation | Remote operation |
| Creates a commit in local history | Shares commits with the team |

**Simple explanation:**

- **Commit = Save locally**
- **Push = Upload to remote repository**

---

## Q91. What is the difference between Git Fetch and Git Pull?

### Interview Ready Answer

| Git Fetch | Git Pull |
|------------|----------|
| Downloads changes only | Downloads and merges changes |
| Does not modify the working branch | Updates the current branch |
| Safe for reviewing changes | Immediately integrates changes |

**Simple explanation:**

- **Fetch = Download**
- **Pull = Download + Merge**

---

## Q92. Explain the Pull Request (PR) workflow.

### Interview Ready Answer

The standard Pull Request workflow is:

1. Create a feature branch.
2. Develop and commit changes.
3. Push the branch to the remote repository.
4. Raise a Pull Request.
5. Code Review is performed.
6. CI validation pipeline executes.
7. Reviewers approve the PR.
8. Merge into the main branch.
9. CI/CD pipeline deploys the changes.

This process ensures code quality and collaboration.

---

## Q93. What are GitHub Actions?

### Interview Ready Answer

GitHub Actions is GitHub's CI/CD platform that automates software build, test, and deployment workflows.

Workflows are defined in YAML files located under:

```text
.github/workflows/
```

Common use cases include:

- Build automation
- Testing
- Terraform deployments
- Docker image builds
- Kubernetes deployments

---

## Q94. What is a Workflow Dispatch event?

### Interview Ready Answer

`workflow_dispatch` is a manual trigger for GitHub Actions.

It allows users to start a workflow on demand from the GitHub UI or via the GitHub API.

Example:

```yaml
on:
  workflow_dispatch:
```

It is commonly used for production deployments and ad-hoc infrastructure changes.

---

## Q95. Explain the basic syntax of a GitHub Actions workflow.

### Interview Ready Answer

A GitHub Actions workflow consists of:

- Name
- Trigger (`on`)
- Jobs
- Runs-on
- Steps

Example:

```yaml
name: Terraform Pipeline

on:
  push:
    branches:
      - main

jobs:
  terraform:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - name: Terraform Init
        run: terraform init
```

---

# Microsoft Azure

---

## Q96. What is the difference between a Virtual Machine (VM) and a Virtual Machine Scale Set (VMSS)?

### Interview Ready Answer

| Virtual Machine (VM) | Virtual Machine Scale Set (VMSS) |
|-----------------------|----------------------------------|
| Single virtual machine | Group of identical virtual machines |
| Manual scaling | Automatic scaling |
| Best for standalone workloads | Best for highly available applications |
| Individual management | Centralized management |

---

## Q97. When would you choose a VM over a VMSS?

### Interview Ready Answer

I would choose a standalone VM when:

- Only one server is required.
- The workload is stateful.
- The application requires unique configuration.
- Manual administration is acceptable.

Examples:

- Domain Controller
- Database Server
- Bastion Host
- Jump Server

---

## Q98. What is the difference between an Availability Set and an Availability Zone?

### Interview Ready Answer

| Availability Set | Availability Zone |
|------------------|-------------------|
| Protects against hardware failures within a datacenter | Protects against datacenter failures |
| Resources remain in the same datacenter | Resources are distributed across separate datacenters |
| Lower level of fault isolation | Higher availability and resilience |

Availability Zones provide greater fault tolerance than Availability Sets.

---

## Q99. What is Virtual Network Peering?

### Interview Ready Answer

Virtual Network Peering connects two Azure Virtual Networks, allowing them to communicate over the Microsoft backbone network.

Benefits include:

- Low latency
- High bandwidth
- No VPN Gateway required
- Secure private communication

It is commonly used to connect Hub-and-Spoke network architectures.

---

## Q100. How can you block all outbound traffic from an Azure VM without using an NSG?

### Interview Ready Answer

If an NSG cannot be used, outbound traffic can be controlled by:

1. **Azure Firewall** – Apply network or application rules to deny outbound traffic.
2. **User Defined Routes (UDRs)** – Route all outbound traffic to Azure Firewall or a Network Virtual Appliance (NVA), where it can be blocked or filtered.

In enterprise environments, the preferred approach is to use **Azure Firewall with UDRs**, as it provides centralized control, logging, and policy enforcement.


# Kubernetes, Docker, Azure Architecture & Miscellaneous

---

# 6. Kubernetes

---

## Q101. What is the difference between Stateless and Stateful applications?

### Interview Ready Answer

| Stateless Application | Stateful Application |
|------------------------|----------------------|
| Does not store client/session data | Stores persistent data |
| Any replica can serve any request | Each replica has its own identity and storage |
| Easy to scale horizontally | Scaling requires persistent storage management |
| Uses Deployments | Usually uses StatefulSets |
| Example: Nginx, Web APIs | Example: MySQL, PostgreSQL, MongoDB |

**Simple explanation:**

- **Stateless** → No data is stored between requests.
- **Stateful** → Data is stored and must persist even if the Pod restarts.

---

## Q102. How would you troubleshoot a Pod stuck in the **Pending** state?

### Interview Ready Answer

I follow a structured troubleshooting approach:

1. Check the Pod status.

```bash
kubectl get pods
```

2. Describe the Pod.

```bash
kubectl describe pod <pod-name>
```

3. Check Events for scheduling errors.

Common causes include:

- Insufficient CPU or Memory
- No available Nodes
- Taints without matching Tolerations
- Node Selector mismatch
- PVC not bound
- Resource Quotas exceeded
- Image Pull Secret issues

4. Check Node status.

```bash
kubectl get nodes
```

5. Verify Persistent Volume Claims.

```bash
kubectl get pvc
```

6. Check Scheduler logs if required.

> **Interview Tip:** Mention **`kubectl describe pod`** first. Most scheduling issues can be identified from the Events section.

---

## Q103. What is a Persistent Volume (PV)?

### Interview Ready Answer

A Persistent Volume (PV) is a cluster-wide storage resource in Kubernetes that provides persistent storage independent of the Pod lifecycle.

Key points:

- Created by an Administrator or dynamically provisioned.
- Exists independently of Pods.
- Can be reused after Pod deletion.
- Accessed through a Persistent Volume Claim (PVC).

Examples:

- Azure Disk
- Azure File Share
- NFS
- AWS EBS

---

## Q104. What is a Kubernetes Service Account?

### Interview Ready Answer

A Service Account provides an identity for Pods to authenticate with the Kubernetes API.

Instead of using a user's credentials, Pods use a Service Account when they need to:

- Access the Kubernetes API
- Read Secrets
- Manage ConfigMaps
- Interact with cluster resources

Permissions are granted using **RBAC**.

---

## Q105. What is a ClusterRoleBinding?

### Interview Ready Answer

A ClusterRoleBinding grants cluster-wide permissions by binding a **ClusterRole** to:

- A User
- A Group
- A Service Account

Unlike a RoleBinding, which applies only to a specific namespace, a ClusterRoleBinding applies across the entire Kubernetes cluster.

Example use case:

Giving a monitoring Service Account permission to read Pods in all namespaces.

---

## Q106. Write and explain a Kubernetes YAML manifest.

### Interview Ready Answer

Example Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Explanation:

- **apiVersion** → Kubernetes API version
- **kind** → Resource type
- **metadata** → Object name and labels
- **spec** → Desired state
- **replicas** → Number of Pods
- **selector** → Matches Pods
- **template** → Pod definition
- **containers** → Container configuration

---

## Q107. How do you upgrade a Kubernetes cluster?

### Interview Ready Answer

The upgrade process depends on the platform (AKS, EKS, GKE, or self-managed).

For **AKS**, the general steps are:

1. Check available Kubernetes versions.
2. Upgrade the Control Plane.
3. Upgrade the Node Pools.
4. Verify cluster health.
5. Validate application functionality.

Example:

```bash
az aks upgrade
```

Best practices:

- Take backups.
- Upgrade one minor version at a time.
- Test in a non-production environment first.
- Monitor workloads after the upgrade.

---

## Q108. How do you maintain a Kubernetes cluster?

### Interview Ready Answer

Cluster maintenance includes:

- Regular Kubernetes version upgrades.
- Patch worker nodes.
- Monitor cluster health.
- Review logs and events.
- Monitor resource usage.
- Rotate certificates before expiry.
- Backup ETCD (self-managed clusters).
- Scan container images.
- Apply RBAC and Network Policies.
- Remove unused resources.
- Enable monitoring with Prometheus/Grafana or Azure Monitor.

---

# 7. Docker

---

## Q109. What is Docker?

### Interview Ready Answer

Docker is a containerization platform used to package applications along with their dependencies into lightweight, portable containers.

Benefits:

- Consistent environments
- Fast deployment
- Lightweight compared to VMs
- Easy scalability
- Platform independent

Docker ensures the application behaves consistently across development, testing, and production.

---

## Q110. Where are Docker Images stored?

### Interview Ready Answer

Docker Images are stored in Docker Registries.

Common registries include:

- Docker Hub
- Azure Container Registry (ACR)
- Amazon ECR
- Google Artifact Registry
- GitHub Container Registry

On the local machine, Docker stores images in its local image cache.

In enterprise projects, we typically use **Azure Container Registry (ACR)**.

---

## Q111. What is a Docker Volume?

### Interview Ready Answer

A Docker Volume is a persistent storage mechanism used to retain data outside the container's writable layer.

Benefits:

- Data persists after container deletion.
- Data can be shared between containers.
- Better performance than bind mounts.
- Managed by Docker.

Common use cases:

- Databases
- Application logs
- Configuration files

---

# 8. Azure Architecture & Design

---

## Q112. How would you manage multiple Azure subscriptions?

### Interview Ready Answer

For enterprise environments, I would:

- Organize subscriptions using **Management Groups**.
- Use **Terraform Modules** for reusable infrastructure.
- Store Terraform State separately for each subscription.
- Use **Workload Identity Federation** for authentication.
- Use parameterized Azure DevOps YAML pipelines.
- Apply Azure Policies and RBAC centrally.
- Use a landing zone architecture.

This approach provides centralized governance, security, and scalability.

---

## Q113. If you need to deploy two Virtual Machines and two Databases, which architecture would you choose?

### Interview Ready Answer

It depends on the workload.

If the application requires independent servers with different configurations, I would deploy:

- **2 Azure Virtual Machines**
- **2 Azure SQL Databases** (or Managed Databases)

I would not use VM Scale Sets because there are only two servers with potentially different roles.

For production, I would also include:

- Availability Zones
- Load Balancer (if required)
- Private Endpoints
- Azure Key Vault
- Monitoring
- Backup

---

## Q114. Explain your approach to designing enterprise-scale Azure infrastructure.

### Interview Ready Answer

My approach is:

1. Gather business and technical requirements.
2. Design the Azure Landing Zone.
3. Organize subscriptions using Management Groups.
4. Apply RBAC and Azure Policy.
5. Create reusable Terraform modules.
6. Store Terraform State in Azure Storage with locking.
7. Implement CI/CD using Azure DevOps.
8. Store secrets in Azure Key Vault.
9. Enable Monitoring and Logging.
10. Design for High Availability and Disaster Recovery.
11. Implement Backup and Security controls.
12. Standardize naming conventions and tagging.

The objective is to build infrastructure that is secure, scalable, highly available, and easy to maintain.

---

# 9. Miscellaneous

---

## Q115. What is Linting?

### Interview Ready Answer

Linting is the process of analyzing source code to identify syntax errors, style violations, potential bugs, and best practice issues before execution.

Benefits:

- Improves code quality.
- Maintains coding standards.
- Detects errors early.
- Reduces deployment failures.

---

## Q116. What is TFLint?

### Interview Ready Answer

TFLint is a static analysis tool specifically designed for Terraform.

It checks:

- Invalid resource arguments
- Provider best practices
- Deprecated syntax
- Unused declarations
- Common configuration mistakes

It is typically executed before `terraform plan` as part of a CI pipeline.

---

## Q117. What is an Output Block in Terraform?

### Interview Ready Answer

An Output Block is used to expose values from Terraform after deployment.

It is commonly used to display or pass information such as:

- VM Public IP
- Storage Account Name
- Resource ID
- Connection Strings

Example:

```hcl
output "vm_public_ip" {
  value = azurerm_public_ip.vm.ip_address
}
```

Outputs are also useful for sharing values between Terraform modules or with CI/CD pipelines.

---

## Q118. How can Terraform Outputs be passed to an Azure DevOps Pipeline?

### Interview Ready Answer

Terraform outputs can be retrieved using:

```bash
terraform output
```

For automation, JSON format is preferred:

```bash
terraform output -json
```

In Azure DevOps, a script can read the output and set it as a pipeline variable using:

```bash
echo "##vso[task.setvariable variable=vmIP]$VM_IP"
```

This allows subsequent pipeline tasks to use the Terraform output.

---

## Q119. Which command displays Terraform Outputs in JSON format?

### Interview Ready Answer

The command is:

```bash
terraform output -json
```

This is commonly used in CI/CD pipelines for machine-readable output.

---

## Q120. What are Terraform Slugs?

### Interview Ready Answer

**Terraform does not have an official feature called "Terraform Slugs."**

In interviews, this term usually refers to **slugified or sanitized names** used for resource naming.

For example:

- Convert names to lowercase.
- Replace spaces with hyphens.
- Remove special characters.
- Ensure names meet Azure naming rules.

Example:

```
Production Storage Account
```

becomes:

```
production-storage-account
```

This helps create consistent and compliant resource names across environments.

> **Interview Tip:** If asked about "Terraform Slugs," clarify that Terraform has no built-in concept called a slug. It's generally a naming convention implemented using Terraform string functions or external logic.
