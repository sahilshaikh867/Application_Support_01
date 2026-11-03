# Cloud Computing Interview Questions

## Level 1 — Beginner

### 1. What is cloud computing?
Cloud computing provides computing resources such as servers, storage and databases over a network on demand.

### 2. What is IaaS?
Infrastructure as a Service provides infrastructure resources such as virtual machines and networking.

### 3. What is PaaS?
Platform as a Service provides a managed application platform so developers do not manage the complete underlying infrastructure.

### 4. What is SaaS?
Software as a Service provides a ready-to-use application.

### 5. What is a Region?
A Region is a geographical AWS infrastructure location.

### 6. What is an Availability Zone?
An Availability Zone is an isolated infrastructure location within an AWS Region.

### 7. What is scalability?
The ability to increase or decrease capacity.

### 8. What is elasticity?
The ability of a system to automatically adjust capacity based on demand.

---

## Level 2 — Intermediate

### 9. EC2 vs S3?
EC2 provides compute capacity. S3 provides object storage.

### 10. What is a VPC?
A Virtual Private Cloud is a logically isolated network in AWS.

### 11. What is IAM?
Identity and Access Management controls authentication and authorization for AWS resources.

### 12. Security Group vs NACL?
A Security Group is associated with network interfaces/instances and is stateful. A Network ACL operates at the subnet level and is stateless.

### 13. What is an AMI?
An Amazon Machine Image is a template used to launch EC2 instances.

### 14. What is an Elastic Load Balancer?
It distributes incoming traffic across registered targets.

### 15. What is Auto Scaling?
It adjusts compute capacity according to configured conditions and policies.

---

## Level 3 — Advanced

### 16. How would you design a highly available web application?

A common approach is:

```text
Users
  |
Load Balancer
  |
+---------+---------+
|                   |
AZ-1                AZ-2
EC2                  EC2
|                   |
+---------+---------+
          |
      RDS Multi-AZ
          |
         S3
```

The exact architecture depends on workload requirements.

### 17. How would you reduce AWS cost?

Consider:
- Right-sizing
- Scheduling non-production resources
- Removing unused resources
- Appropriate storage classes
- Reserved/committed pricing where justified
- Autoscaling
- Monitoring cost and usage

### 18. What is the shared responsibility model?
AWS secures the underlying cloud infrastructure. Customers remain responsible for security tasks assigned to them by the service model.

### 19. What is Infrastructure as Code?
Infrastructure as Code manages infrastructure through version-controlled configuration files.

Examples:
- Terraform
- AWS CloudFormation

### 20. What is observability?
Observability is the ability to understand system behavior using telemetry such as metrics, logs and traces.

---

## Scenario Questions

### Scenario 1

A website becomes slow every evening.

What would you check?

Expected thinking:
1. Load
2. CPU/memory
3. Network
4. Application logs
5. Database performance
6. Request latency
7. Traffic pattern
8. Scaling configuration

### Scenario 2

An EC2 server cannot be reached through SSH.

Check:

```text
Internet path
    ↓
Route table
    ↓
Subnet
    ↓
Security Group
    ↓
Network ACL
    ↓
Instance
    ↓
SSH service
```

Do not immediately blame the server.

### Scenario 3

AWS cost suddenly increases.

Investigate:
- Cost Explorer
- Newly created resources
- Data transfer
- Storage
- NAT Gateway usage
- Large compute resources
- Forgotten test environments

---

# Practical Interview Challenge

Explain this architecture in 5 minutes:

```text
Internet
   |
ALB
   |
Auto Scaling Group
   |
EC2 instances
   |
RDS

S3 → static/object storage
CloudWatch → monitoring
IAM → access control
```

The interviewer is testing whether you understand relationships between services, not whether you memorized definitions.
