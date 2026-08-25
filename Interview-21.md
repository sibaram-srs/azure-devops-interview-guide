# EY — SRE + DevOps — Amex Interview Answers

**1. Can we start with your introduction?**

I have around 10 years of experience in DevOps and Cloud engineering, primarily working with Azure, Kubernetes, AKS, Terraform, CI/CD, Docker, Linux, monitoring, and automation. In my recent projects, I have been responsible for infrastructure automation, application deployments, Kubernetes operations, production troubleshooting, monitoring, security, and CI/CD. My focus as a senior engineer is on reliability, automation, scalability, and reducing operational issues.

**2. Do you have experience in Kubernetes?**

Yes, I have hands-on production experience with Kubernetes. I have worked with Deployments, StatefulSets, DaemonSets, Services, Ingress, ConfigMaps, Secrets, HPA, PDB, resource requests/limits, RBAC, Helm, node pools, scheduling, probes, troubleshooting, and upgrades.

**3. Have you used AKS?**

Yes. AKS is one of my primary areas of experience. I have worked on AKS provisioning, node pools, networking, ingress, autoscaling, workload deployment, monitoring, security, upgrades, and production troubleshooting.

**4. Do you have experience on other cloud platforms like AWS or GCP?**

My primary hands-on experience is with Azure. I have good conceptual understanding of AWS and GCP and can map equivalent services, but I would position Azure as my strongest production experience.

**5. Can you explain the difference between the image and the container?**

A Docker image is an immutable package containing the application code, runtime, libraries, and dependencies. A container is a running instance of that image.

In simple terms:

`Image = blueprint/package`

`Container = running instance of that image`

**6. How can we reduce the Docker image size?**

I use a minimal base image, multi-stage builds, `.dockerignore`, remove unnecessary packages and files, avoid installing development dependencies in the final image, and combine related RUN commands where appropriate.

For example, with a multi-stage build, I compile the application in a builder image and copy only the required runtime artifacts into a smaller final image.

**7. Can you explain the structure of a Dockerfile and what is considered while writing one?**

A typical Dockerfile contains `FROM`, `WORKDIR`, `COPY`, `RUN`, `ENV`, `EXPOSE`, and `ENTRYPOINT` or `CMD`.

While writing it, I consider image size, security, caching, build reproducibility, non-root execution, dependency management, secrets, and startup behavior.

A typical structure is:

`FROM → WORKDIR → COPY → RUN → ENV → EXPOSE → ENTRYPOINT/CMD`

I also avoid putting passwords or secrets directly inside the Dockerfile.

**8. Can we override the entry point via CLI?**

Yes. Docker allows overriding the entrypoint using the `--entrypoint` option.

For example:

`docker run --entrypoint /bin/sh image-name`

This is useful for troubleshooting or starting a container with a different executable.

**9. Can you explain the architecture of Kubernetes?**

Kubernetes has a control plane and worker nodes.

The control plane includes API Server, Scheduler, Controller Manager, and etcd.

Worker nodes contain kubelet, container runtime, and kube-proxy/networking components.

The basic flow is:

`kubectl → API Server → Scheduler/Controllers → Worker Node → Kubelet → Container Runtime → Pods`

The API Server is the central communication point, while etcd stores cluster state.

**10. How would you troubleshoot a CrashLoopBackOff error in Kubernetes?**

First, I check the pod status and events:

`kubectl get pods`

`kubectl describe pod <pod-name>`

Then I check the previous container logs:

`kubectl logs <pod-name> --previous`

I investigate application errors, incorrect configuration, missing secrets, environment variables, image issues, resource limits, OOMKilled, and liveness probe failures.

My troubleshooting flow is:

`Pod status → Events → Previous logs → Application configuration → Resources → Probes → Dependencies`

I don't simply restart the pod; I identify why the container is repeatedly terminating.

**12. What is PDB (Pod Disruption Budget) in Kubernetes?**

PodDisruptionBudget defines how many pods of an application can be voluntarily disrupted at a given time.

For example, if I have five replicas and configure a PDB with `minAvailable: 4`, Kubernetes should maintain at least four available replicas during voluntary disruptions such as node maintenance.

PDB helps maintain application availability during maintenance and node operations.

**13. What is a ConfigMap and what happens if we change the value in a ConfigMap?**

ConfigMap stores non-sensitive configuration data such as environment-specific settings, URLs, feature flags, or application configuration.

If a ConfigMap is mounted as a volume, changes can be reflected in the mounted files after Kubernetes updates the volume, subject to application behavior.

If the ConfigMap is injected as environment variables, existing containers do not automatically receive the new environment variable values; the pods need to be recreated/restarted.

**14. If a value gets changed in a ConfigMap, does it automatically reflect in pods, or do we need to restart them?**

It depends on how the ConfigMap is consumed.

If it is consumed as environment variables, the running pod needs to be restarted to get the new values.

If it is mounted as a volume, Kubernetes can update the mounted files eventually, but the application must be capable of re-reading the changed configuration.

In production, I usually manage this carefully through deployment automation, for example by triggering a rollout when configuration changes require application restart.

**15. Can you explain the difference between Deployment, StatefulSet, and DaemonSet?**

Deployment is mainly used for stateless applications where pods are interchangeable.

StatefulSet is used for stateful applications that require stable identity, stable naming, and persistent storage.

DaemonSet ensures a pod runs on every eligible node.

Simple comparison:

`Deployment → Stateless applications`

`StatefulSet → Stateful applications`

`DaemonSet → Node-level workloads`

**16. What are the specific use cases for a DaemonSet?**

DaemonSets are commonly used for node-level workloads such as log collection agents, monitoring agents, security agents, network plugins, and storage-related agents.

For example, if I need a monitoring agent on every AKS node, I can deploy it as a DaemonSet. When a new node joins, the DaemonSet automatically schedules the agent there.

**17. What is a service in Kubernetes, and what are the different types of services?**

A Kubernetes Service provides a stable network endpoint for accessing a group of pods.

The common service types are:

- `ClusterIP` — internal cluster communication
- `NodePort` — exposes the service on a node port
- `LoadBalancer` — exposes the service through a cloud load balancer
- `ExternalName` — maps the service to an external DNS name

The Service uses label selectors to route traffic to the appropriate pods.

**18. Are you using Enterprise-based Terraform in your setup?**

Yes, I have worked with enterprise Terraform practices including remote state, reusable modules, environment separation, variable management, state locking, RBAC, code reviews, CI/CD-based Terraform execution, and controlled plan/apply processes.

The idea is to avoid engineers making uncontrolled infrastructure changes manually and maintain infrastructure as code.

**19. What is a data block in Terraform?**

A Terraform data block is used to retrieve information about an existing resource or external data source without creating or managing that resource.

For example:

```hcl
data "azurerm_resource_group" "example" {
  name = "existing-rg"
}


Then I can reference its attributes from Terraform.

20. Can a data block call any resource, or must that resource have been created through Terraform?

It does not have to be created through Terraform.

A data block can query an existing resource as long as Terraform has a supported data source for that resource.

For example, if a resource was manually created in Azure, Terraform can still retrieve its information using a supported data source.

21. What is the use of for_each and count in Terraform?

Both are used to create multiple instances of a resource.

count is useful when resources are based mainly on a numeric count or simple list.

for_each is better when each resource has a unique key or configuration.

For example:

resource "azurerm_resource_group" "rg" {
  for_each = var.resource_groups

  name     = each.key
  location = each.value.location
}


I generally prefer for_each for enterprise infrastructure when resources have meaningful identities because it provides more predictable resource addressing.

22. If someone makes manual changes in the console but terraform apply does not show those changes, what might be the reason?

First, I would run terraform plan and verify the state.

Possible reasons are that the manually changed attribute is not managed by Terraform, is ignored using lifecycle.ignore_changes, the state is stale or the change is outside the resource attributes Terraform is tracking.

If the attribute is managed by Terraform and differs from the configuration/state, Terraform should normally detect drift during refresh/plan.

The key point is that Terraform only manages what is defined and tracked in its configuration/state.

23. How do you handle a scenario where Module A is dependent on Module B, but Module B requires an output value from Module A?

That is a circular dependency and I would first redesign the module boundaries.

Usually, one common solution is to move the shared value into a separate foundational module.

For example:

Common/Foundation Module → Module A

Common/Foundation Module → Module B

Instead of:

Module A → Module B → Module A

I avoid circular dependencies because Terraform needs a Directed Acyclic Graph to determine the correct execution order.

24. Can a circular dependency be resolved in Terraform?

Not directly by forcing Terraform to execute it.

Terraform requires a dependency graph without cycles.

The correct solution is to redesign the architecture, split shared resources into another module, pass values through variables/outputs appropriately, or remove the unnecessary dependency.

25. Which CI/CD tools have you worked on?

My primary experience is with Azure DevOps. I have also worked with GitHub Actions, Jenkins concepts, Git, Docker, Helm, and ArgoCD depending on the project.

I have implemented pipelines for build, testing, security scanning, Docker image creation, image publishing, Terraform, and Kubernetes deployments.

26. Do you have any experience with Jenkins or GitHub Actions?

Yes. My strongest experience is Azure DevOps, but I have worked with Jenkins and GitHub Actions as well.

The concepts are similar: source control trigger, build, test, security scan, artifact/image creation, publishing, deployment, and post-deployment validation.

27. Can you explain the stages of your CI/CD pipeline?

A typical pipeline is:

Checkout → Build → Unit Test → Code Scan → Security/Dependency Scan → Docker Build → Image Scan → Push to ACR → Deploy → Smoke Test → Approval/Production

For infrastructure pipelines:

Checkout → Terraform Format → Validate → Init → Plan → Approval → Apply

I keep production deployment controlled with approvals and proper validation.

28. Have you worked on application-specific pipelines, such as microservices?

Yes. I have worked with pipelines for microservices where each service has its own build and deployment lifecycle.

A typical flow is:

Git → Build → Unit Test → Sonar/Security Scan → Docker Build → Image Scan → ACR → Helm/Manifest Update → ArgoCD/AKS → Smoke Test

I also ensure that environment-specific configuration and secrets are not hardcoded in the pipeline.

29. Do you have experience working with Python or shell scripting?

Yes. I use Bash/Shell scripting regularly for Linux and DevOps automation, and I have used Python for automation and utility scripts.

Typical use cases include log processing, API calls, health checks, deployment automation, Azure operations, file processing, and repetitive operational tasks.

30. Which operating system do you use for your work, Linux or Windows?

I primarily work with Linux for DevOps activities, especially Ubuntu and Red Hat-based environments.

I also work with Windows where required, particularly for enterprise applications or specific tooling.

31. Have you performed any troubleshooting on a Linux machine?

Yes, extensively.

I troubleshoot CPU, memory, disk, processes, services, networking, permissions, logs, and connectivity.

Common commands I use include:

top, ps, free, df, du, iostat, ss, netstat, curl, ping, nslookup/dig, systemctl, journalctl, grep, and tail.

My approach is to first identify whether the problem is CPU, memory, disk, process, service, network, or application related.

32. Can you explain your work regarding monitoring using Prometheus and Grafana?

Yes. Prometheus is used for collecting and storing time-series metrics, while Grafana is used for visualization and dashboards.

In Kubernetes, we monitor metrics such as:

CPU and memory
Pod/container health
Node health
Pod restarts
Request rate
Error rate
Latency
Application-specific metrics

A typical flow is:

Application/Kubernetes → Prometheus → Grafana → Alerts

I have created dashboards and alerts to identify abnormal resource usage, application failures, high latency, pod restarts, and infrastructure issues.

33. Other than Prometheus and Grafana, have you used any other monitoring tools?

Yes. In Azure environments, I have worked with Azure Monitor, Log Analytics, Application Insights, and Container Insights.

I have also worked with centralized logging and alerting solutions depending on the project.

For troubleshooting, I generally correlate infrastructure metrics, Kubernetes metrics, application logs, and network information rather than relying on a single monitoring tool.

34. Do you have any questions for the interviewer?

Yes, I would ask:

"What are the main SRE/DevOps challenges the team is currently trying to solve?"
"What is the current Kubernetes and cloud architecture?"
"How is the on-call and production support model structured?"
"What would you consider success for this role in the first six months?"
"How much of the infrastructure and deployment process is currently automated?"
