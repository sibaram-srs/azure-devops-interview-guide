# NewVision — Senior Azure Engineer Interview Answers

**1. Why don't you start with your intro? (Focusing on day-to-day activities and achievements from the past two quarters)**

I have around 10 years of experience in DevOps and Azure Cloud engineering, with strong hands-on experience in AKS, Terraform, Azure DevOps, Kubernetes, networking, security, CI/CD, monitoring, and production operations.

In my day-to-day role, I work on AKS administration, Terraform infrastructure, CI/CD pipelines, deployments, production troubleshooting, Azure networking, monitoring, security, and cost optimization.

Over the last two quarters, my major focus has been improving deployment automation, optimizing AKS resources, strengthening security controls, improving monitoring and alerting, and reducing manual operational activities.

One of my key achievements has been standardizing infrastructure and deployment processes through reusable Terraform modules and automated pipelines, which improved deployment consistency and reduced manual errors.

**2. Where is your code repository located (GitHub, Azure Repos, etc.)?**

Our primary repositories are hosted in Azure Repos/Git, with separate repositories or folders for application code, Terraform infrastructure, Helm charts, and environment-specific configurations.

We follow pull requests, branch policies, code reviews, and CI/CD-based deployments.

**3. Can you walk me through the architecture of your AKS cluster (specifically node groups, cluster autoscaling, and RBAC rules in production)?**

Our production AKS cluster is divided into system and user node pools.

The system node pool runs critical Kubernetes components, while user node pools run application workloads.

We use Cluster Autoscaler to automatically add or remove nodes based on pending workloads and capacity.

For application scaling, HPA increases or decreases pod replicas based on resource or custom metrics.

For security, we use Azure AD/Entra ID integration, Kubernetes RBAC, Azure RBAC where appropriate, managed identities/workload identity, and least-privilege access.

The architecture is:

`Users → Front Door/WAF → Ingress → AKS Service → Pods`

Supporting components include ACR, Key Vault, Azure Monitor, Log Analytics, private networking, and databases.

**4. How exactly is your production AKS set up?**

Production AKS is deployed using Terraform.

We use separate system and user node pools, private networking, Azure CNI where required, private endpoints, Azure Container Registry, Key Vault, monitoring, RBAC, autoscaling, ingress, and network policies.

Workloads are separated using namespaces, resource quotas, requests/limits, and appropriate RBAC.

Production changes are controlled through CI/CD and pull-request approvals rather than direct manual changes.

**5. How many nodes do you have in your cluster?**

The exact number varies based on workload and autoscaling configuration. For example, we may have around **[X] system nodes and [Y] user nodes**, with Cluster Autoscaler configured to scale within defined minimum and maximum limits.

I prefer describing the architecture rather than giving only a fixed number because production capacity changes according to workload.

**6. How many microservices and applications are currently deployed on that cluster?**

We have approximately **[X] microservices/applications** deployed across multiple namespaces.

They include UI services, backend APIs, microservices, background workers, and scheduled workloads.

We use namespaces, resource limits, RBAC, network policies, and Helm-based deployment to keep workloads organized.

**7. Which application service is the most frequently deployed or modified?**

The **[APPLICATION/SERVICE NAME]** is modified most frequently because it is one of the customer-facing services and receives regular UI/API enhancements and bug fixes.

Its deployment is fully automated through CI/CD, so developers don't need direct access to the production cluster.

**8. Can you walk me through the deployment cycle of a UI fix from the moment a developer commits to the dev branch until it reaches production?**

The flow is:

`Developer → Git Commit → Pull Request → Build → Unit Tests → Code Scan → Docker Build → Security Scan → ACR → Dev Deployment → Testing → UAT → Approval → Production`

For AKS deployments, the image is pushed to Azure Container Registry and the deployment is performed through Helm/ArgoCD or the approved Azure DevOps deployment mechanism.

After production deployment, we perform smoke testing and monitor application health, errors, latency, and availability.

**9. Do you use Helm or direct YAML files for deployments?**

I prefer Helm for enterprise applications because it provides reusable templates and makes environment-specific configuration easier to manage.

We maintain values files for environments such as:

`values-dev.yaml`

`values-uat.yaml`

`values-prod.yaml`

For simple Kubernetes resources, direct YAML can also be appropriate, but for multiple microservices and environments, Helm provides better maintainability.

**10. If a feature is not working properly after deployment, what specific command would you use to roll back to the previous version?**

For a Kubernetes Deployment, I can use:

```bash
kubectl rollout history deployment/<deployment-name> -n <namespace>
kubectl rollout undo deployment/<deployment-name> -n <namespace>


If Helm is being used:

helm history <release> -n <namespace>
helm rollback <release> <revision> -n <namespace>


After rollback, I verify pod health and application functionality.

11. What exactly does the terraform init command do?

terraform init initializes the Terraform working directory.

It primarily:

Downloads required providers.
Initializes the backend.
Downloads required modules.
Configures Terraform's working environment.
Prepares the directory for plan and apply.

For example:

terraform init
terraform validate
terraform plan


12. If you update a TLS version in two shared modules but terraform plan shows zero changes, what did you do wrong or miss?

I would first verify whether the updated child module is actually being used by the root module and whether the change is part of the Terraform configuration being executed.

I would check:

Correct branch and working directory
Module source/path
Whether the module version changed
Whether terraform init refreshed the module
Whether the TLS variable is actually passed to the child modules
Whether the resource supports that attribute
Terraform state and current configuration

If the module source/version was changed or the child module code was updated, I would run:

terraform init -upgrade
terraform plan


I would not assume that zero changes means everything is correct; I would verify the module dependency and actual resource configuration.

13. Do you need to run terraform init again after modifying child modules?

Not necessarily.

If I only modify the source code of a local child module, Terraform generally does not require a new init.

However, if I change the module source, version, backend, or provider requirements, then I may need to run terraform init again.

For example:

terraform init -upgrade


is useful when I need to refresh module/provider versions.

14. How would you delete only one specific resource using Terraform without deleting the code permanently?

If I want Terraform to destroy only one resource while keeping its configuration in the code, I can use a targeted destroy:

terraform destroy -target=azurerm_resource.example


However, targeted operations should be used carefully because they can bypass the normal dependency graph.

If the intention is simply to remove a resource from Terraform management without deleting the actual Azure resource, that's different; I would use:

terraform state rm <resource>


So I first clarify whether the requirement is to destroy the Azure resource or remove it from Terraform state.

15. If a VM is lost/corrupted, how do you use the "taint" feature to ensure it is recreated during the next pipeline run?

In older Terraform versions, I can use:

terraform taint azurerm_linux_virtual_machine.example


Then:

terraform plan
terraform apply


Terraform marks the resource for replacement.

In modern Terraform, the preferred approach is:

terraform apply -replace="azurerm_linux_virtual_machine.example"


This is more explicit and avoids modifying the state just to force replacement.

16. Have you maintained a Business Continuity Plan (BCP) or Disaster Recovery strategy for your environment?

Yes. I have been involved in DR/BCP planning from the infrastructure and application perspective.

We define:

RPO
RTO
Backup strategy
Recovery sequence
Secondary region
Database replication
Application deployment strategy
DNS/traffic failover
Dependency recovery
DR testing

The important point is that DR is not simply having backups; we need a tested process to restore the complete application stack.

17. What kind of databases do you use, and how do you manage their backups?

We have worked with relational databases such as Azure SQL and also other database technologies depending on the application.

For Azure-managed databases, I use native backup capabilities, point-in-time restore, retention policies, and geo-redundancy where required.

For critical applications, I align the backup and replication strategy with the application's RPO and RTO requirements.

18. How frequently are backups performed in your project?

The frequency depends on the criticality of the application and the RPO.

For example, daily backups may be sufficient for some systems, while critical databases may require continuous transaction log backups or point-in-time recovery.

I don't define backup frequency purely from an infrastructure perspective; I first understand the business RPO.

19. Scenario: If you have daily backups at 9:00 PM but the application fails at 5:00 PM the next day, how would you recover the lost 18 hours of data?

If we only have a single daily backup at 9 PM and the failure happens at 5 PM the next day, that backup alone cannot recover the subsequent 20-hour period accurately.

For a production system, I would use point-in-time recovery, transaction log backups, continuous backup, database replication, or another mechanism based on the database technology.

The key is that if the business requires low RPO, daily backups alone are insufficient.

For example:

Full Backup → Transaction Logs/Continuous Backup → Point-in-Time Restore

This allows us to restore the database closer to the failure time.

20. How do you ensure that critical data is not lost between backup intervals?

I design the recovery strategy around the required RPO.

For critical databases, I can use continuous backup, transaction log backups, geo-replication, read replicas, or synchronous/asynchronous replication depending on the platform.

I also regularly test restores because having a backup does not guarantee that the backup is actually recoverable.

21. What is the purpose of Azure Service Bus?

Azure Service Bus is a managed messaging service used to reliably decouple applications and services.

It supports queues and topics/subscriptions.

For example:

Application A → Service Bus → Application B

This allows the producer and consumer to operate independently.

It provides capabilities such as retries, dead-letter queues, message ordering scenarios, duplicate detection, and reliable asynchronous communication.

22. What is Azure Front Door, and how does it function as a global load balancer?

Azure Front Door is a global Layer-7 application delivery service.

It receives HTTP/HTTPS traffic at Microsoft's edge locations and routes requests to healthy backend origins based on routing rules, health probes, latency, priority, or other configuration.

A typical architecture is:

Global Users → Azure Front Door/WAF → Region 1 AKS

→ Region 2 AKS

This provides global routing, application acceleration, WAF, SSL termination, and failover capabilities.

23. What are the differences between an Application Load Balancer (Layer 7) and a Network Load Balancer (Layer 4)?

Layer 7 load balancing understands application protocols such as HTTP/HTTPS and can perform host-based and path-based routing.

Layer 4 load balancing works at the transport layer and routes traffic primarily based on IP addresses and ports.

For example:

Layer 7 → HTTP/HTTPS, URL/path routing, headers

Layer 4 → TCP/UDP, IP/port-based routing

In Azure, Application Gateway is an example of a Layer-7 application delivery service, while Azure Load Balancer is primarily Layer 4.

24. What protocols do each of these load balancers support?

Layer-7 application load balancers primarily handle HTTP and HTTPS.

Layer-4 network load balancers can handle TCP and UDP traffic.

The choice depends on the application protocol and whether we need application-level routing or simple network-level load balancing.

25. In your AKS cluster, do you use Ingress or a Service Mesh like Istio?

We primarily use Kubernetes Ingress for north-south application traffic.

Ingress handles HTTP/HTTPS routing into the cluster.

A service mesh such as Istio would be considered if we need advanced east-west traffic management, mTLS, service-to-service authorization, traffic splitting, retries, circuit breaking, and detailed service-level telemetry.

I would not introduce a service mesh unless the application architecture actually requires those capabilities because it also adds operational complexity.

26. How does an Ingress controller redirect traffic to specific services using host-based or path-based rules?

The Ingress resource defines routing rules.

For example:

rules:
  - host: api.example.com
    http:
      paths:
        - path: /users
          backend:
            service:
              name: user-service
              port:
                number: 80


The Ingress Controller reads these rules and configures the underlying proxy/load balancer.

So:

api.example.com/users → user-service

api.example.com/orders → order-service

The Service then routes traffic to the matching pods.

27. Why do we need Private Endpoints in Azure?

Private Endpoint provides private connectivity to supported Azure PaaS services through a private IP address in our VNet.

For example:

AKS/VM → Private Endpoint → Azure SQL/Storage/Key Vault

This keeps traffic on private connectivity rather than requiring public internet access.

It improves security and helps meet enterprise network isolation requirements.

28. If a VM in a private subnet needs to access an API on the internet, how does it interact with the outside world?

The VM can use controlled outbound connectivity through NAT Gateway or another centralized egress solution.

For example:

Private VM → NAT Gateway → Internet API

The VM does not need a public IP.

If the organization requires centralized inspection, traffic can instead go through Azure Firewall:

VM → Route Table → Azure Firewall → Internet

I prefer controlled outbound access with explicit rules rather than giving private workloads unrestricted internet access.

29. Are you aware of security compliances like HIPAA?

Yes. I understand that frameworks such as HIPAA, PCI DSS, SOC 2, ISO 27001, and others impose requirements around security, access control, encryption, auditing, monitoring, data protection, and operational processes.

From a DevOps perspective, I focus on implementing controls such as:

Least-privilege RBAC
Encryption at rest and in transit
Key Vault
Private networking
Logging and auditing
Vulnerability scanning
Secret management
Policy enforcement
Access reviews

Compliance is not only a technical configuration; it also involves organizational processes, governance, and evidence.

31. How would you automatically apply a large set of policies, 100+, to every new subscription immediately upon purchase?

I would use Azure Management Groups and Azure Policy rather than manually applying policies to every subscription.

Policies can be assigned at the Management Group level so that subscriptions created under that hierarchy automatically inherit them.

For enterprise governance, I would use Policy Initiatives to group multiple policies into a single logical definition.

The architecture is:

Root Management Group → Platform Management Group → Subscription → Resources

Then:

Policy Initiative → Multiple Policies → Automatic Inheritance

I would automate subscription onboarding using Azure Landing Zone/management-group patterns and ensure policy assignments are part of the subscription provisioning process.

32. How do you handle and secure secrets within your CI/CD pipelines?

I avoid hardcoding secrets in YAML, Git repositories, Terraform code, or Dockerfiles.

I prefer Azure Key Vault integrated with the CI/CD platform.

The flow is:

Pipeline → Managed Identity/Service Connection → Key Vault → Secret

I also use secret scanning, RBAC, short-lived credentials where possible, and least-privilege access.

Secrets should be masked in pipeline logs and should never be printed during troubleshooting.

33. How do you use Terraform to create a Key Vault and populate it with secrets without exposing the values in plain text within your code?

The important point is that Terraform itself can store secret values in its state if it manages the secret resource, even when the value is marked sensitive.

So I avoid committing secret values into Terraform code.

For example, values can come from secure pipeline variables or an external secret-management system, and Terraform can use them as sensitive variables.

I also secure the Terraform backend with encryption, RBAC, restricted access, and appropriate state protection.

For highly sensitive production secrets, I prefer having Terraform create the Key Vault and access policies/RBAC, while secrets are populated by a secure secret-management process rather than hardcoding them in Terraform.

34. How do you use sensitive tags and pipeline variables to hide passwords?

I use secret/sensitive variables in the CI/CD platform and mark them as secret so their values are masked in logs.

In Terraform, I can define:

variable "db_password" {
  type      = string
  sensitive = true
}


However, sensitive = true mainly prevents Terraform from displaying the value in normal CLI output; it does not mean the value is absent from state.

Therefore, the backend state must also be properly secured.

I never put passwords inside resource tags because tags are metadata and are not a secure secret store.

35. What is an Azure DevOps Agent, and how do self-hosted agents communicate with an AKS cluster on a different VNet?

An Azure DevOps Agent is the machine that executes pipeline jobs and tasks.

For a self-hosted agent, the agent initiates outbound communication to Azure DevOps and receives work.

If the agent needs to communicate with an AKS cluster in another VNet, we establish private network connectivity between the networks, such as VNet peering, VPN, or ExpressRoute depending on the architecture.

For a private AKS cluster, the agent needs network/DNS connectivity to the private API server endpoint and appropriate Azure/Kubernetes permissions.

The flow is:

Azure DevOps → Self-hosted Agent → Private Network Connectivity → AKS API Server → Kubernetes

Security is controlled using RBAC, NSGs, routing, DNS, and least-privilege identities.

36. Where are the logs from Log Analytics and application telemetry actually stored in Azure?

Azure Monitor Logs are stored in a Log Analytics Workspace.

Application Insights telemetry is also backed by Azure Monitor and is associated with the relevant Application Insights resource/workspace architecture.

For example:

Application → Application Insights → Azure Monitor/Log Analytics

AKS → Container Insights → Log Analytics Workspace

The data can then be queried using KQL.

37. How do you use KQL to retrieve specific data?

I use KQL to filter, aggregate, summarize, and correlate telemetry.

For example, to find failed requests:

requests
| where success == false
| summarize FailedRequests = count() by bin(timestamp, 5m)
| order by timestamp desc


To investigate exceptions:

exceptions
| where timestamp > ago(1h)
| project timestamp, type, outerMessage
| order by timestamp desc


For production troubleshooting, I typically start with a time range, filter by application/service, identify errors, and then correlate the request with dependencies and exceptions.

38. Have you been involved in cost optimization or right-sizing infrastructure?

Yes.

I have worked on right-sizing VM and AKS node pools, reviewing CPU/memory utilization, removing unused resources, optimizing autoscaling, controlling public IP usage, and reviewing storage and networking costs.

I use actual utilization and workload patterns rather than reducing capacity blindly.

The objective is:

Required Performance + Availability + Security + Minimum Waste

39. Can you give a specific example of how you saved costs in a project?

One example was AKS node-pool optimization.

We analyzed CPU and memory utilization over a sustained period and found that the node pool was consistently over-provisioned. We reviewed pod resource requests/limits and adjusted them based on actual usage.

Then we optimized the node-pool VM size and configured appropriate Cluster Autoscaler minimum and maximum values.

We also removed unused resources such as unnecessary public IPs and reviewed non-production environments for shutdown schedules.

The result was a reduction in infrastructure cost while maintaining the required application performance and availability.

The important part was that we used monitoring data before making the change, validated the workload after right-sizing, and kept sufficient headroom for traffic spikes.
