# Cloud Computing Fundamentals — Notes

## 1. What is Cloud Computing?

Cloud computing means using computing resources over the internet instead of purchasing and maintaining every server, disk and network device yourself.

Examples of resources:
- Virtual machines
- Storage
- Databases
- Networking
- Containers
- Monitoring
- Security services

Instead of saying, “I need to buy a physical server,” we can say, “I need a virtual server for this workload.”

## 2. Why Cloud?

Traditional infrastructure usually requires:
- Hardware purchase
- Data-center space
- Power and cooling
- Network setup
- Hardware maintenance
- Capacity planning

Cloud changes this model. Resources can be created when needed and removed when no longer needed.

### Main benefits

- Pay-as-you-go
- Fast provisioning
- Scalability
- High availability options
- Global infrastructure
- Automation
- Managed services

## 3. Cloud Service Models

### IaaS — Infrastructure as a Service

You manage more of the infrastructure.

Examples:
- AWS EC2
- Azure Virtual Machines
- Google Compute Engine

Typical responsibility:
- OS
- Applications
- Configuration
- Security inside the OS

### PaaS — Platform as a Service

The provider manages more of the platform.

Examples:
- AWS Elastic Beanstalk
- Azure App Service
- Google App Engine

### SaaS — Software as a Service

You consume a finished application.

Examples:
- Gmail
- Microsoft 365
- Salesforce

## 4. Deployment Models

### Public Cloud
Infrastructure is provided by a cloud provider.

### Private Cloud
Infrastructure is dedicated to one organization.

### Hybrid Cloud
A combination of private/on-premises and public cloud.

### Multi-Cloud
Using more than one cloud provider.

## 5. Core AWS Building Blocks

For beginner-level AWS work, understand:

- IAM — identity and access
- EC2 — virtual servers
- VPC — networking
- S3 — object storage
- RDS — managed relational database
- CloudWatch — monitoring
- ELB — load balancing
- Auto Scaling — automatic capacity adjustment

## 6. Regions and Availability Zones

A Region is a geographical AWS location.

An Availability Zone is an isolated infrastructure location inside a Region.

High-availability designs often distribute resources across multiple Availability Zones.

## 7. Scalability vs Elasticity

**Scalability** means increasing or decreasing capacity.

**Elasticity** means the system can automatically adjust resources according to demand.

Example:
A website receives 100 users normally and 10,000 users during an event.

A well-designed cloud architecture can add capacity during the spike and remove unnecessary capacity afterward.

## 8. Shared Responsibility

Cloud security is shared.

The provider is responsible for security **of** the cloud infrastructure.

The customer remains responsible for many things **in** the cloud, depending on the service:
- IAM configuration
- Data
- Application security
- OS patching for self-managed VMs
- Network rules
- Secrets

## 9. Cloud Cost Thinking

Never create resources without understanding their lifecycle.

Before creating a resource, ask:
1. Why am I creating it?
2. What will it cost?
3. How long will it exist?
4. How will I delete it?
5. Does it need a public IP?
6. Can a managed service reduce operational work?

## 10. Learning Path

Beginner:
- Cloud concepts
- Regions/AZs
- IAM
- EC2
- S3

Intermediate:
- VPC
- RDS
- Load Balancer
- CloudWatch
- Auto Scaling

Advanced:
- Infrastructure as Code
- Containers
- Kubernetes
- CI/CD
- Observability
- High availability
- Disaster recovery
- Security architecture
