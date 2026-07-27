# KPMG DevSecOps Interview (24th July) – Interview Ready Answers
---

# 1. Introduce yourself.

Hi, I'm a DevSecOps Engineer with around 10 years of experience in Cloud Infrastructure, DevOps, and Security Automation. My expertise includes Azure, AWS, AKS, Terraform, Kubernetes, GitHub Actions, Azure DevOps, Docker, Helm, and CI/CD pipelines. I have designed secure cloud landing zones, implemented Infrastructure as Code, automated deployments, integrated security scanning into CI/CD, managed Kubernetes platforms, and worked on cloud security, governance, and compliance for enterprise environments.

---

# 2. What is an Azure Landing Zone?

An Azure Landing Zone is a pre-configured, scalable, and secure cloud environment that provides a standardized foundation for deploying workloads. It includes identity, networking, security, governance, monitoring, policies, and management based on Azure Cloud Adoption Framework (CAF).

---

# 3. What are the best practices for designing an Azure Landing Zone?

- Follow Azure Cloud Adoption Framework (CAF)
- Use Management Groups and Subscriptions
- Implement Hub-and-Spoke networking
- Centralize identity with Azure Entra ID
- Apply Azure Policy and RBAC
- Use Azure Key Vault for secrets
- Enable Microsoft Defender for Cloud
- Centralize monitoring with Azure Monitor and Log Analytics
- Automate deployments using Terraform
- Separate Production and Non-Production environments

---

# 4. Explain the Hub-and-Spoke network architecture.

Hub-and-Spoke is a centralized networking model where shared services are deployed in the Hub VNet, and application workloads are deployed in separate Spoke VNets. All spokes communicate through the Hub, improving security, governance, and scalability.

---

# 5. Which resources are typically deployed in the Hub network?

- Azure Firewall
- VPN Gateway
- ExpressRoute Gateway
- Bastion Host
- DNS Forwarder
- Azure Firewall Manager
- Network Virtual Appliances (NVA)
- Shared Monitoring Services
- Log Analytics Workspace

---

# 6. What is the best networking strategy for a large enterprise with multiple teams?

Use Hub-and-Spoke architecture with Azure Virtual WAN for global connectivity. Each team gets its own subscription and spoke VNet while shared services remain centralized in the Hub.

---

# 7. How does communication happen between Hub and Spoke networks?

Communication occurs through VNet Peering. Traffic flows from the Spoke to the Hub, where Azure Firewall or NVAs inspect and route the traffic.

---

# 8. How do you enable Spoke-to-Spoke communication?

- Route traffic through the Hub using User Defined Routes (UDRs)
- Enable VNet Peering between Hub and all Spokes
- Configure Azure Firewall or NVA to forward traffic
- Enable "Allow Forwarded Traffic" in VNet Peering

---

# 9. Why do we use User Defined Routes (UDRs)?

UDRs override Azure's default routing to control traffic flow. They are used to route traffic through Azure Firewall, VPN Gateway, or Network Virtual Appliances for inspection and security.

---

# 10. What are the limitations of VNet Peering in large-scale environments?

- Complex management with many VNets
- No centralized routing
- Manual peering configuration
- Scaling becomes difficult
- Mesh topology increases operational overhead

---

# 11. How do you overcome VNet Peering limitations?

- Azure Virtual WAN
- Azure Route Server
- Azure Firewall
- Transit architecture
- Infrastructure as Code using Terraform

---

# 12. What is Azure Virtual WAN and when would you use it?

Azure Virtual WAN is a managed networking service that provides centralized connectivity between Azure VNets, branches, remote users, and on-premises networks. It is ideal for large enterprises with multiple regions and hybrid cloud environments.

---

# 13. How do you connect Azure with On-Premises and other cloud providers?

- Site-to-Site VPN
- ExpressRoute
- Azure Virtual WAN
- IPSec VPN
- AWS Direct Connect with ExpressRoute
- SD-WAN solutions

---

# 14. What is AWS Transit Gateway and how is it different from VPC Peering?

| AWS Transit Gateway | VPC Peering |
|---------------------|-------------|
| Centralized routing | Direct connection |
| Supports many VPCs | One-to-one connection |
| Scalable | Limited scalability |
| Transit architecture | No transit routing |
| Easier management | Complex at scale |

---

# 15. Explain your Terraform workflow.

1. Write Terraform code
2. Initialize providers (`terraform init`)
3. Validate configuration (`terraform validate`)
4. Format code (`terraform fmt`)
5. Review changes (`terraform plan`)
6. Apply changes (`terraform apply`)
7. Store state remotely
8. Deploy through CI/CD pipeline

---

# 16. What are the main components of a GitHub Actions workflow?

- Workflow (.github/workflows)
- Events (push, pull_request, workflow_dispatch)
- Jobs
- Runners
- Steps
- Actions
- Secrets
- Environment Variables

---

# 17. How do you manage Terraform State Files?

- Store remotely in Azure Storage Account
- Enable state locking
- Enable versioning
- Encrypt at rest
- Restrict access using RBAC
- Separate state files per environment

---

# 18. What is the best strategy for storing Terraform State remotely?

Use Azure Storage Account with:
- Blob backend
- State locking
- Versioning
- Encryption
- Private Endpoint
- RBAC
- Separate storage containers for each environment

---

# 19. What is SIEM?

SIEM (Security Information and Event Management) collects, correlates, analyzes, and monitors security logs from multiple systems to detect threats and support incident response.

---

# 20. What is SOAR?

SOAR (Security Orchestration, Automation, and Response) automates security workflows, incident response, and integrates multiple security tools to reduce manual effort.

---

# 21. Have you worked with any SIEM or SOAR platforms?

Yes.

SIEM:
- Microsoft Sentinel
- Splunk

SOAR:
- Microsoft Sentinel Automation Rules
- Logic Apps

---

# 22. What is Cloud Security Posture Management (CSPM)?

CSPM continuously monitors cloud resources for misconfigurations, compliance violations, and security risks, providing recommendations for remediation.

---

# 23. Which CSPM tools have you worked with?

- Microsoft Defender for Cloud
- Prisma Cloud
- AWS Security Hub
- Azure Policy

---

# 24. Have you used Wiz or similar posture management tools?

Yes. I have primarily worked with Microsoft Defender for Cloud and Prisma Cloud. I understand Wiz's agentless approach for identifying cloud risks, attack paths, exposed assets, and compliance issues.

---

# 25. Have you worked with Zscaler?

Yes, I have experience integrating Zscaler Internet Access (ZIA) for secure internet access and Zero Trust Network Access (ZTNA). I have worked with traffic forwarding, policy enforcement, and secure connectivity between users and cloud applications.

---

# 26. How do you perform vulnerability management for Virtual Machines?

- Continuous vulnerability scanning
- Patch management
- Microsoft Defender for Servers
- Qualys/Tenable scanning
- Remediation based on CVSS score
- Verify after patch deployment

---

# 27. Which tools do you use for vulnerability scanning?

- Microsoft Defender for Cloud
- Qualys
- Tenable Nessus
- Trivy
- Microsoft Defender Vulnerability Management

---

# 28. Have you worked with AWS Inspector?

Yes. AWS Inspector automatically scans EC2 instances, container images, and Lambda functions for vulnerabilities and provides prioritized remediation recommendations.

---

# 29. What is the Azure equivalent of AWS Inspector?

Microsoft Defender for Cloud with Microsoft Defender for Servers provides vulnerability assessment, security recommendations, compliance monitoring, and threat detection.

---

# 30. What are Pod Security Standards in Kubernetes?

Pod Security Standards define security controls for Pods.

Levels:
- Privileged
- Baseline
- Restricted

Restricted is recommended for production because it enforces least privilege.

---

# 31. How do you secure container images in Kubernetes or AKS?

- Use minimal base images
- Scan images with Trivy
- Store images in Azure Container Registry (ACR)
- Enable image signing (Cosign/Notation)
- Restrict privileged containers
- Enforce Pod Security Standards
- Use Azure Policy
- Pull only trusted images
- Regularly patch base images

---

# 32. What is Azure Entra ID (Azure AD)?

Azure Entra ID is Microsoft's cloud-based Identity and Access Management (IAM) service. It provides authentication, Single Sign-On (SSO), Multi-Factor Authentication (MFA), Conditional Access, and identity governance.

---

# 33. What is Azure Privileged Identity Management (PIM)?

Azure PIM enables Just-In-Time (JIT) privileged access to Azure resources. It reduces standing administrative privileges by requiring approval, MFA, and time-bound role activation.

---

# 34. What is DevSecOps?

DevSecOps integrates security into every phase of the software development lifecycle by automating security checks within CI/CD pipelines, ensuring secure code, infrastructure, containers, and deployments.

---

# 35. How do you integrate security into a CI/CD pipeline?

- SAST (SonarQube)
- Secret scanning (TruffleHog/Gitleaks)
- Dependency scanning
- IaC scanning (Checkov/Tfsec)
- Container image scanning (Trivy)
- DAST (where applicable)
- Quality Gates
- Signed artifacts
- Secure deployment approvals

---

# 36. What are the roles and responsibilities of a DevSecOps Engineer?

- Design secure CI/CD pipelines
- Automate Infrastructure as Code
- Integrate security tools
- Manage Kubernetes security
- Implement cloud security best practices
- Perform vulnerability management
- Manage secrets securely
- Monitor cloud infrastructure
- Ensure compliance and governance
- Support incident response and remediation

---

# 37. What questions do you have about the role?

- What cloud platforms and DevSecOps tools are primarily used?
- How is the DevSecOps team organized?
- What are the biggest security or automation challenges the team is currently addressing?
- Are there opportunities to work on cloud architecture and platform engineering initiatives?
- How do you measure success for this role during the first 6 months?
