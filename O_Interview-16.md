# DevOps Interview Ready Answers

## 1. How will you troubleshoot if your application is down?

**Answer:**

"I follow a top-down approach and validate each layer systematically."

1. Check monitoring tools (CloudWatch, Prometheus, Grafana, Datadog) for alerts.
2. Verify application health and logs.
3. Check Kubernetes/EC2 instance status.
4. Validate CPU, Memory, Disk, Network utilization.
5. Verify Load Balancer target health.
6. Check DNS resolution.
7. Verify database connectivity and latency.
8. Validate dependent services (Redis, MQ, APIs).
9. Check recent deployments or infrastructure changes.
10. Rollback immediately if deployment caused the issue and perform RCA.

---

# 2. Explain what happens when end users hit flipkart.com. Explain network traffic flow.

**Answer:**

```
User Browser
      │
DNS Resolution (Route53/Cloudflare)
      │
Nearest CDN (CloudFront)
      │
AWS WAF
      │
Application Load Balancer
      │
Kubernetes Ingress / Nginx
      │
Application Pods
      │
Redis Cache
      │
Database (RDS/Aurora)
      │
Response returned to user
```

Traffic Flow:

- Browser resolves DNS.
- CDN serves static content.
- Dynamic requests reach WAF.
- Load Balancer distributes traffic.
- Ingress routes request to application.
- Application checks Redis cache.
- If cache miss, query database.
- Response goes back through ALB → CDN → Browser.

---

# 3. How will you implement FinOps strategy in cloud environment?

**Answer:**

"I focus on Visibility, Optimization, Governance, and Automation."

- Apply resource tagging (Project, Owner, Environment).
- Use Cost Explorer and AWS Budgets.
- Enable Cost Allocation Tags.
- Purchase Savings Plans/Reserved Instances for predictable workloads.
- Use Spot Instances for batch jobs.
- Auto Scaling to avoid overprovisioning.
- Schedule non-production resources.
- Move infrequently accessed data to cheaper storage classes.
- Continuous cost monitoring using CloudWatch and Cost Anomaly Detection.

---

# 4. How would you set IOPS for an EBS volume?

**Answer:**

- Select the appropriate EBS volume type.
- Use **io1/io2** for provisioned IOPS workloads.
- Configure required IOPS based on application needs.
- Monitor CloudWatch metrics:
  - VolumeQueueLength
  - ReadLatency
  - WriteLatency
  - BurstBalance
- Modify volume online if higher IOPS are required.

Example:

- Database → io2
- General application → gp3 (configure IOPS independently)

---

# 5. How would you leverage costing in S3 bucket's data?

**Answer:**

- Enable Lifecycle Policies.
- Move old objects:
  - Standard
  - Standard-IA
  - Intelligent Tiering
  - Glacier Instant Retrieval
  - Glacier Flexible Retrieval
  - Deep Archive
- Delete obsolete objects automatically.
- Enable S3 Storage Lens.
- Enable Intelligent Tiering.
- Compress logs before storing.
- Monitor storage using Cost Explorer.

---

# 6. A machine in private subnet wants to access Internet. How would you do that?

**Answer:**

- Launch a NAT Gateway in a public subnet.
- Attach Elastic IP to NAT Gateway.
- Update private subnet route table:

```
0.0.0.0/0 → NAT Gateway
```

- Public subnet route table:

```
0.0.0.0/0 → Internet Gateway
```

Private instances can access the internet but cannot receive inbound internet traffic.

---

# 7. How would you connect Terraform to CI/CD pipeline?

**Answer:**

Typical Pipeline:

```
Git Push
   │
GitHub/GitLab/Jenkins
   │
Terraform fmt
Terraform validate
Terraform lint (tflint)
Terraform init
Terraform plan
Approval Stage
Terraform apply
```

Best Practices:

- Store state remotely (S3 + DynamoDB locking)
- Use IAM Role/OIDC authentication
- Store secrets in Vault or AWS Secrets Manager
- Separate Dev, QA, Prod workspaces
- Use manual approval before production apply

---

# 8. Why is Ansible Agentless?

**Answer:**

"Ansible communicates over standard SSH (Linux) or WinRM (Windows), so no agent installation is required on managed nodes."

Benefits:

- Easy deployment
- Lower maintenance
- Improved security
- No additional software on target servers
- Centralized management

---

# 9. How do you use secrets in Terraform?

**Answer:**

Never hardcode secrets.

Use:

- AWS Secrets Manager
- HashiCorp Vault
- Azure Key Vault
- GCP Secret Manager

Retrieve secrets dynamically:

```hcl
data "aws_secretsmanager_secret_version" "db" {
  secret_id = "prod-db-password"
}
```

Mark variables as:

```hcl
sensitive = true
```

Use IAM roles instead of static credentials.

---

# 10. You executed a production change and it caused an outage. What would be your next steps?

**Answer:**

1. Acknowledge the incident.
2. Rollback immediately.
3. Restore service first.
4. Notify stakeholders.
5. Validate application health.
6. Investigate root cause.
7. Collect logs and metrics.
8. Perform RCA.
9. Implement preventive measures.
10. Update documentation and automation.

Priority:

**Restore Service → RCA → Prevention**

---

# 11. Why are you looking for a job change?

**Answer:**

"I'm looking for an opportunity where I can work on larger-scale cloud infrastructure, modern DevOps practices, Kubernetes, Terraform, CI/CD, and automation. My current role has helped me build a strong foundation, and now I'm looking for more challenging projects, greater ownership, and opportunities to grow while contributing to the organization's cloud transformation."
