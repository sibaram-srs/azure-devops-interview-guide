# EY — Observability Engineer Interview Answers

**1. Do you have any experience with observability, and have you read the JD with respect to the tools needed?**

Yes. I have hands-on experience with observability across infrastructure, Kubernetes, applications, networking, and cloud services. I have worked with Azure Monitor, Log Analytics, Application Insights, Prometheus, Grafana, Kubernetes metrics, centralized logging, and alerting.

My observability approach covers the three major pillars: **metrics, logs, and traces**, along with alerting and dashboards.

**2. Have you worked primarily on AWS or Azure?**

My primary hands-on production experience is with **Azure**. I have worked extensively with AKS, Azure Monitor, Log Analytics, Application Insights, Front Door, Application Gateway, Key Vault, Storage, networking, Terraform, and Azure DevOps.

I also have knowledge of AWS concepts and services, but Azure is my strongest area.

**3. If Kubernetes is showing a running state but users are getting HTTP 503 errors, how and what would you check?**

I would not assume that `Running` means the application is healthy. I would trace the request path end-to-end.

First:

```bash
kubectl get pods -n <namespace>
kubectl get svc -n <namespace>
kubectl get ingress -n <namespace>


Then I would check:

kubectl describe ingress <ingress-name> -n <namespace>
kubectl describe svc <service-name> -n <namespace>
kubectl get endpointslices -n <namespace>


I would verify:

Pod readiness
Service selectors
EndpointSlices
Ingress/backend health
Readiness probes
Application logs
Ingress controller logs
DNS
Network policies
Backend connection failures

A common scenario is that pods are Running but not Ready, so the Service has no healthy endpoints and the Ingress returns 503.

My troubleshooting flow is:

User → DNS → Load Balancer/Ingress → Service → Endpoints → Pod → Application

4. If a Kubernetes pod only uses 20% CPU but is restarting several times every hour, what could be the cause, and how would you find the cause?

Low CPU does not mean the application is healthy.

I would first check:

kubectl get pod <pod-name>
kubectl describe pod <pod-name>
kubectl logs <pod-name> --previous
kubectl get events -n <namespace> --sort-by=.lastTimestamp


I would specifically look for:

OOMKilled
Memory limit exceeded
Application crash
Liveness probe failure
Dependency failure
Configuration issues
Node problems
Container exit codes
Signal termination

I would also check restart count and termination reason.

5. Can a pod restarting under low CPU utilization be due to certain memory limits being exceeded?

Absolutely.

CPU and memory are independent resources. A pod can use only 20% CPU but exceed its memory limit and get terminated with OOMKilled.

I would verify with:

kubectl describe pod <pod-name>
kubectl top pod <pod-name>


If the container shows OOMKilled, I would review its memory request/limit and investigate whether there is a memory leak or simply an incorrectly sized limit.

6. If users report that the application is running very slow, but CPU utilization is only at 30% and memory is around 40% to 50% with no obvious errors, how would you investigate?

I would not conclude that the infrastructure is healthy just because CPU and memory are normal.

I would investigate latency across the complete request path.

I would check:

Application response time
HTTP latency
Database query performance
External API latency
Network latency
DNS resolution
Connection pools
Thread pools
Application logs
Application Insights distributed traces
Load Balancer/Ingress latency
Pod CPU throttling
Disk I/O
Database connections/locks
Cache performance

With Application Insights or distributed tracing, I would identify which dependency is consuming the most time.

For example:

User Request → API → Database

If the API takes 2 seconds but 1.8 seconds is spent waiting for the database, increasing pod CPU will not solve the problem.

7. Have you used Helm for deployment?

Yes. I have hands-on experience with Helm for Kubernetes deployments.

I use Helm charts to package Kubernetes resources and maintain environment-specific values.

For example:

Chart
 |
 +-- Deployment
 +-- Service
 +-- Ingress
 +-- ConfigMap
 +-- HPA
 |
 +-- values-dev.yaml
 +-- values-uat.yaml
 +-- values-prod.yaml


8. What is Helm?

Helm is a package manager for Kubernetes.

It allows us to define reusable Kubernetes templates and deploy them using configurable values.

Instead of maintaining many duplicated YAML files, we can create one reusable chart and provide different values for each environment.

Common commands are:

helm install
helm upgrade
helm rollback
helm history
helm list


9. Using Azure resources, can you provide a brief architectural diagram for a front-end and back-end application to make it scalable, available, and reliable?

A typical architecture would be:

                         Internet Users
                              |
                              v
                     Azure Front Door
                         + WAF
                              |
                              v
                    Application Gateway
                         + WAF
                              |
                              v
                    AKS Ingress Controller
                         /          \
                        /            \
                       v              v
                  Frontend Pods    Backend Pods
                       |              |
                       |              v
                       |         Azure Service Bus
                       |              |
                       |              v
                       |          Azure SQL
                       |
                       +--------> Azure Storage
                                      |
                                   Key Vault

                    AKS → Azure Monitor
                    AKS → Log Analytics
                    AKS → Application Insights


For scalability, I would use HPA and Cluster Autoscaler.

For availability, I would use multiple replicas, availability zones where appropriate, health probes, and multiple backend instances.

For security, I would use private endpoints, managed identity/workload identity, Key Vault, RBAC, WAF, and network policies.

10. If an application running on Kubernetes has been using Azure Blob Storage regularly, but suddenly returns an "access denied" error without any code changes, how would you investigate this as a DevOps engineer?

I would first determine whether the problem is authentication, authorization, networking, or the storage service itself.

I would check:

Pod identity/workload identity
Managed Identity
Azure RBAC permissions
Storage account IAM
Storage firewall/network restrictions
Private Endpoint
Private DNS
Storage account configuration
SAS token expiry, if SAS is being used
Key/credential rotation
Kubernetes ServiceAccount configuration
Application logs
Azure Activity Logs

For example, if the application uses Workload Identity, I would verify that the Kubernetes ServiceAccount is correctly mapped to the Azure identity and that the identity still has the required Storage Blob Data permissions.

I would also check Azure Activity Logs and Storage diagnostic logs to determine why the request was denied.

11. With respect to CI/CD, what are the detailed stages you would provide to deploy on Kubernetes?

A production-grade pipeline would typically be:

Developer Commit
      |
      v
Pull Request
      |
      v
Code Review
      |
      v
Build
      |
      v
Unit Tests
      |
      v
SonarQube / Code Quality
      |
      v
Dependency / SAST Scan
      |
      v
Docker Build
      |
      v
Container Image Scan
      |
      v
Push Image to ACR
      |
      v
Update Helm/Deployment Configuration
      |
      v
Deploy to Dev
      |
      v
Integration / Functional Tests
      |
      v
UAT
      |
      v
Approval
      |
      v
Production Deployment
      |
      v
Smoke Test
      |
      v
Monitoring / Validation


I also ensure image tagging is immutable, secrets are retrieved securely, production deployments require appropriate approvals, and rollback is available.

12. How do you handle approvals in your Azure DevOps pipeline, and do you have experience with Jenkins?

In Azure DevOps, I use environment-based approvals and checks.

For example:

Build → Dev → UAT → Approval → Production

Production can have approval requirements, branch policies, security checks, and business approvals.

I also have exposure to Jenkins. The concepts are similar: source trigger, build, test, security checks, artifact creation, deployment, approval, and post-deployment validation.

13. Are you aware of SRE-related work and terminologies?

Yes. I have worked with SRE concepts such as:

SLI
SLO
SLA
Error Budget
Availability
Reliability
Incident Management
On-call
MTTR
MTTD
MTBF
Observability
Toil Reduction
Capacity Planning
Disaster Recovery

The primary SRE objective I follow is to improve reliability while reducing manual operational toil through automation and better observability.

14. If the availability of a particular application is 99.9%, what other SRE parameters would you go and verify?

I would not look at availability alone.

I would verify:

Latency
Error rate
Request success rate
SLI/SLO compliance
Error budget consumption
MTTR
MTTD
Incident frequency
Deployment failure rate
Rollback frequency
Saturation
Capacity
Dependency availability

For example, an application could have 99.9% availability but still have very poor latency. So availability needs to be considered along with performance and reliability indicators.

15. If automated deployments are configured but manual changes are made in the portal by someone with write access, what are the various ways you can resolve this configuration drift?

First, I would detect the drift using Terraform plan, Azure Policy, configuration monitoring, or GitOps drift detection.

Then I would decide which state is correct.

If Git/Terraform is the source of truth, I would revert the manual change through automation.

If the manual change is legitimate, I would update the Terraform code accordingly and deploy it through the normal pipeline.

To prevent recurrence, I would use:

Least-privilege RBAC
Restrict production write access
Azure Policy
GitOps
Terraform-based deployments
Pull-request approvals
Audit logging

The goal is not only to fix drift but to prevent unauthorized manual changes.

16. If a virtual machine is deleted manually but the Terraform state file still says it is present, how do you handle and resolve this state mismatch?

I would run:

terraform plan


During refresh, Terraform should detect that the resource no longer exists.

Terraform will generally show that the resource needs to be created again if it remains declared in the configuration.

I would review the plan carefully and then run:

terraform apply


If required, I can also refresh/inspect the state and verify the actual Azure resource before applying.

17. Will Terraform show the drift during the plan stage or automatically try to recreate the resource when you run apply?

terraform plan normally detects the drift during the refresh/plan process and shows that the resource needs to be created again because it exists in configuration/state but no longer exists remotely.

terraform apply then executes the planned replacement/creation.

So my process is:

terraform plan → Review → terraform apply

I never blindly run apply in production without reviewing the plan.

18. Can you explain the difference between git merge, git fetch, and git pull?

git fetch downloads the latest changes from the remote repository but does not modify my current working branch.

git merge combines changes from another branch into the current branch.

git pull is essentially:

git fetch + git merge

For example:

git fetch origin
git merge origin/main


is conceptually similar to:

git pull origin main


Although git pull can also be configured to use rebase instead of merge.

19. Do you have knowledge or exposure to shell scripting?

Yes. I have good hands-on experience with Bash/Shell scripting for DevOps and Linux automation.

I use shell scripts for tasks such as:

Deployment automation
Health checks
Log processing
File operations
Azure CLI automation
Kubernetes operations
Backup/cleanup scripts
Environment validation

For example, I can combine kubectl, az, grep, awk, sed, curl, and other Linux utilities to automate repetitive operational tasks.

I also follow scripting best practices such as error handling, parameterization, logging, and avoiding hardcoded credentials.
