# TCS + MTN — DevOps Engineer Interview Questions & Answers

## 1. Can you start with your introduction, technical expertise in your last project, and your specific role?

**Answer:**

Sure. I have around 10 years of experience in IT, primarily focused on DevOps, Azure Cloud, Kubernetes/AKS, Infrastructure as Code, CI/CD, cloud networking, security, and production operations.

In my recent project, I was working as a Senior DevOps/Cloud Engineer, where I was responsible for designing, provisioning, automating, and maintaining Azure infrastructure and supporting application teams with their deployments.

My key technical areas include Azure, AKS, Terraform, Azure DevOps, Git, Docker, Kubernetes, Helm, ArgoCD, Azure Container Registry, Application Gateway, Azure Front Door, API Management, Key Vault, Azure Storage, Azure Networking, Azure Monitor, Log Analytics, and security tools.

My responsibilities were not limited to CI/CD. I was involved in the complete application lifecycle — infrastructure provisioning, containerization, AKS deployment, networking, security, monitoring, autoscaling, production troubleshooting, upgrades, and operational support.

As a senior engineer, my focus is mainly on automation, reliability, scalability, security, and reducing manual operational activities.

---

## 2. What is your total experience in the cloud?

**Answer:**

I have approximately **[X] years of hands-on cloud experience**, primarily with Microsoft Azure.

My Azure experience covers compute, containers, networking, security, storage, monitoring, and automation.

I have worked extensively with services such as AKS, Azure VM/VMSS, Azure Container Registry, Virtual Networks, Subnets, NSGs, Private Endpoints, Application Gateway, Azure Front Door, API Management, Key Vault, Storage Accounts, Azure Monitor, Log Analytics, and Application Insights.

From an engineering perspective, my strength is not only knowing individual Azure services but understanding how they integrate together to build a production-ready cloud platform.

---

## 3. How many applications did you move to AKS from on-premises/cloud, and what was your migration strategy?

**Answer:**

In my recent project, I worked on migrating approximately **[X] applications** to AKS.

We followed a phased migration strategy rather than directly moving applications into Kubernetes.

First, we performed an application assessment covering the application architecture, dependencies, database connectivity, storage requirements, configuration, secrets, ports, traffic patterns, CPU and memory requirements, and availability requirements.

Then we categorized applications based on complexity and migration readiness.

The overall approach was:

```text
Existing Application
        |
        v
Application Assessment
        |
        v
Containerization
        |
        v
Docker Image
        |
        v
Azure Container Registry
        |
        v
AKS Deployment
        |
        v
Dev / Test / UAT
        |
        v
Performance & Security Testing
        |
        v
Production Migration


For production migration, depending on the application, we used rolling deployment, blue-green deployment, or controlled cutover.

After migration, we validated functionality, connectivity, performance, monitoring, security, autoscaling, and rollback capability.

The main principle was to migrate, validate, observe, and then optimize.

4. What was the functionality of those applications?

Answer:

The applications were mainly enterprise business applications consisting of REST APIs, microservices, backend services, web applications, integration services, and background processing workloads.

Some applications were completely stateless and were suitable for Kubernetes Deployments with horizontal scaling.

Other applications had persistent storage, scheduled jobs, background workers, database dependencies, or message queue integrations.

So we selected the Kubernetes workload type based on the application's behavior.

For example:

Stateless applications → Deployment
Stateful applications → StatefulSet
Node-level agents → DaemonSet
One-time processing → Job
Scheduled processing → CronJob

The important part was understanding the application architecture before deciding how to deploy it on AKS.

5. What other Azure services (Storage, Networking, Firewalls, etc.) have you worked with?

Answer:

I have worked with Azure services across networking, security, storage, compute, containers, and monitoring.

For networking, I have worked with:

Azure VNet
Subnets
NSGs
Route Tables / UDR
Private Endpoints
Private DNS
Azure Load Balancer
Application Gateway
Azure Front Door
VPN connectivity
Network troubleshooting

For security:

Azure Key Vault
Managed Identity
RBAC
WAF
NSGs
Private Endpoints
Secret management
Security scanning

For storage:

Azure Storage Accounts
Blob Storage
Azure Files
Private connectivity
Managed Identity/SAS-based access where applicable

For monitoring:

Azure Monitor
Log Analytics
Application Insights
Container Insights
Alerts
KQL

I generally look at these services as part of an integrated architecture rather than isolated services.

6. What is your real-world experience with AKS?

Answer:

AKS is one of my major areas of hands-on experience.

I have worked with AKS from both platform and application perspectives.

My experience includes:

AKS cluster provisioning
System and user node pools
Kubernetes RBAC
Azure networking
Kubernetes Services
Ingress
Application Gateway integration
ConfigMaps
Secrets
Managed Identity / Workload Identity
Resource requests and limits
HPA
Cluster Autoscaler
Pod scheduling
Taints and tolerations
Node affinity
Pod affinity/anti-affinity
StatefulSets
DaemonSets
Jobs and CronJobs
Helm
Azure Container Registry
Monitoring
Log Analytics
Cluster upgrades
Production troubleshooting

I have also handled issues such as:

Pods stuck in Pending
CrashLoopBackOff
ImagePullBackOff
OOMKilled
Readiness/liveness probe failures
HPA not scaling
Node pressure
CPU/memory shortage
Ingress issues
DNS/networking problems
Application latency
Deployment failures

My troubleshooting approach is to identify whether the issue is at the application, pod, Kubernetes, node, network, or Azure infrastructure layer.

7. Have you performed AKS upgrades, and which versions did you choose?

Answer:

Yes, I have been involved in AKS upgrades.

I don't select a Kubernetes version simply because it is the latest available version. I first check the supported versions, application compatibility, Kubernetes API deprecations, and compatibility of all dependent components.

Before an upgrade, I normally check:

Current AKS version
Target AKS version
Kubernetes API deprecations
Application compatibility
Helm compatibility
Ingress controller compatibility
CSI drivers
Monitoring agents
Admission controllers
PodDisruptionBudgets
Node pool compatibility
Third-party integrations

For example, when upgrading from one supported minor version to another, I first perform the upgrade in non-production, validate the workloads, and then proceed with production.

The process is:

Current AKS Version
        |
        v
Compatibility Assessment
        |
        v
API Deprecation Check
        |
        v
Non-Production Upgrade
        |
        v
Application Validation
        |
        v
Production Upgrade
        |
        v
Node Pool Validation
        |
        v
Smoke Testing & Monitoring


The important point is that an AKS upgrade is not only a control-plane version change. We need to consider workloads, node pools, add-ons, APIs, networking, monitoring, and application dependencies.

8. What is a StatefulSet, and what is the specific use case for it, especially regarding naming?

Answer:

A StatefulSet is a Kubernetes workload resource designed for stateful applications where pods require stable identity, stable network identity, and persistent storage.

Unlike a normal Deployment, StatefulSet provides predictable pod names.

For example:

mysql-0
mysql-1
mysql-2


If mysql-0 is recreated, Kubernetes attempts to maintain the same identity.

This is useful for applications such as:

Databases
Kafka
ZooKeeper
Elasticsearch-type workloads
Distributed systems requiring stable identity

StatefulSet also provides ordered deployment and termination and maintains association with persistent volumes.

The key difference is:

Deployment
    |
    +--> Pods are generally interchangeable

StatefulSet
    |
    +--> Pods have stable identity and storage association


So my rule is:

If pod identity and persistent state matter, StatefulSet is a candidate. If pods are stateless and interchangeable, Deployment is generally preferred.

9. What is a DaemonSet, and how do you use it for monitoring or specific requirements?

Answer:

A DaemonSet ensures that a pod runs on every eligible node in a Kubernetes cluster.

For example, if the cluster has four nodes:

Node 1 --> Monitoring Agent
Node 2 --> Monitoring Agent
Node 3 --> Monitoring Agent
Node 4 --> Monitoring Agent


If a new node joins the cluster, Kubernetes automatically schedules the DaemonSet pod on that node as well.

DaemonSets are commonly used for:

Log collection agents
Monitoring agents
Security agents
Node-level metrics
Network plugins
Storage-related agents

A practical example is a logging agent that runs on every node and collects container logs before sending them to a centralized logging platform.

The simple distinction is:

Deployment is generally used when I need a defined number of application replicas.

DaemonSet is used when I need a specific pod running on every eligible node.

11. Can you describe the end-to-end architecture/components involved from a user hitting the application to the backend pods and database?

Answer:

A typical production architecture can look like this:

                    Internet / Users
                           |
                           v
                  Azure Front Door
                           |
                           v
                      WAF / Edge
                           |
                           v
              Application Gateway / WAF
                           |
                           v
                    AKS Ingress
                           |
                           v
                 Kubernetes Service
                           |
                           v
                    Application Pods
                       /          \
                      /            \
                     v              v
              Backend Service     Cache/Queue
                     |
                     v
                  Database


Supporting services can include:

Application Pod
      |
      +----> Azure Key Vault
      |
      +----> Azure Storage
      |
      +----> External APIs
      |
      +----> Message Queue
      |
      +----> Azure Monitor / Log Analytics


The request flow is:

User sends an HTTPS request.
Azure Front Door receives the request.
WAF and routing policies are applied.
Traffic is routed to the appropriate backend.
Application Gateway/Ingress handles application-level routing.
Kubernetes Service routes the request to the appropriate pod.
Application processes the request.
The application communicates with backend services, cache, queues, or databases.
Logs and metrics are collected by the monitoring platform.

From a troubleshooting perspective, I always consider the complete request path because latency or failure can occur at any of these layers.

12. If traffic is increasing but your pods are not scaling up, even though HPA rules and configurations are correct, where would you start troubleshooting?

Answer:

I would not immediately assume that HPA is broken.

I would troubleshoot the complete autoscaling chain.

First, I would check the HPA status:

kubectl get hpa -n <namespace>
kubectl describe hpa <hpa-name> -n <namespace>


Then I would check the actual pod metrics:

kubectl top pods -n <namespace>


I would verify whether the metric configured in HPA is actually increasing.

For example, if HPA is CPU-based but the application is experiencing high latency while CPU remains at 40%, HPA may correctly decide not to scale.

In that situation, CPU is simply not a good scaling signal.

I would also check:

kubectl get pods -n <namespace>
kubectl get events -n <namespace>


Then I would investigate:

Traffic
   |
   v
Application Load
   |
   v
Metrics
   |
   v
HPA
   |
   v
Replica Count
   |
   v
Scheduler
   |
   v
Node Capacity
   |
   v
Cluster Autoscaler


Depending on the application, I would also consider metrics such as requests per second, queue length, active connections, latency, or custom application metrics.

13. What other reasons could cause scaling to fail, such as resource limits or node pool capacity?

Answer:

There can be several reasons.

First, HPA may successfully increase the replica count, but the pods may remain Pending because the cluster does not have enough CPU or memory.

I would check:

kubectl get pods
kubectl describe pod <pod-name>
kubectl get nodes
kubectl describe node <node-name>


Other possible causes include:

Insufficient node capacity

The node pool may not have enough allocatable CPU or memory.

Cluster Autoscaler issue

The autoscaler may not be able to add new nodes because of node pool limits, quota, VM availability, or other infrastructure issues.

Incorrect resource requests

If a pod requests too much CPU or memory, it may not fit on available nodes.

Taints and tolerations

Nodes may have taints that the pods don't tolerate.

Node affinity / anti-affinity

Scheduling rules may prevent pods from being placed on available nodes.

ResourceQuota

Namespace-level quotas may prevent additional pods or resources.

Subnet IP exhaustion

In Azure networking scenarios, insufficient IP addresses can prevent new pods or nodes from being created.

HPA metric issues

Metrics may be unavailable, stale, or not representative of actual application load.

So I would troubleshoot both:

HPA Scaling
     +
Kubernetes Scheduling
     +
Node Pool Capacity
     +
Azure Infrastructure

14. If the application performance is slow/low despite proper configuration, what levels of service or logs would you check?

Answer:

I would troubleshoot performance layer by layer.

First, I would determine where the latency is being introduced.

1. Front Door

I would check:

Request latency
Backend response time
HTTP status codes
Routing
WAF events
2. Application Gateway / Ingress

I would check:

Access logs
Backend health
Response codes
Connection errors
TLS issues
Backend response time
3. Kubernetes

I would check:

CPU utilization
Memory utilization
Pod restarts
OOMKilled
CPU throttling
Pod scheduling
Readiness/liveness probes

For example:

kubectl top pods
kubectl get pods
kubectl describe pod <pod-name>
kubectl get events

4. Application

I would check:

Application logs
Request latency
Thread pools
Connection pools
Garbage collection
Dependency calls
Application errors
5. Database

I would check:

Slow queries
Query execution time
CPU
Memory
Locks
Connection pool
Connection limits
6. External Dependencies

I would check:

API response time
Timeouts
Retries
DNS
Network connectivity

The objective is to identify the exact latency boundary instead of simply checking logs randomly.

15. Would you check network-level issues as part of that performance troubleshooting?

Answer:

Absolutely.

Network issues can cause application latency even when the application and Kubernetes configuration appear correct.

I would check:

DNS resolution
Network latency
Packet loss
NSG rules
Route tables
Firewall rules
Private Endpoint connectivity
Application Gateway backend health
Network policies
TLS handshake
Connection timeouts
NAT/SNAT limitations where applicable

For example, if an application normally gets a database response in 20 ms but suddenly it takes 500 ms, I would not immediately conclude that the database is slow.

I would verify:

Application Pod
      |
      v
DNS
      |
      v
Network Path
      |
      v
Firewall / NSG
      |
      v
Private Endpoint
      |
      v
Database


This helps determine whether the problem is application latency, network latency, or database latency.

16. Why are you using Azure Front Door, and how does it differ from a standard Load Balancer in a global context?

Answer:

Azure Front Door is useful when we need global HTTP/HTTPS traffic management, edge routing, application-level routing, and WAF capabilities.

A standard Azure Load Balancer is primarily a Layer 4 load balancer, while Front Door provides Layer 7 HTTP/HTTPS capabilities.

The main difference is:

Feature	Azure Front Door	Azure Load Balancer
Layer	Layer 7	Layer 4
Global traffic management	Yes	Primarily regional
HTTP/HTTPS routing	Yes	No application-level routing
URL/path-based routing	Yes	No
WAF	Yes	Not its primary purpose
Edge/Anycast delivery	Yes	No
Global failover	Yes	No equivalent global edge role
TCP/UDP	No	Yes

For example:

                    Users
                      |
                      v
               Azure Front Door
                  /        \
                 /          \
                v            v
           Region 1       Region 2
              |              |
             AKS            AKS


Front Door can route users toward healthy backends and provides global application delivery capabilities.

So I would summarize it as:

Azure Front Door is primarily a global Layer-7 application delivery and traffic management service, while Azure Load Balancer is primarily a Layer-4 network load balancing service.

17. What is your experience with Azure Application Gateway and API Management (APIM)?

Answer:

I have worked with both, but they solve different problems.

Application Gateway

Application Gateway is mainly used for Layer-7 traffic management.

I have worked with:

Host-based routing
Path-based routing
TLS termination
Backend pools
Health probes
WAF
AKS ingress integration

For example:

example.com/api/*
        |
        v
Application Gateway
        |
        v
API Service

example.com/web/*
        |
        v
Application Gateway
        |
        v
Web Service

API Management

APIM is focused more on API management and governance.

I have worked with:

API publishing
Authentication/authorization
API policies
Rate limiting
Quotas
Request/response transformation
API versioning
Subscription management
Backend routing

A typical flow can be:

Client
  |
  v
Front Door
  |
  v
APIM
  |
  v
Backend APIs
  |
  v
AKS


So I would not consider Application Gateway and APIM as direct replacements. Application Gateway focuses more on application traffic routing and WAF, while APIM provides API lifecycle management and governance.

18. Do you know how to configure rate limiting and hit limits in APIM?

Answer:

Yes.

APIM provides policies for controlling API consumption.

For short-term request throttling, I can use rate limiting.

For example:

<rate-limit calls="100"
            renewal-period="60" />


Conceptually, this allows 100 calls within a 60-second period.

For longer-term consumption limits, such as 10,000 calls per day, I would use quota policies.

The distinction is:

Rate Limit
    |
    +--> Short-term request throttling

Quota
    |
    +--> Longer-term consumption limit


We can also apply policies based on consumers, subscriptions, IP addresses, headers, tokens, or other business requirements depending on the API design.

A typical flow is:

Client
  |
  v
APIM
  |
  +--> Authentication
  |
  +--> Rate Limit
  |
  +--> Quota
  |
  +--> Other Policies
  |
  v
Backend API


I would also monitor rejected/throttled requests and make sure consumers receive an appropriate response when the configured limit is exceeded.

19. What is your experience with development—which languages or application stacks have you worked with?

Answer:

I consider myself primarily a DevOps and Cloud engineer, but I have good development knowledge that helps me work effectively with application teams.

I have worked around application stacks such as:

Java / Spring Boot
.NET / ASP.NET
Python
Node.js
REST APIs
Microservices

My focus is not to position myself as a full-time application developer, but I understand application behavior well enough to troubleshoot build, deployment, runtime, and infrastructure issues.

For example, if a Java application is repeatedly restarting in Kubernetes, I would investigate:

Pod Restart
    |
    +--> Application Exception?
    |
    +--> OOMKilled?
    |
    +--> Liveness Probe?
    |
    +--> Dependency Failure?
    |
    +--> Configuration?
    |
    +--> Secret?
    |
    +--> Network?


This development knowledge helps me identify whether an issue is actually related to Kubernetes or is originating from the application itself.

20. Apart from Terraform and Azure DevOps, what other tools are you using regularly?

Answer:

Apart from Terraform and Azure DevOps, I regularly work with tools across source control, Kubernetes, security, monitoring, and automation.

Source Control
Git
GitHub
Azure Repos
Kubernetes
kubectl
Helm
AKS
ArgoCD
Containers
Docker
Azure Container Registry
Security
SonarQube
Trivy
SAST tools
Dependency scanning
Container image scanning
Secret scanning
Monitoring
Azure Monitor
Log Analytics
Application Insights
Prometheus/Grafana where applicable
Scripting
Bash
Python
YAML

I use these tools mainly to automate deployment, improve security, monitor applications, and reduce manual operational work.

21. Do you have hands-on experience with ArgoCD for GitOps automation?

Answer:

Yes, I have hands-on experience with ArgoCD and GitOps-based deployments.

The basic architecture is:

Developer
    |
    v
Git Repository
    |
    v
Kubernetes Manifests / Helm
    |
    v
ArgoCD
    |
    v
AKS


The Git repository acts as the source of truth for the desired Kubernetes state.

For example, if Git specifies:

replicaCount: 5


but the cluster is running three replicas, ArgoCD can detect the drift between the desired state and the actual state.

ArgoCD provides visibility into:

Sync status
Application health
Configuration drift
Deployment history
Kubernetes resources

A common model is:

Application Build
       |
       v
Container Image
       |
       v
Container Registry
       |
       v
Git Repository
       |
       v
ArgoCD
       |
       v
AKS


I see the main benefit of GitOps as having an auditable desired state, controlled deployments, and visibility into configuration drift.

22. What is your current notice period, and have you already resigned?

Answer:

My current notice period is [X days].

I have [already resigned / not resigned yet].

My expected last working day would be [DATE], subject to the formal release process.

If required, I can also discuss the possibility of an early release.

23. What is your current CTC and expected CTC?

Answer:

My current CTC is approximately ₹[CURRENT CTC] LPA.

Considering my overall experience, technical expertise, and the responsibilities associated with this role, I am expecting around ₹[EXPECTED CTC] LPA.

However, I am open to discussing the overall compensation based on the role, responsibilities, and company compensation structure.

24. Where are you located, and are you comfortable with a hybrid model, traveling to the office twice a week?

Answer:

I am currently based in [YOUR LOCATION].

Yes, I am comfortable with a hybrid working model and can travel to the office twice a week as required.

I am comfortable working according to the team's operational and collaboration requirements.

25. Are you comfortable with operational support roles and being available for critical deployment issues or system failures outside of regular hours?

Answer:

Yes, absolutely.

I understand that a senior DevOps role involves production ownership and sometimes requires support outside normal business hours, especially during critical deployments, production incidents, infrastructure failures, or system outages.

I am comfortable participating in:

On-call support
Critical production deployments
Production incidents
Infrastructure failures
Kubernetes troubleshooting
Emergency rollback
Disaster recovery activities
Release support

My approach during an incident is:

Production Incident
        |
        v
Immediate Assessment
        |
        v
Mitigate / Restore Service
        |
        v
Root Cause Analysis
        |
        v
Permanent Corrective Action
        |
        v
Automation / Monitoring Improvement
        |
        v
Prevent Recurrence


I also believe that operational support should not become continuous firefighting.

If the same issue keeps occurring, I would investigate the root cause and implement automation, monitoring, alerting, or architectural improvements to prevent recurrence.

For me, senior DevOps ownership means not only restoring the service during an incident but also making sure the same incident is less likely to happen again.
