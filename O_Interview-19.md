# Persistent Interview – Expert DevSecOps Answers

---

# DevSecOps Fundamentals

## 1. What is your understanding of DevSecOps?

DevSecOps is the practice of integrating security into every phase of the Software Development Life Cycle (SDLC). Instead of performing security testing only before production, security is automated and embedded into development, CI/CD pipelines, infrastructure provisioning, containerization, and runtime monitoring.

---

## 2. Can you explain DevSecOps in detail?

DevSecOps follows the principle of **Shift Left Security**, where security checks are performed as early as possible.

Typical flow:

```
Developer
      │
GitHub Push
      │
Static Code Analysis (SonarQube)
      │
Dependency Scan
      │
Secrets Scan
      │
Terraform Scan (tfsec)
      │
Docker Build
      │
Container Scan (Trivy)
      │
Quality Gate Validation
      │
Deploy to Azure AKS/App Service
      │
Runtime Monitoring
```

Security becomes part of automation rather than a manual activity.

---

## 3. Can you give a practical example of DevSecOps?

In my current project:

- Developers push code to GitHub.
- GitHub Actions/Jenkins triggers automatically.
- SonarQube performs static code analysis.
- tfsec scans Terraform files.
- Trivy scans Docker images.
- Secrets scanning prevents API keys from being committed.
- Pipeline fails automatically if High or Critical vulnerabilities exceed the defined threshold.
- Only validated builds are deployed to Azure.

---

## 4. What security best practices do you recommend during SDLC?

- Shift Left Security
- Least Privilege Access (RBAC)
- Secure Coding Standards
- Mandatory Code Reviews
- Branch Protection Rules
- Secrets Management (Azure Key Vault)
- Infrastructure as Code Scanning
- Dependency Scanning
- Container Image Scanning
- Security Gates in CI/CD
- Runtime Monitoring
- Regular Patch Management

---

## 5. If you had to lead a DevSecOps project, what would your approach be?

1. Understand application architecture.
2. Identify security risks.
3. Define DevSecOps strategy.
4. Automate CI/CD.
5. Integrate security scanners.
6. Implement RBAC.
7. Store secrets in Key Vault.
8. Configure Quality Gates.
9. Enable image signing.
10. Enable runtime monitoring.
11. Define security KPIs.
12. Continuously improve security posture.

---

## 6. What kind of DevSecOps work have you done in your current project?

- Integrated SonarQube
- Integrated Trivy
- Implemented tfsec scanning
- Automated Terraform deployments
- Managed Azure Key Vault
- Configured RBAC
- Implemented Branch Protection
- Configured GitHub Actions/Jenkins pipelines
- Container hardening
- Vulnerability remediation
- Security gate implementation

---

# CI/CD Pipeline

## 1. Explain your complete CI/CD pipeline.

```
Developer
    │
GitHub Push
    │
GitHub Actions/Jenkins
    │
Checkout Code
    │
SonarQube Scan
    │
Dependency Scan
    │
tfsec Scan
    │
Terraform Validate
    │
Terraform Plan
    │
Docker Build
    │
Trivy Scan
    │
Push Image to ACR
    │
Terraform Apply
    │
Deploy to AKS
    │
Smoke Test
```

---

## 2. Which security tools have you integrated?

- SonarQube
- Trivy
- tfsec
- GitHub Secret Scanning
- Dependabot
- Azure Key Vault

---

## 3. How did you integrate SonarQube, Trivy and tfsec?

Each tool is executed as a separate pipeline stage.

Example:

```
Build
↓

SonarQube

↓

tfsec

↓

Docker Build

↓

Trivy

↓

Deploy
```

Pipeline proceeds only if all security gates pass.

---

## 4. How are these tools invoked?

SonarQube

```bash
sonar-scanner
```

tfsec

```bash
tfsec .
```

Trivy

```bash
trivy image myimage:latest
```

---

## 5. How do you implement these stages?

Separate pipeline stages.

Example:

```
Checkout

↓

Build

↓

SonarQube

↓

tfsec

↓

Docker Build

↓

Trivy

↓

Deploy
```

---

# Vulnerability Management

## 1. What happens if Trivy detects vulnerabilities?

The pipeline evaluates severity.

- LOW → Continue
- MEDIUM → Continue with warning
- HIGH → Fail (based on policy)
- CRITICAL → Fail immediately

Developers receive the report and remediate before rerunning.

---

## 2. What happens when tfsec reports vulnerabilities?

Pipeline stops if critical Terraform misconfigurations are detected.

Examples:

- Public Storage Account
- Public SQL Database
- Open Security Group
- Missing Encryption
- Missing Network Rules

---

## 3. What vulnerability thresholds do you configure?

Typical production policy:

```
Critical = Block

High = Block

Medium = Warning

Low = Ignore/Report
```

---

## 4. How do you decide whether pipeline should fail?

Based on:

- Severity
- CVSS Score
- Exploit Availability
- Compliance Requirement
- Business Risk

---

## 5. How do you configure policies in Trivy?

Using:

- `.trivyignore`
- Severity flags
- Exit codes

Example:

```bash
trivy image \
--severity HIGH,CRITICAL \
--exit-code 1
```

---

## 6. Do you know Progressive Enforcement?

Yes.

Progressive Enforcement means introducing security gradually.

Example:

Phase 1

Report only

↓

Phase 2

Warn developers

↓

Phase 3

Block High vulnerabilities

↓

Phase 4

Block Medium vulnerabilities

---

# SonarQube

## 1. What command invokes SonarQube?

```bash
sonar-scanner
```

---

## 2. How is SonarQube integrated?

Using the SonarQube plugin in Jenkins or Azure DevOps/GitHub Actions, with project key, authentication token, and Quality Gate validation.

---

## 3. Repository level or branch level?

Both.

Project exists at repository level.

Analysis happens at branch level.

---

## 4. Why branch level?

To validate feature branches before merging into main and prevent introducing technical debt or vulnerabilities.

---

## 5. Is scan incremental or full?

Normally incremental.

Only changed files are analyzed efficiently.

---

## 6. Can you force full scan?

Yes.

Deleting previous analysis or triggering a fresh analysis causes a complete scan.

---

## 7. How?

Run a new analysis with clean workspace and re-index the project.

---

# Terraform Security

## 1. At what stage do you run tfsec?

During CI before Terraform Plan and Apply.

```
Checkout

↓

Terraform Validate

↓

tfsec

↓

Terraform Plan

↓

Terraform Apply
```

---

## 2. CI or CD?

CI.

Infrastructure should be validated before deployment.

---

## 3. Command?

```bash
tfsec .
```

---

## 4. How configure tfsec policies?

Using:

- `.tfsec.yml`
- Custom checks
- Severity configuration
- Ignore rules (only if justified)

---

## 5. Real example?

tfsec detected:

Azure Storage Account configured with public access.

---

## 6. Exact violation?

```
Public Blob Access Enabled
```

---

## 7. Why security risk?

Anyone on the internet could access storage data.

---

# GitHub Actions / Jenkins

## 1. Comfortable with Jenkins?

Yes.

I have created, maintained, and optimized Declarative and Scripted pipelines, managed agents, plugins, credentials, webhooks, and integrated security scanning tools.

---

## 2. Difference between GitHub Actions and Jenkins?

| GitHub Actions | Jenkins |
|---------------|----------|
| Built into GitHub | Separate CI server |
| YAML workflows | Jenkinsfile |
| GitHub-hosted runners | Master-Agent architecture |
| Minimal maintenance | Requires server maintenance |
| Easy GitHub integration | Supports many SCM tools |

---

## 3. Security scans available?

- Code Scanning
- Secret Scanning
- Dependabot
- Trivy
- tfsec
- SonarQube
- Snyk
- Checkov

---

## 4. How configure GitHub Actions?

Workflow YAML under:

```
.github/workflows/
```

---

## 5. Trigger?

```
push

pull_request

workflow_dispatch

schedule

release
```

---

# Python / Scripting

## 1. Python?

Intermediate.

Used for automation, API integration, parsing reports, and scripting.

---

## 2. Groovy?

Good.

Used for Jenkins pipelines.

---

## 3. Ruby?

Basic knowledge.

Mostly for reading and modifying existing scripts.

---

## 4. Extract last two characters?

```python
def last_two(s):
    return s[-2:]
```

---

# Current Project

## 1. Explain your project.

We built an automated Azure cloud platform where infrastructure is provisioned using Terraform, applications are deployed through GitHub Actions/Jenkins, containers run on AKS, and security is enforced using SonarQube, tfsec, and Trivy.

---

## 2. Problem being solved?

Reduce manual deployments, improve deployment consistency, enforce security, and enable faster, reliable releases.

---

## 3. Your role?

- CI/CD automation
- Infrastructure provisioning
- Security integration
- Azure administration
- Container deployment
- Monitoring and troubleshooting

---

## 4. DevSecOps work?

- Security scanning
- IaC scanning
- Container scanning
- Secrets management
- RBAC
- Pipeline security
- Vulnerability remediation

---

## 5. Infrastructure provisioned?

Using Terraform:

- Resource Groups
- Virtual Networks
- Subnets
- AKS
- ACR
- Storage Accounts
- Key Vault
- NSGs
- Load Balancers

---

## 6. Application deployment?

GitHub Actions/Jenkins → Docker Build → Push to ACR → Deploy to AKS using Kubernetes manifests or Helm.

---

## 7. Technologies?

- Azure
- Terraform
- GitHub
- GitHub Actions
- Jenkins
- Docker
- Kubernetes
- Helm
- SonarQube
- Trivy
- tfsec
- Linux
- Python
- Bash

---

# Container Security

## 1. Image scanning?

Using Trivy after Docker build and before pushing to Azure Container Registry.

---

## 2. Why image scanning?

To detect:

- OS vulnerabilities
- Package vulnerabilities
- Secrets
- Misconfigurations
- Malware

before deployment.

---

## 3. Why run as non-root?

Following the Principle of Least Privilege reduces the impact of a container compromise and limits unauthorized access.

---

## 4. Why minimal images like Alpine?

- Smaller attack surface
- Fewer vulnerabilities
- Faster downloads
- Faster deployments

---

## 5. Trivy command?

```bash
trivy image \
--severity HIGH,CRITICAL \
--exit-code 1 \
myimage:latest
```

---

## 6. Real example?

Trivy detected a critical OpenSSL vulnerability in the base image. The pipeline failed, we updated the base image to a patched version, rebuilt the image, and the scan passed before deployment.

---

# Build Failure Scenario

## 1. Build blocked due to vulnerabilities?

1. Review scan report.
2. Identify affected packages.
3. Verify severity.
4. Check if it's a true positive.
5. Update dependencies or base image.
6. Rebuild.
7. Re-run security scan.
8. Deploy only after all quality gates pass.

---

## 2. How unblock the build?

Remediate the issue, rebuild, and rerun the pipeline. Temporary exceptions are granted only after documented risk approval and compensating controls.

---

## 3. True positive vs false positive?

- Verify vulnerability against vendor advisories or the NVD.
- Confirm whether the vulnerable package is actually present and reachable in the application.
- If it's not exploitable or incorrectly detected, document it as a false positive and update the exception policy if approved.

---

## 4. Remediation process before rerunning?

1. Analyze the report.
2. Upgrade vulnerable dependencies or base images.
3. Fix code or infrastructure misconfigurations.
4. Validate locally if applicable.
5. Commit changes.
6. Rerun CI/CD pipeline.
7. Ensure all security gates pass before deployment.
