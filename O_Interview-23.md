# Deloitte DevSecOps Interview (1st Round) – Interview Ready Answers

---

# 1. Introduce yourself and explain your DevSecOps experience.

Hi, I'm a DevSecOps Engineer with around 10 years of experience in designing, automating, and securing cloud infrastructure. My primary expertise is in Azure, AKS, Terraform, Azure DevOps, GitHub Actions, Docker, Kubernetes, Helm, and CI/CD automation.

I have built reusable Terraform modules, managed multi-environment infrastructure, implemented GitOps practices, integrated security tools like SonarQube, Trivy, Checkov, TruffleHog, and Azure Key Vault, and automated end-to-end deployments. I have also worked on production support, infrastructure optimization, monitoring, and zero-downtime deployments.

---

# 2. How do you organize reusable Terraform modules?

I follow a modular architecture.

```
terraform/
│
├── modules/
│   ├── network
│   ├── aks
│   ├── vm
│   ├── keyvault
│   ├── storage
│   └── sql
│
└── environments/
    ├── dev
    ├── qa
    ├── stage
    └── prod
```

Each module contains:

- main.tf
- variables.tf
- outputs.tf
- versions.tf

Modules are reusable across all environments.

---

# 3. How do you structure Terraform code for Dev, QA, Stage, and Production?

I keep one reusable module and separate environment folders.

```
environments/

dev/
qa/
stage/
prod/
```

Each environment has:

- backend.tf
- terraform.tfvars
- main.tf

Only values change, module remains the same.

---

# 4. How do you deploy changes using reusable Terraform modules?

1. Modify the module if infrastructure changes.
2. Update module version if required.
3. Run:

```bash
terraform init
terraform plan
terraform apply
```

CI/CD automatically validates, plans, and applies after approval.

---

# 5. If you need to add a new VM to an existing infrastructure, what changes do you make?

I simply call the VM module again.

```hcl
module "vm2" {
  source = "../../modules/vm"

  vm_name = "appvm02"
}
```

No changes to existing infrastructure.

---

# 6. Terraform shows changes even though you didn't modify the code. Why?

Possible reasons:

- Infrastructure drift
- Provider version change
- Computed attributes
- Manual Azure changes
- State mismatch
- API updates

I verify using:

```bash
terraform plan
terraform refresh
```

---

# 7. What is Terraform Drift?

Terraform Drift occurs when actual infrastructure differs from Terraform state due to manual changes outside Terraform.

---

# 8. How do you resolve Terraform Drift?

1. Run

```bash
terraform refresh
```

or

```bash
terraform plan
```

2. Decide whether to keep manual changes.

3. Either

- Update Terraform code

OR

- Apply Terraform to restore desired state.

---

# 9. How do you prevent infrastructure drift?

- No manual production changes
- RBAC
- CI/CD only deployments
- Remote state
- Terraform plan before apply
- Policy enforcement
- Regular drift detection

---

# 10. How do you update production infrastructure without downtime?

- Rolling deployment
- Blue-Green deployment
- Canary deployment
- Multiple replicas
- Health probes
- Load Balancer
- AKS rolling updates

---

# 11. Explain create_before_destroy.

It creates the new resource first and deletes the old one after successful creation.

```hcl
lifecycle {
  create_before_destroy = true
}
```

Used to avoid downtime.

---

# 12. Explain depends_on meta-argument.

It creates explicit dependency.

Example:

```hcl
depends_on = [
  azurerm_resource_group.rg
]
```

Terraform waits until dependency is completed.

---

# 13. How do you manage multiple environments in Terraform?

I use:

- Separate backend
- Separate tfvars
- Separate state files
- Separate pipelines
- Same reusable modules

---

# 14. How do you migrate Terraform local state to a remote backend?

Configure backend.

```hcl
terraform {
 backend "azurerm" {}
}
```

Then

```bash
terraform init
```

Terraform asks to migrate state.

Choose

```
yes
```

---

# 15. Which Kubernetes platform are you currently working on?

Currently I work on Azure Kubernetes Service (AKS).

I manage:

- AKS clusters
- Node Pools
- Ingress
- Helm
- Autoscaling
- Monitoring
- Azure CNI
- Azure Key Vault CSI Driver

---

# 16. What are the components of an AKS cluster?

- Control Plane (Managed by Azure)
- Node Pool
- Pods
- ReplicaSets
- Deployments
- Services
- Ingress
- CoreDNS
- kube-proxy
- etcd
- Azure Load Balancer

---

# 17. What is the difference between Load Balancer and Ingress?

| Load Balancer | Ingress |
|--------------|---------|
| Layer 4 | Layer 7 |
| Exposes one service | Routes multiple services |
| IP based | Host/Path based |
| TCP/UDP | HTTP/HTTPS |
| No SSL termination | SSL termination supported |

---

# 18. How do you connect multiple Linux VMs inside a VNet?

- Same VNet
- Same/Subnets
- NSG rules
- Private IP communication
- Internal DNS

No Public IP required.

---

# 19. How many VMs can be connected using a single Public IP?

One Azure Load Balancer Public IP can serve multiple backend VMs.

Direct Public IP assignment is typically one Public IP per NIC.

---

# 20. How do you connect two VNets?

Using

- VNet Peering

If cross-region

- Global VNet Peering

Hybrid

- VPN Gateway
- ExpressRoute

---

# 21. How do you create an AKS cluster using Terraform?

Create resources:

- Resource Group
- VNet
- Subnet
- AKS Cluster
- Node Pool
- Managed Identity
- Azure AD Integration

Deploy using

```bash
terraform apply
```

---

# 22. How do you connect to an AKS cluster?

```bash
az login

az aks get-credentials \
--resource-group rg-demo \
--name aks-demo

kubectl get nodes
```

---

# 23. Draw and explain your Terraform reusable module structure for AKS.

```
terraform/

modules/

    aks/

        main.tf
        variables.tf
        outputs.tf

    network/

    keyvault/

    acr/

environments/

    dev/

    qa/

    stage/

    prod/
```

Each environment calls the same AKS module with different variables.

---

# 24. Explain your Terraform project folder structure.

```
terraform/

modules/

    aks

    vm

    network

    sql

    keyvault

environments/

    dev

    qa

    stage

    prod

pipelines/

scripts/

README.md
```

---

# 25. What is SAST?

SAST (Static Application Security Testing) scans source code without executing it to detect security vulnerabilities early in development.

---

# 26. Why do you perform SAST in the CI pipeline?

- Shift Left Security
- Early vulnerability detection
- Faster fixes
- Secure code before build
- Compliance

---

# 27. How does SonarQube help in code quality?

SonarQube checks:

- Bugs
- Vulnerabilities
- Code Smells
- Duplicates
- Coverage
- Quality Gates

Pipeline fails if Quality Gate fails.

---

# 28. How do you scan a Docker image?

I use Trivy.

```bash
trivy image myapp:v1
```

Sometimes Microsoft Defender and Docker Scout as well.

---

# 29. What secret management solutions have you used?

- Azure Key Vault
- Kubernetes Secrets
- Azure Managed Identity
- AKS CSI Driver
- Azure DevOps Variable Groups

Primary solution is Azure Key Vault.

---

# 30. How does TruffleHog work?

TruffleHog scans Git repositories and commits for exposed secrets like:

- API Keys
- Passwords
- Tokens
- AWS Keys
- Azure Keys
- Private Keys

It detects both verified and high-entropy secrets.

---

# 31. A critical vulnerability is found in a production container image. What steps will you take?

1. Confirm vulnerability severity.
2. Identify affected services.
3. Stop new deployments.
4. Update vulnerable package/base image.
5. Rebuild image.
6. Scan using Trivy.
7. Push to ACR.
8. Deploy through CI/CD.
9. Verify production.
10. Monitor logs and rollback if required.

---

# 32. How do you implement security in Terraform-based AKS deployments?

- Private AKS
- Azure RBAC
- Managed Identity
- Network Policies
- NSGs
- Azure Key Vault
- Secrets Store CSI Driver
- Azure Policy
- Defender for Cloud
- Encryption at Rest
- Least Privilege
- Private ACR

---

# 33. How do you integrate Azure Key Vault with your CI/CD pipeline?

- Store secrets in Azure Key Vault.
- Grant pipeline Managed Identity or Service Principal access.
- Fetch secrets during pipeline execution.
- Pass secrets as secure variables.
- Never store secrets in Git.

---

# 34. How do you securely access secrets in AKS?

I use Azure Key Vault with the Secrets Store CSI Driver and Managed Identity. Pods retrieve secrets directly from Key Vault at runtime without storing them in container images or source code.

---

# 35. Do you have any questions for us?

- How is the DevSecOps team structured?
- What CI/CD and GitOps tools are currently used?
- What are the biggest infrastructure or security challenges the team is working on?
- Is there an opportunity to work on platform engineering and cloud architecture initiatives?
