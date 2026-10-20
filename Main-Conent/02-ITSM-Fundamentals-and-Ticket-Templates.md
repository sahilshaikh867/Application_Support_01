# ITSM Fundamentals & Practical Ticket Templates

> **Course:** Application Support  
> **Module:** 2  
> **Level:** Beginner → Industry  
> **File:** `02-ITSM-Fundamentals-and-Ticket-Templates.md`

---

# 1. What is ITSM?

Before understanding ITSM, let's understand one simple thing.

Imagine you are working in a company.

The company has:

- Websites
- Mobile applications
- Databases
- Servers
- APIs
- Networks
- Email systems
- Payment systems
- Internal applications

Now imagine that one day the company's application stops working.

Someone has to:

1. Receive the complaint
2. Create a ticket
3. Understand the problem
4. Check how serious it is
5. Assign it to the correct team
6. Fix the problem
7. Inform the user
8. Close the ticket
9. Make sure the same problem doesn't happen again

This complete way of managing IT services is called:

> **ITSM — IT Service Management**

In simple words:

> **ITSM is a proper process for providing, supporting, maintaining, and improving IT services.**

---

# 2. Why Do We Need ITSM?

Without ITSM, support teams can become very unorganized.

For example:

```text
User: Website is not working!

Engineer: Okay, I will check.

Another user: Payment is failing!

Engineer: Okay, I will check that too.

Manager: What is happening?

Engineer: I don't know.

Manager: Who is working on the issue?

Engineer: I don't know.

Manager: When will it be fixed?

Engineer: I don't know.
```

This is a bad support environment.

With ITSM:

```text
User reports problem
        ↓
Ticket created
        ↓
Ticket categorized
        ↓
Priority assigned
        ↓
Engineer assigned
        ↓
Issue investigated
        ↓
Issue fixed
        ↓
User informed
        ↓
Ticket closed
        ↓
Documentation updated
```

Now everyone knows what is happening.

---

# 3. ITIL Fundamentals

You will often hear the word **ITIL** in Application Support jobs.

ITIL stands for:

> **Information Technology Infrastructure Library**

ITIL provides guidelines and best practices for managing IT services.

You don't need to memorize ITIL like a theory subject.

Just understand the main idea:

> **ITIL helps organizations manage IT services in a structured way.**

For example:

```text
Something breaks
       ↓
Incident Management
       ↓
Fix the service
       ↓
Find why it happened
       ↓
Problem Management
       ↓
Prevent it from happening again
```

ITIL is commonly used as a framework for ITSM.

---

# 4. What is an IT Service?

A service is something provided to users that helps them perform their work.

Examples:

- Online banking
- Company email
- E-commerce website
- Employee login system
- Payment gateway
- HR portal
- Cloud application
- Customer support portal

For example:

```text
Company
   |
   +-- Email Service
   |
   +-- HR Service
   |
   +-- Payment Service
   |
   +-- Customer Portal
   |
   +-- Database Service
```

If employees use Gmail or Microsoft 365 for company email, email is an IT service.

---

# 5. What is an Incident?

An **incident** is something that causes an IT service to stop working properly.

Simple definition:

> **Incident = Something is broken or not working as expected.**

### Examples

```text
Website is down
Login is not working
Payment is failing
Application is slow
Database connection failed
API is returning errors
```

Example:

A customer tries to make a payment.

```text
Payment → Failed
```

This can be treated as an incident.

---

# 6. What is a Problem?

A problem is the **underlying reason behind one or more incidents**.

Let's understand this with an example.

Suppose:

```text
Application crashes
       ↓
Engineer restarts application
       ↓
Application works
```

But after two hours:

```text
Application crashes again
```

This means we have fixed the immediate incident, but we haven't found the actual reason.

Investigation shows:

```text
Memory Leak
    ↓
Application consumes RAM
    ↓
RAM becomes full
    ↓
Application crashes
```

The memory leak is the **problem/root cause**.

### Easy difference

| Incident | Problem |
|---|---|
| Something is broken | Why did it break? |
| Focuses on restoring service | Focuses on finding root cause |
| Immediate action | Long-term solution |
| Example: Website down | Example: Memory leak |

---

# 7. What is a Service Request?

A service request is something a user asks for that is normally part of the organization's services.

It is not necessarily a failure.

Examples:

- Create a new user
- Reset password
- Request application access
- Install software
- Request a laptop
- Request database access

Example:

```text
Employee:
I need access to the HR application.
```

This is a **Service Request**.

---

# 8. Incident vs Problem vs Service Request

Remember this simple table:

| Type | Meaning | Example |
|---|---|---|
| Incident | Something is broken | Website is down |
| Problem | Why something keeps breaking | Memory leak |
| Service Request | User needs something | Give me application access |

Easy memory trick:

```text
INCIDENT  → FIX IT
PROBLEM   → FIND WHY
REQUEST   → PROVIDE IT
```

---

# 9. What is an Event?

An event is something that happens in an IT environment.

Not every event is a problem.

For example:

```text
Server CPU = 40%
```

This is an event.

Another example:

```text
Backup completed successfully
```

This is also an event.

Events can come from:

- Servers
- Applications
- Databases
- Networks
- Monitoring tools

---

# 10. What is an Alert?

An alert is a notification that tells us:

> **"Hey, something needs your attention."**

Example:

```text
CPU Usage = 95%
```

Monitoring system:

```text
⚠ CPU usage is above 90%
```

That is an alert.

### Event vs Alert

```text
Event
 ↓
Something happened

Alert
 ↓
Something happened that needs attention
```

---

# 11. Incident Lifecycle

An incident normally goes through several stages.

```text
Incident Reported
       ↓
Ticket Created
       ↓
Ticket Assigned
       ↓
Investigation
       ↓
Resolution
       ↓
User Confirmation
       ↓
Ticket Closed
```

Let's understand each stage.

### 1. Reported

User reports the issue.

### 2. Ticket Created

A support ticket is created.

### 3. Assigned

The ticket is assigned to the correct engineer/team.

### 4. Investigation

Engineer starts checking logs, servers, database, APIs, etc.

### 5. Resolution

The problem is fixed or service is restored.

### 6. User Confirmation

User confirms that the service is working.

### 7. Closed

Ticket is officially closed.

---

# 12. Problem Management

Problem Management focuses on finding the root cause of incidents.

Example:

```text
Incident 1 → Application crashed
Incident 2 → Application crashed
Incident 3 → Application crashed
Incident 4 → Application crashed
```

Instead of restarting the application every time:

```text
Why is it crashing?
        ↓
Check logs
        ↓
Check memory
        ↓
Check code
        ↓
Find memory leak
        ↓
Fix memory leak
```

Now the problem is permanently addressed.

---

# 13. Change Management

Sometimes we need to change something in production.

Examples:

- Deploy new application version
- Change database configuration
- Increase server memory
- Modify firewall rule
- Update application configuration
- Upgrade software

These changes should not be performed randomly.

Example:

```text
Developer:
Let's change production database configuration.

Support Engineer:
Okay, but first:

What are we changing?
Why are we changing?
When will we change?
What is the risk?
What if something goes wrong?
What is the rollback plan?
```

This is the idea behind **Change Management**.

---

# 14. Request Fulfillment

Request Fulfillment means handling normal user requests.

Example:

```text
User requests VPN access
        ↓
Request created
        ↓
Manager approval
        ↓
IT team processes request
        ↓
VPN access provided
        ↓
Request closed
```

---

# 15. Incident Classification

Classification means deciding:

> **What type of issue is this?**

For example:

```text
Application
Database
Network
Server
Security
Access
Hardware
Cloud
```

Suppose:

```text
User cannot login.
```

Possible classification:

```text
Category: Application
Sub-category: Authentication
```

Classification helps route tickets to the right team.

---

# 16. Incident Categorization

Categorization means organizing incidents into meaningful categories.

Example:

```text
Application
 ├── Login
 ├── Payment
 ├── Performance
 ├── API
 └── Database Connection
```

This becomes useful when the company has thousands of tickets.

You can later analyze:

```text
How many payment incidents happened this month?

How many login incidents happened?

Which application has the most incidents?
```

---

# 17. Incident Prioritization

Not every incident has the same importance.

Imagine:

### Incident A

One employee cannot access an internal report.

### Incident B

The company's payment system is down.

Obviously:

```text
Incident B
```

should be handled first.

This is where **priority** comes in.

---

# 18. Severity

Severity tells us:

> **How serious is the incident?**

Example:

| Severity | Meaning |
|---|---|
| Sev 1 | Critical |
| Sev 2 | High |
| Sev 3 | Medium |
| Sev 4 | Low |

### Example

```text
Sev 1
Payment system completely down

Sev 2
Major feature not working

Sev 3
Small group of users affected

Sev 4
Minor issue
```

Companies may use different names, but the idea is similar.

---

# 19. Priority

Priority tells us:

> **How quickly should we work on this?**

Priority usually depends on:

```text
Impact + Urgency
```

For example:

```text
High Impact + High Urgency
        ↓
High Priority
```

---

# 20. Impact

Impact means:

> **How many users or how much business is affected?**

Example:

```text
1 employee affected
        ↓
Low Impact

100 employees affected
        ↓
Medium Impact

Entire company affected
        ↓
High Impact
```

For a customer-facing application:

```text
10 customers affected
        ↓
Lower impact

10,000 customers affected
        ↓
High impact
```

---

# 21. Urgency

Urgency means:

> **How quickly does the issue need to be handled?**

Example:

```text
Application will become unavailable in 10 minutes
```

This is highly urgent.

Another example:

```text
Minor UI issue
Can be fixed next week
```

Low urgency.

---

# 22. Impact vs Urgency vs Priority

Remember:

```text
IMPACT
↓
How many are affected?

URGENCY
↓
How quickly is action required?

PRIORITY
↓
How quickly should we work on it?
```

Example:

```text
Payment system down

Impact = Very High
Urgency = Very High
Priority = P1
```

---

# 23. Escalation

Sometimes the current engineer cannot solve the issue.

Then the issue is escalated.

Example:

```text
L1 Engineer
     ↓
Cannot solve
     ↓
L2 Engineer
     ↓
Still cannot solve
     ↓
L3 / Development Team
```

This is escalation.

---

# 24. Functional Escalation

Functional escalation means:

> **Sending the issue to another technical team because they have the required knowledge.**

Example:

```text
Application Support
       ↓
Database Team
```

Maybe the problem is related to SQL.

So the ticket is escalated to the Database Team.

---

# 25. Hierarchical Escalation

Hierarchical escalation means involving a manager or higher authority.

Example:

```text
Incident not resolved
        ↓
Team Lead
        ↓
Manager
        ↓
Senior Management
```

This can happen when:

- SLA is about to breach
- Major business impact exists
- Customer is affected
- Additional resources are required

---

# 26. SLA

SLA means:

> **Service Level Agreement**

It defines the expected service level between a service provider and customer/business.

For example:

```text
P1 Incident

Response Time: 15 minutes
Resolution Target: 2 hours
```

The exact values depend on the organization.

---

# 27. OLA

OLA means:

> **Operational Level Agreement**

An OLA is an internal agreement between teams.

Example:

```text
Application Support
        ↓
Database Team
```

Company SLA says:

```text
Customer issue must be handled within 2 hours.
```

Internal OLA may say:

```text
Database Team must respond within 30 minutes.
```

Simple difference:

```text
SLA → Customer / Business agreement

OLA → Internal team agreement
```

---

# 28. Response Time

Response time means:

> **How long it takes before the support team responds to the incident.**

Example:

```text
Ticket created: 10:00 AM
Engineer responds: 10:10 AM

Response Time = 10 minutes
```

---

# 29. Resolution Time

Resolution time means:

> **How long it takes to resolve the issue.**

Example:

```text
Issue reported: 10:00 AM
Issue resolved: 11:00 AM

Resolution Time = 1 hour
```

---

# 30. MTTR

MTTR commonly means:

> **Mean Time To Repair** or **Mean Time To Restore**

It tells us approximately how long it takes to restore a service after failure.

Example:

```text
Incident 1 → 30 min
Incident 2 → 60 min
Incident 3 → 90 min
```

Average:

```text
MTTR = (30 + 60 + 90) / 3

MTTR = 60 minutes
```

Lower MTTR is generally better.

---

# 31. MTTD

MTTD means:

> **Mean Time To Detect**

It tells us how long it takes to detect an issue.

Example:

```text
Application failed at 10:00
Monitoring detected it at 10:05

Detection Time = 5 minutes
```

The goal is generally to detect problems quickly.

---

# 32. MTBF

MTBF means:

> **Mean Time Between Failures**

It tells us the average time between failures.

Example:

```text
Application works for 100 hours
        ↓
Failure
        ↓
Application works for another 120 hours
        ↓
Failure
```

A higher MTBF generally indicates better reliability.

---

# 33. Major Incident Management

A major incident is a very serious incident that has a large business impact.

Examples:

```text
Banking application completely down
Payment system unavailable
Entire company unable to login
E-commerce website completely down
```

Major incidents require:

- Fast response
- Senior engineers
- Clear communication
- Regular updates
- Management involvement
- Proper documentation

---

# 34. Major Incident Process

A simple major incident flow:

```text
Major Incident Detected
          ↓
Major Incident Declared
          ↓
Incident Manager Assigned
          ↓
Technical Teams Joined
          ↓
Investigation
          ↓
Regular Updates
          ↓
Service Restored
          ↓
Incident Closed
          ↓
Post-Incident Review
```

---

# 35. Communication During Incidents

During a major incident, communication is extremely important.

Do not send messages like:

```text
Working on it.
```

This doesn't tell anyone much.

A better update:

```text
10:30 AM Update

The payment service is currently unavailable.
The Application and Database teams are investigating.

Impact:
Customers are currently unable to complete payments.

Next update:
We will provide another update in 30 minutes.
```

Good communication should answer:

```text
What happened?
Who is affected?
What are we doing?
What is the current status?
When is the next update?
```

---

# 36. Incident Closure

After fixing the issue, don't immediately close the ticket without documenting what happened.

A good closure should contain:

```text
Problem:
Payment API was returning HTTP 500 errors.

Cause:
Database connection pool was exhausted.

Action:
Restarted affected service and corrected connection pool configuration.

Result:
Payment processing is working normally.

Monitoring:
Application is being monitored for further errors.
```

---

# 37. Post-Incident Review

After a major incident, the team should discuss:

- What happened?
- Why did it happen?
- How was it detected?
- How long did it take to recover?
- What went well?
- What went wrong?
- How can we prevent it?

This is called a:

> **Post-Incident Review**

The goal is not to blame someone.

The goal is:

> **Learn from the incident and improve the system.**

---

# 38. Knowledge Management

Support engineers solve many problems repeatedly.

Instead of solving the same issue from zero every time, document the solution.

Example:

```text
Problem:
Application cannot connect to database.

Solution:
1. Check database status.
2. Check network connectivity.
3. Check credentials.
4. Check connection pool.
5. Restart application if required.
```

This can become a **Knowledge Base Article**.

Next time:

```text
Engineer sees issue
        ↓
Searches Knowledge Base
        ↓
Finds solution
        ↓
Resolves issue faster
```

---

# 39. Change Advisory Board (CAB)

CAB means:

> **Change Advisory Board**

CAB is a group of people who review important changes.

For example:

```text
Production Database Upgrade
```

Before the change:

```text
What is the risk?
What is the downtime?
What is the rollback plan?
When will it happen?
Who is responsible?
```

CAB helps make sure important changes are properly reviewed.

---

# 40. Standard Change

A standard change is:

> **A common, repeated, low-risk change that already has an approved process.**

Example:

```text
Resetting a standard user password
```

or:

```text
Adding a user to an already-approved group
```

The organization already knows:

```text
How to perform it
What risk exists
What steps to follow
```

---

# 41. Normal Change

A normal change is a change that requires proper review and approval.

Example:

```text
Upgrade production database
```

Typical flow:

```text
Change Request
      ↓
Risk Assessment
      ↓
Approval
      ↓
Schedule
      ↓
Implementation
      ↓
Testing
      ↓
Closure
```

---

# 42. Emergency Change

Sometimes waiting for the normal approval process can cause more damage.

Example:

```text
Critical security vulnerability
```

The company may need to patch production immediately.

This is an:

> **Emergency Change**

It still needs proper documentation and approval according to the organization's emergency process.

---

# 43. Standard vs Normal vs Emergency Change

| Change | Meaning |
|---|---|
| Standard | Common, low-risk, pre-approved |
| Normal | Requires planning and approval |
| Emergency | Needs immediate action |

Easy way to remember:

```text
STANDARD
↓
Routine

NORMAL
↓
Planned

EMERGENCY
↓
Urgent
```

---

# 44. Complete ITSM Picture

Now connect everything together.

```text
                    ITSM
                     |
        +------------+------------+
        |            |            |
    Incident      Problem       Change
        |            |            |
     Restore       Find Root     Control
     Service        Cause        Changes
        |
        ↓
   Ticket Created
        |
        ↓
 Classification
        |
        ↓
 Categorization
        |
        ↓
 Impact + Urgency
        |
        ↓
 Priority
        |
        ↓
 Assignment
        |
        ↓
 Investigation
        |
        ↓
 Escalation if needed
        |
        ↓
 Resolution
        |
        ↓
 Communication
        |
        ↓
 Closure
        |
        ↓
 Knowledge / RCA
```

---

# PART 2 — PRACTICAL TICKET TEMPLATES

Now let's move from theory to actual Application Support work.

In a real company, you will work with tickets almost every day.

A ticket should tell another engineer:

> **What happened, what has been checked, what is happening now, and what needs to happen next.**

---

# 45. Basic Incident Ticket Template

```text
Ticket ID:
INC-1001

Title:
Application Login Failure

Description:
Users are unable to login to the application.

Impact:
Approximately 50 users are affected.

Urgency:
High

Priority:
P2

Environment:
Production

Time Reported:
10:15 AM

Initial Investigation:
Application logs show authentication errors.

Actions Taken:
1. Checked application logs.
2. Checked authentication service.
3. Verified database connectivity.

Current Status:
Investigation in progress.

Assigned Team:
Application Support

Next Action:
Check authentication service configuration.
```

---

# 46. Simple Incident Ticket

For beginners, remember this structure:

```text
What happened?
Who is affected?
When did it start?
What have I checked?
What did I find?
What am I doing now?
```

Example:

```text
Issue:
Users cannot login.

Affected Users:
Approximately 20 users.

Started:
10:30 AM.

Checked:
Application logs and authentication service.

Finding:
Authentication API is returning HTTP 500.

Current Action:
Application team is investigating the API issue.
```

---

# 47. Resolved Incident Ticket

When an issue is fixed:

```text
Ticket ID:
INC-1002

Issue:
Customers were unable to complete payments.

Root Cause:
Payment API could not connect to the database.

Resolution:
Database connection issue was corrected and the payment service was restarted.

Validation:
Test payment completed successfully.

Impact:
Customers were unable to make payments for approximately 25 minutes.

Status:
Resolved.
```

---

# 48. Major Incident Ticket

```text
Major Incident ID:
MI-1001

Title:
Production Payment Service Down

Severity:
Sev 1

Impact:
All customers are unable to complete payments.

Start Time:
2:10 PM

Detection:
Monitoring alert.

Teams Involved:
- Application Support
- DevOps
- Database Team
- Network Team

Current Status:
Investigation in progress.

Initial Finding:
Payment API is returning HTTP 503 errors.

Actions:
1. Checked application health.
2. Checked database connectivity.
3. Checked recent deployments.
4. Rolled back the latest deployment.

Current Result:
Payment service is partially restored.

Next Update:
Another update will be provided in 15 minutes.
```

---

# 49. Incident Update Template

During an ongoing incident:

```text
Incident Update

Time:
3:30 PM

Current Status:
Investigation is still in progress.

Impact:
Customers are experiencing payment failures.

What We Checked:
- Application logs
- Database connectivity
- API health
- Recent deployment

Finding:
Errors started after the latest application deployment.

Action:
Team is currently rolling back the deployment.

Next Update:
Next update will be provided in 15 minutes.
```

---

# 50. Incident Resolution Update

```text
Resolution Update

The payment issue has been resolved.

Root Cause:
A configuration issue in the latest deployment caused the payment API to fail.

Action Taken:
The deployment was rolled back and the configuration was corrected.

Validation:
Test transactions are successful.

Current Status:
Payment service is operating normally.

Monitoring:
The service will continue to be monitored.
```

---

# 51. Service Request Ticket

```text
Request ID:
REQ-1001

Title:
Request Access to HR Application

Requested By:
Employee

Request:
Please provide access to the HR application.

Business Reason:
Required for daily HR-related work.

Approval:
Manager approval received.

Action:
HR application access has been provided.

Validation:
User successfully logged in.

Status:
Completed.
```

---

# 52. Access Request Template

```text
Request:
Database Read Access

User:
John

Application:
Reporting Application

Environment:
Production

Access Required:
Read-only

Business Reason:
User needs access to generate reports.

Approval:
Manager approval received.

Action:
Read-only access granted.

Status:
Completed.
```

---

# 53. Problem Ticket

Problem tickets are used when we want to find the root cause.

```text
Problem ID:
PRB-1001

Title:
Application Frequently Crashing

Related Incidents:
INC-1001
INC-1005
INC-1010

Problem Description:
The application has crashed multiple times during peak traffic.

Impact:
Users experience application downtime.

Investigation:
Application memory usage increases continuously.

Finding:
Possible memory leak.

Root Cause:
Memory leak in application component.

Permanent Fix:
Development team updated the affected component.

Validation:
Application has been stable for 7 days.

Status:
Resolved.
```

---

# 54. Root Cause Analysis (RCA) Template

RCA means:

> **Root Cause Analysis**

Use this after a serious incident.

```text
Incident:
Payment Service Outage

Date:
10 October 2026

Duration:
35 minutes

Impact:
Customers were unable to complete payments.

What Happened:
Payment API started returning errors.

Root Cause:
Database connection pool became exhausted.

Why Did It Happen?
A configuration change increased the number of application connections.

Why Was It Not Detected Earlier?
Monitoring did not have an alert for connection pool usage.

Resolution:
Configuration was corrected and the application was restarted.

Preventive Actions:
1. Add connection pool monitoring.
2. Create alert thresholds.
3. Review configuration changes.
4. Update deployment checklist.

Owner:
Application Team

Status:
Actions in progress.
```

---

# 55. Change Request Template

```text
Change ID:
CHG-1001

Title:
Production Database Upgrade

Change Description:
Upgrade the production database to the approved version.

Reason:
Current database version is reaching end of support.

Risk:
Application may experience compatibility issues.

Planned Date:
Saturday

Planned Time:
11:00 PM

Expected Downtime:
30 minutes

Implementation Plan:
1. Take database backup.
2. Stop application.
3. Upgrade database.
4. Start database.
5. Start application.
6. Perform application testing.

Validation:
Run application health checks and test transactions.

Rollback Plan:
Restore previous database version from backup.

Approvals:
Application Team
Database Team
Change Manager

Status:
Scheduled.
```

---

# 56. Standard Change Template

```text
Change Type:
Standard Change

Change:
Create standard application user account.

Risk:
Low

Steps:
1. Verify request.
2. Verify approval.
3. Create account.
4. Assign standard permissions.
5. Test login.
6. Update ticket.

Rollback:
Disable account if required.

Status:
Completed.
```

---

# 57. Emergency Change Template

```text
Change ID:
EC-1001

Title:
Emergency Security Patch

Reason:
Critical security vulnerability discovered.

Risk of Not Implementing:
Potential security breach.

Action:
Apply emergency security patch to production servers.

Implementation:
1. Take backup.
2. Apply patch.
3. Restart required services.
4. Validate application.
5. Monitor system.

Rollback:
Restore previous package/version if required.

Approval:
Emergency Change Approval

Status:
Completed.
```

---

# 58. Escalation Ticket Template

```text
Ticket:
INC-1008

Issue:
Application API returning HTTP 500 errors.

Current Team:
Application Support

Reason for Escalation:
Issue appears to be related to database connectivity.

Escalated To:
Database Team

Investigation Completed:
- Application logs checked.
- Network connectivity checked.
- Database connection errors confirmed.

Required Action:
Please investigate database connection issues.

Priority:
P2

SLA:
2 hours
```

---

# 59. Shift Handover Template

Shift handover is very important in Application Support.

Imagine your shift ends at 10 PM.

Another engineer starts at 10 PM.

You need to tell them what is still happening.

```text
# Shift Handover

Date:
10 October 2026

Shift:
2 PM - 10 PM

## Open Incidents

### INC-1001
Issue:
Payment API errors.

Status:
Application team investigating.

Current Finding:
Recent deployment may have caused the issue.

Next Action:
Monitor rollback.

Priority:
P1

---

### INC-1005
Issue:
Slow application response.

Status:
Under investigation.

Finding:
High database CPU.

Next Action:
Database team checking long-running queries.

Priority:
P2

---

## Important Notes

No other major incidents are currently active.

Please continue monitoring the payment API.
```

---

# 60. Knowledge Base Article Template

```text
# Application Login Failure

## Problem

Users cannot login to the application.

## Symptoms

- Login page loads
- User enters credentials
- Login fails
- HTTP 500 error appears

## Checks

1. Check application status.
2. Check authentication service.
3. Check application logs.
4. Check database connectivity.

## Common Cause

Authentication service may be unavailable.

## Solution

Restart authentication service according to the approved procedure.

## Validation

Ask user to login again.

## Escalation

If the issue continues, escalate to the Application Team.
```

---

# 61. Ticket Statuses

Most ticketing systems have statuses similar to these:

```text
New
 ↓
Assigned
 ↓
In Progress
 ↓
Pending
 ↓
Resolved
 ↓
Closed
```

### New

Ticket has been created.

### Assigned

Ticket has been assigned to someone.

### In Progress

Engineer is working on it.

### Pending

Waiting for something.

For example:

```text
Waiting for user response
Waiting for approval
Waiting for another team
```

### Resolved

Issue has been fixed.

### Closed

Ticket is officially completed.

---

# 62. Complete Ticket Lifecycle

Let's connect everything.

```text
User Reports Issue
        ↓
Ticket Created
        ↓
Classification
        ↓
Categorization
        ↓
Impact + Urgency
        ↓
Priority
        ↓
Assignment
        ↓
Investigation
        ↓
Escalation
        ↓
Resolution
        ↓
User Confirmation
        ↓
Ticket Resolved
        ↓
Ticket Closed
```

For major incidents:

```text
Major Incident
      ↓
Incident Manager
      ↓
Technical Teams
      ↓
Communication
      ↓
Service Restoration
      ↓
RCA
      ↓
Post-Incident Review
      ↓
Preventive Actions
```

---

# 63. Good Ticket vs Bad Ticket

## Bad Ticket

```text
Application not working.

Please check.
```

This is not useful.

The next engineer will ask:

```text
Which application?
Who is affected?
When did it start?
What error?
What environment?
What was checked?
```

---

## Good Ticket

```text
Production customer application is returning HTTP 500 errors.

Approximately 200 users are affected.

Issue started at 10:20 AM.

Application logs show database connection errors.

Application health check is successful, but database connections are failing.

Database team has been contacted.

Priority: P2.

Current Status: Investigation in progress.
```

This is much better.

---

# 64. Practical Ticket Investigation Format

Whenever you receive an incident, try to think in this order:

```text
1. What is broken?

2. Who is affected?

3. When did it start?

4. Is it Production?

5. What error are we getting?

6. What changed recently?

7. What do the logs say?

8. Is the server healthy?

9. Is the application healthy?

10. Is the database healthy?

11. Is the network working?

12. Do we have monitoring alerts?

13. Can we restore the service?

14. Do we need escalation?
```

This thinking process is extremely useful for Application Support engineers.

---

# 65. Real-World Application Support Example

Imagine an e-commerce company.

The customer says:

```text
I cannot place my order.
```

Support engineer starts investigation.

### Step 1 — Create Ticket

```text
INC-2001
```

### Step 2 — Understand Impact

```text
Only one customer?
```

or:

```text
Many customers?
```

Suppose:

```text
Thousands of customers affected.
```

Impact is high.

### Step 3 — Check Application

```text
Application → Running
```

### Step 4 — Check API

```text
Order API → HTTP 500
```

### Step 5 — Check Logs

```text
Database connection timeout
```

### Step 6 — Check Database

```text
Database CPU → 98%
```

### Step 7 — Escalate

```text
Application Support
        ↓
Database Team
```

### Step 8 — Find Cause

Database team finds:

```text
Long-running query
        ↓
Database CPU increases
        ↓
Connections become slow
        ↓
Order API times out
        ↓
Customers cannot place orders
```

### Step 9 — Restore Service

The query is stopped/optimized according to the approved process.

### Step 10 — Validate

```text
Test Order → Successful
```

### Step 11 — Close Incident

Document everything.

### Step 12 — Create Problem

Because the issue may happen again, create a Problem ticket.

### Step 13 — Prevent Future Issue

Possible actions:

```text
Optimize query
Add database monitoring
Add CPU alert
Review query performance
Update documentation
```

This is how ITSM works in the real world.

---

# 66. Important ITSM Terms — Quick Revision

| Term | Simple Meaning |
|---|---|
| ITSM | Managing IT services properly |
| ITIL | IT service management best-practice framework |
| Service | Something provided to users |
| Incident | Something is broken |
| Problem | Root cause behind incidents |
| Service Request | User asks for something |
| Event | Something happened |
| Alert | Something needs attention |
| Severity | How serious the issue is |
| Impact | How many/how much is affected |
| Urgency | How quickly action is needed |
| Priority | How quickly we should work |
| Escalation | Move issue to another/higher team |
| SLA | Service-level agreement |
| OLA | Internal team agreement |
| MTTR | Average time to restore/repair |
| MTTD | Average time to detect |
| MTBF | Average time between failures |
| CAB | Group that reviews changes |
| RCA | Finding the root cause |
| Standard Change | Routine approved change |
| Normal Change | Planned change requiring approval |
| Emergency Change | Urgent change |

---

# 67. Ticket Writing Rules

When writing tickets, follow these simple rules.

### Rule 1 — Write Clearly

Bad:

```text
Server problem.
```

Good:

```text
Production server is experiencing high CPU usage.
```

---

### Rule 2 — Mention the Impact

Bad:

```text
Payment API error.
```

Good:

```text
Payment API is failing and approximately 500 customers are unable to complete payments.
```

---

### Rule 3 — Mention What You Checked

```text
Checked:

- Application logs
- Server CPU
- Database connectivity
- API health
```

---

### Rule 4 — Mention What You Found

```text
Finding:

Database connection pool is exhausted.
```

---

### Rule 5 — Mention the Next Action

```text
Next Action:

Database team is investigating connection pool configuration.
```

---

### Rule 6 — Keep Updating the Ticket

Don't wait until the issue is completely fixed.

Update the ticket when:

```text
Investigation starts
Finding discovered
Team escalated
Workaround applied
Service restored
Root cause identified
```

---

# 68. Student Practice

Now practice writing your own tickets.

---

## Exercise 1 — Application Down

Scenario:

```text
Company website is not opening.

All customers are affected.

Monitoring shows the application server is down.
```

Create an Incident ticket containing:

- Title
- Description
- Impact
- Priority
- Environment
- Investigation
- Current status
- Next action

---

# Exercise 2 — Access Request

Scenario:

```text
A new employee needs access to the HR application.

Manager has approved the request.
```

Create a Service Request ticket.

---

# Exercise 3 — Database Problem

Scenario:

```text
Application is slow.

Database CPU is 95%.

Several slow queries are running.
```

Create:

1. Incident ticket
2. Escalation ticket
3. Problem ticket

---

# Exercise 4 — Production Deployment

Scenario:

```text
Application team wants to deploy version 2.5 to production.
```

Create a Change Request containing:

- Reason
- Risk
- Implementation plan
- Validation
- Rollback plan
- Approval

---

# Exercise 5 — Major Incident

Scenario:

```text
Payment system is completely down.

Thousands of customers are affected.
```

Create a Major Incident ticket.

Include:

- Severity
- Impact
- Teams involved
- Current status
- Investigation
- Communication update
- Next update

---

# 69. Interview Questions

After completing this module, you should be able to answer:

### Beginner

1. What is ITSM?
2. What is ITIL?
3. What is an IT service?
4. What is an incident?
5. What is a problem?
6. What is a service request?
7. What is an event?
8. What is an alert?

### Intermediate

9. What is the difference between Incident and Problem?
10. What is severity?
11. What is priority?
12. What is impact?
13. What is urgency?
14. What is escalation?
15. What is functional escalation?
16. What is hierarchical escalation?
17. What is SLA?
18. What is OLA?

### Advanced

19. What is MTTR?
20. What is MTTD?
21. What is MTBF?
22. What is a major incident?
23. What is RCA?
24. What is CAB?
25. What is a standard change?
26. What is a normal change?
27. What is an emergency change?
28. Explain the complete incident lifecycle.

---

# 70. Final Revision

If you remember only a few things from this module, remember these:

```text
ITSM
↓
Manage IT Services Properly
```

```text
INCIDENT
↓
Something is broken
```

```text
PROBLEM
↓
Find why it is breaking
```

```text
SERVICE REQUEST
↓
User needs something
```

```text
IMPACT
↓
How many are affected?
```

```text
URGENCY
↓
How quickly is action needed?
```

```text
PRIORITY
↓
How quickly should we work?
```

```text
SLA
↓
Expected service level
```

```text
ESCALATION
↓
Send to the right/higher team
```

```text
RCA
↓
Find the actual root cause
```

```text
CHANGE MANAGEMENT
↓
Make production changes safely
```

---

# 71. Most Important Skill for an Application Support Engineer

ITSM is not just about remembering definitions.

The real skill is:

> **Knowing what to do when something goes wrong.**

When a production issue happens, think:

```text
What happened?
      ↓
Who is affected?
      ↓
How serious is it?
      ↓
What is the priority?
      ↓
What should I check?
      ↓
What do the logs say?
      ↓
Can I fix it?
      ↓
Do I need to escalate?
      ↓
Is the service restored?
      ↓
What was the root cause?
      ↓
How can we prevent it?
```

That mindset is what turns a beginner into a good Application Support Engineer.

---

# Module 2 Completed

You have now learned:

- ITSM
- ITIL
- IT Services
- Incidents
- Problems
- Service Requests
- Events
- Alerts
- Incident Lifecycle
- Problem Management
- Change Management
- Request Fulfillment
- Classification
- Categorization
- Prioritization
- Severity
- Priority
- Impact
- Urgency
- Escalation
- SLA
- OLA
- Response Time
- Resolution Time
- MTTR
- MTTD
- MTBF
- Major Incidents
- Incident Communication
- Incident Closure
- Post-Incident Review
- Knowledge Management
- CAB
- Standard Changes
- Normal Changes
- Emergency Changes
- Practical Incident Tickets
- Service Request Tickets
- Problem Tickets
- RCA
- Change Tickets
- Escalation Tickets
- Shift Handover
- Knowledge Articles

## Next Step

Now that we understand **how Application Support works from a process perspective**, we can move to the most important technical foundation:

# Linux

In the next module we will start Linux from the beginning:

```text
What is Linux?
      ↓
Why is Linux important?
      ↓
Linux History
      ↓
Linux Architecture
      ↓
Linux Distributions
      ↓
Terminal
      ↓
Shell
      ↓
Basic Commands
      ↓
Files & Directories
      ↓
Users & Groups
      ↓
Permissions
      ↓
Processes
      ↓
Services
      ↓
Networking
      ↓
Logs
      ↓
Linux Troubleshooting
```

This is where the **hands-on Application Support journey** really begins.
