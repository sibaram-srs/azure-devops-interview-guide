# DevOps Interview Ready Answers (GitHub Actions, Jenkins, Terraform, Azure)

## 1. Can you give me a quick introduction about yourself and your professional experience?

Hi, I'm a DevOps Engineer with around **4+ years of experience** in CI/CD, Cloud, Infrastructure as Code, and Automation. I have worked extensively on **Azure, GitHub Actions, Jenkins, Terraform, Docker, Kubernetes, Linux, and Azure DevOps**. My responsibilities include building CI/CD pipelines, provisioning infrastructure using Terraform, automating deployments, monitoring applications, and supporting development teams by improving deployment reliability and reducing manual effort.

---

## 2. How long have you worked with Jenkins?

I have worked with Jenkins for around **3 years**.

---

## 3. During that period with Jenkins, what exactly did you do?

- Created and maintained Jenkins pipelines.
- Developed Declarative and Scripted Pipelines.
- Integrated Jenkins with GitHub.
- Configured Webhooks for automatic builds.
- Integrated SonarQube for code quality.
- Integrated Docker image builds.
- Triggered Terraform deployments.
- Managed Jenkins agents.
- Installed and configured Jenkins plugins.
- Managed credentials securely.
- Troubleshot failed builds and deployments.

---

## 4. Did you make any changes to the Jenkins pipeline, or were you only responsible for running the already configured pipeline?

Yes. I actively modified existing pipelines and also created new pipelines based on project requirements. I added new stages, optimized build execution, integrated quality gates, Docker builds, Terraform deployment, notifications, and deployment approvals.

---

## 5. Have you ever created a new Jenkins pipeline from scratch?

Yes.

The process was:

- Create Jenkinsfile
- Configure SCM
- Configure Webhook
- Configure Credentials
- Define Build Stages
- Add Testing Stage
- Build Docker Image
- Push Image to Registry
- Deploy using Terraform/Kubernetes
- Configure Notifications

---

## 6. Can you share your screen and draw a block diagram of your build/deployment environment using GitHub, GitHub Actions, Terraform, and Azure?

```text
Developer
     │
     ▼
 GitHub Repository
     │
     ▼
 GitHub Actions Trigger
     │
     ▼
 GitHub Runner
(Self-hosted/GitHub-hosted)
     │
     ├── Build Application
     ├── Run Tests
     ├── SonarQube Scan
     ├── Build Docker Image
     ├── Push Image to ACR
     └── Terraform Apply
                │
                ▼
          Azure Infrastructure
      (VMs, AKS, App Service, Storage)
                │
                ▼
          Application Deployment
```

---

## 7. When exactly does the GitHub Actions pipeline trigger?

Normally it triggers:

- On Push
- Pull Request
- Workflow Dispatch (Manual)
- Schedule (Cron)
- Release
- Tag Push

Most CI pipelines trigger on **Push** and **Pull Request**.

---

## 8. Where is that trigger configured?

Inside the workflow YAML file under the **on:** section.

Example:

```yaml
on:
  push:
    branches:
      - dev
```

---

## 9. Where does the GitHub Actions pipeline code reside?

Inside the same GitHub repository.

---

## 10. Is your GitHub a Cloud GitHub instance or an Enterprise GitHub instance?

We use **GitHub Enterprise Cloud**.

*(If your company uses github.com, answer GitHub Cloud.)*

---

## 11. In which directory of the GitHub repository are the GitHub Actions workflow files located?

```
.github/workflows/
```

---

## 12. Which specific YAML file is responsible for triggering the pipeline on a branch push?

Example:

```
ci.yml
```

or

```
build.yml
```

or

```
deploy.yml
```

The actual filename depends on the project.

---

## 13. What are the main sections inside a GitHub Actions YAML workflow file?

- name
- on
- env
- jobs
- runs-on
- steps
- uses
- run
- needs
- if
- strategy
- permissions

---

## 14. Does the GitHub Actions workflow file also contain information about which machine (runner) the pipeline should execute on?

Yes.

That information is specified inside the workflow.

---

## 15. What is the name of the section where the runner information is defined?

```yaml
runs-on
```

Example:

```yaml
runs-on: ubuntu-latest
```

or

```yaml
runs-on: self-hosted
```

---

## 16. What is the difference between a Self-hosted GitHub Runner and a GitHub-hosted Runner?

### GitHub-hosted Runner

- Managed by GitHub
- No installation required
- Automatically provisioned
- Auto cleanup after every job
- Limited customization

### Self-hosted Runner

- Managed by us
- Installed on our VM
- Full customization
- Can access internal network
- We handle updates and maintenance

---

## 17. What is the difference in the setup process between a Self-hosted Runner and a GitHub-hosted Runner?

### GitHub-hosted

No setup required.

Just specify:

```yaml
runs-on: ubuntu-latest
```

GitHub provisions the VM automatically.

### Self-hosted

Need to:

- Create VM
- Install Runner package
- Register Runner
- Configure labels
- Start Runner service

---

## 18. Suppose I have a VM running in my infrastructure and I want to use it as a Self-hosted GitHub Runner. What are the setup steps?

1. Create VM
2. Login to GitHub Repository
3. Go to Settings
4. Actions
5. Runners
6. Add New Runner
7. Download Runner package
8. Extract package
9. Run configuration command

```bash
./config.sh
```

10. Provide GitHub Registration Token

11. Assign Runner Labels

12. Install as Service

```bash
sudo ./svc.sh install
```

13. Start Service

```bash
sudo ./svc.sh start
```

14. Verify Runner is Online

Then use:

```yaml
runs-on: self-hosted
```

---

## 19. When the pipeline triggers on a push, where is that trigger logic written?

Inside the workflow YAML file.

Example:

```yaml
on:
  push:
    branches:
      - dev
```

---

## 20. You mentioned separate repositories for Dev, Test, and QA. Are you creating separate repositories or just separate branches?

Typically we maintain **one repository with multiple branches**:

- dev
- test
- qa
- stage
- main

Each branch triggers its respective deployment pipeline.

---

## 21. Can you tell me the exact GitHub Actions YAML file that triggers the workflow?

Example:

```
.github/workflows/ci.yml
```

or

```
.github/workflows/build.yml
```

or

```
.github/workflows/deploy.yml
```

---

## 22. Can you tell the section that specifies the runner (runs-on)?

Yes.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

or

```yaml
jobs:
  build:
    runs-on: self-hosted
```

The **runs-on** section specifies the runner where the workflow will execute.
