# Module 1 — Introduction to Application Support

> **Level:** Beginner  
> **Module:** 1  
> **Course:** Application Support Engineer  
> **Goal:** Understand what Application Support is, what an Application Support Engineer does, how production support works, and how support teams operate in real companies.

---

# 1. What is Application Support?

**Application Support** is the process of monitoring, maintaining, troubleshooting, and supporting software applications after they are developed and deployed.

In simple words:

> **Application Support means making sure an application continues to work correctly for users and the business.**

An application may work perfectly when developers build it, but problems can occur after it goes into production.

For example:

- Application is not opening
- Login is not working
- API is returning an error
- Database is unavailable
- Server is down
- Application is running slowly
- Disk is full
- SSL certificate has expired
- A scheduled job failed
- Payment service is not working
- Third-party API is unavailable

Application Support Engineers investigate these problems, identify the cause, fix them when possible, and escalate them when necessary.

---

# 2. Why Do We Need Application Support?

Software applications are usually running **24×7**.

Users may access an application:

- During business hours
- At night
- On weekends
- During holidays
- From different countries and time zones

Even if an application was developed perfectly, problems can still occur.

### Common reasons for application problems

```text
Server Failure
     ↓
Network Failure
     ↓
Database Failure
     ↓
Application Bug
     ↓
Configuration Problem
     ↓
High Traffic
     ↓
Third-Party Service Failure
     ↓
Deployment Problem
     ↓
Security Problem
```

Someone must continuously monitor and support the application.

That is where **Application Support** comes in.

---

# 3. Simple Real-World Example

Imagine you use an online shopping application.

```text
User
  ↓
Website
  ↓
Load Balancer
  ↓
Application Server
  ↓
Database
  ↓
Payment Gateway
```

A customer places an order.

Everything works like this:

```text
Customer
   ↓
Login
   ↓
Select Product
   ↓
Add to Cart
   ↓
Place Order
   ↓
Payment
   ↓
Database
   ↓
Order Confirmation
```

Now imagine the customer gets:

```text
"Payment Failed"
```

The Application Support Engineer investigates:

```text
Is the application running?
        ↓
Is the API working?
        ↓
Is the database working?
        ↓
Is the payment gateway working?
        ↓
Are there errors in application logs?
        ↓
Was there a recent deployment?
        ↓
What is the actual root cause?
```

The support engineer then works toward restoring the service.

---

# 4. Application Support in Simple Words

Think of an application as a car.

The development team builds the car.

The Application Support team makes sure the car continues to run.

```text
Development Team
       ↓
Build Application
       ↓
Testing Team
       ↓
Test Application
       ↓
Deployment Team
       ↓
Deploy Application
       ↓
Application Support
       ↓
Monitor + Maintain + Troubleshoot
```

Application Support is therefore an important part of the software lifecycle.

---

# 5. What Does an Application Support Engineer Do?

An Application Support Engineer is responsible for keeping applications available, stable, and usable.

Typical responsibilities include:

- Monitor applications
- Monitor servers
- Check application health
- Analyze application logs
- Troubleshoot application issues
- Troubleshoot Linux servers
- Troubleshoot network problems
- Troubleshoot database issues
- Troubleshoot APIs
- Handle incidents
- Investigate alerts
- Resolve user problems
- Perform basic database queries
- Restart services when required
- Perform application deployments
- Validate deployments
- Escalate complex issues
- Communicate with developers
- Communicate with infrastructure teams
- Create incident reports
- Create Root Cause Analysis documents
- Maintain support documentation
- Participate in on-call support

---

# 6. Skills Required for Application Support

An Application Support Engineer usually needs knowledge of multiple technologies.

```text
                    Application Support
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
      Linux             Networking          Database
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ↓
                          APIs
                           ↓
                       Monitoring
                           ↓
                          AWS
                           ↓
                         DevOps
```

Important skills include:

### Technical Skills

- Linux
- Networking
- Shell scripting
- SQL
- Databases
- HTTP/HTTPS
- REST APIs
- Web servers
- Application servers
- Git
- CI/CD
- Docker
- Kubernetes
- Cloud
- Monitoring
- Logging
- Security

### Support Skills

- Troubleshooting
- Incident management
- Problem solving
- Root Cause Analysis
- Documentation
- Communication
- Escalation
- Time management

---

# 7. Application Support vs Software Development

These roles are related but different.

## Software Developer

The developer primarily focuses on:

- Writing code
- Building features
- Fixing bugs
- Designing applications
- Testing code
- Improving application functionality

## Application Support Engineer

The support engineer primarily focuses on:

- Keeping applications running
- Monitoring applications
- Investigating incidents
- Analyzing logs
- Troubleshooting infrastructure
- Troubleshooting application issues
- Supporting users
- Restoring services
- Escalating issues

### Simple Difference

```text
Developer
   ↓
Builds the application

Application Support
   ↓
Keeps the application running
```

However, there is overlap.

A Support Engineer may need to understand code and sometimes modify scripts or configuration files.

---

# 8. Application Support vs DevOps

These roles can overlap significantly.

## Application Support

Main focus:

```text
Application Availability
Application Health
Incident Resolution
Troubleshooting
User Support
Production Support
```

## DevOps

Main focus:

```text
Automation
CI/CD
Infrastructure
Deployment
Cloud
Infrastructure as Code
Developer Productivity
```

### Example

Suppose an application deployment fails.

The Application Support Engineer may:

```text
Check application status
Check logs
Check error messages
Validate service
Report the problem
```

The DevOps Engineer may:

```text
Check pipeline
Check infrastructure
Check deployment configuration
Check container
Check Kubernetes
Fix deployment automation
```

Both teams may work together.

---

# 9. Application Support vs System Administrator

A System Administrator generally focuses more on infrastructure and operating systems.

### System Administrator

```text
Servers
Linux
Windows
Users
Storage
Networking
Security
Backups
Infrastructure
```

### Application Support Engineer

```text
Application
Application Logs
APIs
Database
Application Servers
Business Processes
Incidents
Production Issues
```

There is still significant overlap.

For example, if an application is down because a Linux service has stopped, both teams may be involved.

---

# 10. Application Support vs SRE

**SRE** stands for:

> Site Reliability Engineering

SRE focuses heavily on reliability, scalability, automation, and engineering practices.

### Application Support

```text
Monitor
Investigate
Troubleshoot
Resolve
Escalate
Document
```

### SRE

```text
Monitor
Automate
Engineer
Improve Reliability
Scale Systems
Reduce Manual Work
Improve Observability
```

A useful way to think about it:

```text
Application Support
       ↓
Keep the application working

SRE
       ↓
Engineer the system so it remains reliable
       with less manual intervention
```

---

# 11. What is Production Support?

**Production Support** means supporting applications that are currently being used by real users.

Production is the environment where the actual business application operates.

Example:

```text
Development
     ↓
Testing
     ↓
UAT
     ↓
Staging
     ↓
Production
```

Production is generally the most critical environment.

A problem in development may affect developers.

A problem in production may affect:

- Customers
- Employees
- Revenue
- Business operations
- Reputation

Therefore, production support requires careful handling.

---

# 12. What is a Production Environment?

The **Production Environment** is the live environment where the actual application is running for real users.

For example:

```text
www.example.com
```

may point to the production application.

A simplified production architecture might look like:

```text
                    Internet
                       |
                       ↓
                    DNS
                       |
                       ↓
                Load Balancer
                       |
             ┌─────────┴─────────┐
             ↓                   ↓
          Server 1            Server 2
             |                   |
             └─────────┬─────────┘
                       ↓
                   Database
                       |
                       ↓
                External APIs
```

---

# 13. Development Environment

The development environment is used by developers to build and modify the application.

Example:

```text
Developer
    ↓
Write Code
    ↓
Run Application
    ↓
Test Changes
```

Problems in development usually do not directly affect customers.

---

# 14. Testing / QA Environment

The testing environment is used by testers and QA engineers.

Purpose:

- Test new features
- Test bug fixes
- Perform regression testing
- Test application behavior
- Find defects

Typical flow:

```text
Developer
   ↓
Development
   ↓
Build
   ↓
QA
   ↓
Testing
```

---

# 15. UAT Environment

**UAT** stands for:

> User Acceptance Testing

The UAT environment allows business users or customers to validate whether the application meets business requirements.

Example:

```text
Development
      ↓
QA
      ↓
UAT
      ↓
Production
```

---

# 16. Staging Environment

Staging is generally designed to closely resemble production.

It is often used for final validation before production deployment.

```text
Staging
   ≈
Production
```

The exact architecture varies from organization to organization.

---

# 17. Production vs Non-Production

### Non-Production

Examples:

```text
Development
QA
Testing
UAT
Staging
```

### Production

```text
Live Application
Real Users
Real Business Data
Real Transactions
```

### Important Rule

> Never treat production like a playground.

Before making a production change, always understand:

- What are you changing?
- Why are you changing it?
- What is the impact?
- Is approval required?
- Is there a rollback plan?
- How will you validate the change?

---

# 18. Application Support Levels

Application Support is commonly divided into different levels.

```text
L1
 ↓
L2
 ↓
L3
```

---

# 19. L1 Support

L1 means **Level 1 Support**.

L1 is usually the first point of contact.

Typical responsibilities:

- Receive incidents
- Create tickets
- Perform basic troubleshooting
- Follow runbooks
- Check dashboards
- Check application availability
- Perform basic health checks
- Collect information
- Communicate with users
- Escalate complex issues

Example:

```text
User:
"I cannot login."

L1:
Check application availability
Check known incidents
Check user account
Follow login troubleshooting guide
Collect error message
```

If L1 cannot resolve the issue, it may be escalated.

---

# 20. L2 Support

L2 means **Level 2 Support**.

L2 performs deeper technical investigation.

Typical responsibilities:

- Analyze application logs
- Analyze server logs
- Troubleshoot Linux
- Troubleshoot APIs
- Run SQL queries
- Analyze database issues
- Investigate application configuration
- Restart services
- Investigate performance issues
- Perform advanced troubleshooting

Example:

```text
L1
 ↓
Login issue
 ↓
L2
 ↓
Check application logs
 ↓
Check database
 ↓
Find authentication error
```

---

# 21. L3 Support

L3 means **Level 3 Support**.

L3 generally involves highly specialized engineers, developers, architects, or product experts.

Typical responsibilities:

- Complex application bugs
- Source code investigation
- Architecture issues
- Deep database problems
- Complex performance problems
- Application design problems
- Product-level issues
- Permanent fixes

Example:

```text
L1
 ↓
Basic investigation

L2
 ↓
Technical investigation

L3
 ↓
Code / Architecture investigation
```

---

# 22. L1 vs L2 vs L3

| Level | Main Responsibility |
|---|---|
| L1 | Basic support and initial investigation |
| L2 | Technical troubleshooting |
| L3 | Deep technical/product-level resolution |

Typical flow:

```text
User
 ↓
L1
 ↓
L2
 ↓
L3
 ↓
Development / Engineering
```

Not every issue needs to reach L3.

A good support organization resolves issues at the lowest appropriate level.

---

# 23. What is On-Call Support?

**On-call support** means an engineer is responsible for responding to production issues during a specified period.

For example:

```text
Monday
09:00 - 18:00 → Engineer A

18:00 - 00:00 → Engineer B

00:00 - 09:00 → Engineer C
```

If a critical alert occurs at 2 AM:

```text
Monitoring System
       ↓
Alert
       ↓
On-Call Engineer
       ↓
Investigation
       ↓
Resolution / Escalation
```

On-call schedules depend on the organization.

---

# 24. What is Shift-Based Support?

Many organizations provide 24×7 application support.

They use shifts.

Example:

```text
Morning Shift
06:00 - 14:00

Evening Shift
14:00 - 22:00

Night Shift
22:00 - 06:00
```

The exact timings vary by organization.

---

# 25. Shift Handover

A proper handover is extremely important.

The outgoing engineer should communicate:

- Open incidents
- Current problems
- Recent changes
- Ongoing deployments
- Known issues
- Pending actions
- Important alerts
- Customer impact
- Escalations
- Expected follow-ups

Example:

```text
INC-1024

Issue:
Payment API intermittently failing.

Impact:
Some users cannot complete payment.

Current Status:
Development team investigating.

Next Action:
Check again after deployment at 02:00.

Priority:
High
```

A poor handover can cause the next engineer to repeat the entire investigation.

---

# 26. What is a Ticket?

A **ticket** is a record used to track an issue, request, or task.

Common ticketing systems include:

- ServiceNow
- Jira
- Zendesk
- Freshservice

A ticket may contain:

```text
Ticket ID
Issue
Description
Priority
Severity
Impact
Affected Application
Affected Users
Timestamp
Investigation
Actions Taken
Resolution
Status
```

Example:

```text
Ticket: INC001245

Application:
Payment Service

Issue:
Payment API returning HTTP 500.

Impact:
Customers cannot complete payments.

Priority:
High

Status:
Investigating
```

---

# 27. Incident

An **incident** is an unplanned interruption or degradation of a service.

Examples:

- Application is down
- API is failing
- Users cannot login
- Database is unavailable
- Website is slow
- Payment service is failing

Simple definition:

> **Incident = Something that is not working as expected and needs restoration.**

---

# 28. Service Request

A **Service Request** is generally a request for something rather than a failure.

Examples:

```text
Create user account
Request application access
Request report
Request software installation
Request permission
Request information
```

### Difference

```text
Incident:
"Application is not working."

Service Request:
"I need access to the application."
```

---

# 29. Problem

A **Problem** is the underlying cause of one or more incidents.

Example:

```text
Incident:
Application crashes every Monday.

Investigation:
Memory usage continuously increases.

Root Cause:
Memory leak in application.

Problem:
Application memory leak.
```

Simple relationship:

```text
Problem
   ↓
Root Cause
   ↓
Multiple Incidents
```

---

# 30. Change

A **Change** is a planned modification to an IT environment.

Examples:

- Application deployment
- Configuration change
- Database change
- Server upgrade
- Security patch
- Network change
- Infrastructure change

Example:

```text
Change Request

Change:
Upgrade application from v2.1 to v2.2

Reason:
Security update

Risk:
Medium

Rollback:
Deploy v2.1
```

---

# 31. Incident vs Problem vs Change vs Request

| Type | Meaning |
|---|---|
| Incident | Something is broken |
| Problem | Why did it break? |
| Change | We intentionally modify something |
| Service Request | User asks for something |

Example:

```text
Application crashes
       ↓
Incident

Why does it keep crashing?
       ↓
Problem

Deploy a permanent fix
       ↓
Change

Give developer access
       ↓
Service Request
```

---

# 32. SLA — Service Level Agreement

**SLA** stands for:

> Service Level Agreement

An SLA defines the agreed service expectations between a service provider and customer/business.

It may define:

- Response time
- Resolution time
- Availability
- Support hours
- Priority
- Escalation requirements

Example:

```text
Critical Incident

Response Time:
15 minutes

Target Resolution:
2 hours
```

The exact values depend on the organization and contract.

---

# 33. Severity

Severity describes **how serious the technical/business impact is**.

A common model:

```text
P1 / Critical
P2 / High
P3 / Medium
P4 / Low
```

Organizations may use different naming conventions.

### Example

```text
P1:
Entire application unavailable

P2:
Major functionality unavailable

P3:
Limited users affected

P4:
Minor issue
```

---

# 34. Priority

Priority determines **how quickly an issue should be handled**.

Priority generally considers:

```text
Impact
+
Urgency
=
Priority
```

Example:

A problem affecting 10,000 customers is usually more urgent than a cosmetic issue affecting one internal user.

---

# 35. Impact

Impact answers:

> **How many users or business processes are affected?**

Example:

```text
1 user
     ↓
Low Impact

100 users
     ↓
Medium Impact

Entire customer base
     ↓
High Impact
```

---

# 36. Urgency

Urgency answers:

> **How quickly does this problem need attention?**

Example:

```text
Payment system failing
        ↓
Very High Urgency

Minor UI issue
        ↓
Low Urgency
```

---

# 37. Escalation

Escalation means transferring an issue to another team or higher support level when required.

### Functional Escalation

Moving an issue to a team with the required technical expertise.

```text
Application Support
        ↓
Database Team
```

### Hierarchical Escalation

Moving an issue to a higher management level.

```text
Engineer
   ↓
Team Lead
   ↓
Manager
   ↓
Senior Management
```

---

# 38. Application Support Workflow

A typical support workflow looks like this:

```text
              Alert / User Report
                       ↓
                 Create Ticket
                       ↓
                Identify Impact
                       ↓
                Assign Priority
                       ↓
              Initial Investigation
                       ↓
                 Check Logs
                       ↓
                Check Server
                       ↓
                Check Network
                       ↓
                Check Database
                       ↓
                Check Application
                       ↓
               Identify Root Cause
                       ↓
                 Apply Resolution
                       ↓
                  Validate Fix
                       ↓
              Communicate Resolution
                       ↓
                  Close Ticket
```

---

# 39. Real-World Example — Application Down

Imagine a company has an online banking application.

At 10:05 AM:

```text
Users report:

"Unable to access the application."
```

### Step 1 — Ticket

Create incident:

```text
INC001001
```

### Step 2 — Check Application

```text
Application Status:
DOWN
```

### Step 3 — Check Server

```bash
systemctl status application
```

Result:

```text
Service: FAILED
```

### Step 4 — Check Logs

```bash
journalctl -u application
```

You find:

```text
Database connection failed
```

### Step 5 — Check Database

Database is unavailable.

### Step 6 — Identify Root Cause

```text
Application
    ↓
Database Connection
    ↓
Database
    ↓
Database Service Down
```

### Step 7 — Escalate

Database team is contacted.

### Step 8 — Recovery

Database service is restored.

### Step 9 — Validate

Application becomes available.

### Step 10 — Close

Document:

```text
Root Cause:
Database service failure.

Impact:
Users could not access the application.

Resolution:
Database service restored.

Validation:
Application tested successfully.
```

This is real Application Support work.

---

# 40. Application Support Mindset

A good Application Support Engineer should not immediately assume:

```text
"Restart the server."
```

Instead, think systematically.

Ask:

```text
What is the actual problem?
        ↓
Who is affected?
        ↓
When did it start?
        ↓
What changed recently?
        ↓
What do the logs say?
        ↓
Is the server healthy?
        ↓
Is the network healthy?
        ↓
Is the database healthy?
        ↓
Are dependencies healthy?
        ↓
What is the evidence?
        ↓
What is the root cause?
```

### Golden Rule

> **Don't guess. Investigate using evidence.**

And another important production rule:

> **Never make a production change just because "it might fix it."**

Understand the impact and follow the organization's change/incident process.

---

# 41. Important Application Support Terminology

| Term | Meaning |
|---|---|
| Application | Software used to perform a business function |
| Production | Live environment used by real users |
| Incident | Unplanned service interruption or degradation |
| Problem | Underlying cause of incidents |
| Change | Planned modification to a system |
| Service Request | Request for a service or access |
| SLA | Agreed service level |
| Severity | Seriousness of an issue |
| Priority | How quickly an issue should be handled |
| Impact | Number/importance of affected users or services |
| Escalation | Moving an issue to another support level/team |
| RCA | Root Cause Analysis |
| Monitoring | Watching system/application health |
| Alert | Notification that something may require attention |
| Runbook | Step-by-step operational procedure |
| SOP | Standard Operating Procedure |
| On-call | Engineer responsible for responding during a support period |
| L1 | First-level support |
| L2 | Technical support |
| L3 | Advanced/product-level support |

---

# 42. Key Takeaways

After completing this module, you should understand:

- What Application Support is
- Why Application Support is required
- What an Application Support Engineer does
- Difference between Support, Development, DevOps, and System Administration
- What Production Support means
- Different application environments
- L1, L2, and L3 support
- On-call support
- Shift-based support
- Shift handover
- Incident
- Problem
- Change
- Service Request
- SLA
- Severity
- Priority
- Impact
- Urgency
- Escalation
- Basic Application Support workflow
- Importance of evidence-based troubleshooting

---

# 43. What You Will Learn Next

Now that we understand **what Application Support is**, we need to learn the technical foundation required to actually perform the job.

The next major topic is:

```text
Linux
  ↓
Linux Administration
  ↓
Linux Troubleshooting
  ↓
Application Support
```

Linux is extremely important because many production applications run on Linux servers.

In the next modules, we will learn how to:

- Manage Linux servers
- Manage users and groups
- Manage permissions
- Manage processes
- Manage services
- Analyze logs
- Monitor system resources
- Troubleshoot servers
- Troubleshoot applications running on Linux

---

# Module 1 Summary

```text
Application Support
        │
        ├── Application Monitoring
        ├── Incident Management
        ├── Troubleshooting
        ├── Production Support
        ├── Log Analysis
        ├── Database Support
        ├── API Support
        ├── Server Support
        ├── Network Troubleshooting
        ├── Cloud Support
        └── Root Cause Analysis
```

> **Application Support is not just about fixing errors.**
>
> It is about understanding the complete system, identifying problems using evidence, restoring services safely, communicating clearly, and preventing the same problem from happening again.
