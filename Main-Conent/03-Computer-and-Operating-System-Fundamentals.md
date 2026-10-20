# Module 3 — Computer & Operating System Fundamentals

> **Course:** Application Support  
> **Module:** 3  
> **Level:** Beginner → Industry  
> **File:** `03-Computer-and-Operating-System-Fundamentals.md`

---

# 1. Introduction

Before working with Linux, servers, applications, databases, AWS, or production systems, we need to understand one basic thing:

> **How does a computer actually work?**

As an Application Support Engineer, you don't always need to build a computer or write an operating system.

But when something goes wrong in production, you need to understand what is happening underneath the application.

For example:

```text
User
 ↓
Application
 ↓
Operating System
 ↓
CPU / RAM / Disk / Network
 ↓
Hardware
```

Suppose a user says:

> "The application is very slow."

You cannot simply restart the application every time.

You need to ask:

```text
Is CPU high?
Is memory full?
Is disk full?
Is disk slow?
Is network slow?
Is another process consuming resources?
Are application logs showing errors?
```

That is why these fundamentals are important.

---

# 2. Computer Architecture

Computer architecture means:

> **How the different parts of a computer are organized and work together.**

A simple computer looks like this:

```text
                 COMPUTER
                    |
       +------------+------------+
       |            |            |
      CPU          RAM         Storage
       |            |            |
   Processing    Temporary    Permanent
                 Memory         Data
       |
       +-------------+
       |
    Operating
     System
       |
    Applications
```

The main components are:

- CPU
- RAM
- Storage
- Network
- Motherboard
- Input/Output devices

For Application Support, CPU, RAM, storage, network, and operating system are especially important.

---

## Real-Life Example

Think about a restaurant.

```text
CPU     = Chef
RAM     = Kitchen working table
Storage = Warehouse
Network = Delivery system
OS      = Restaurant manager
```

The chef prepares food.

The working table holds things currently being used.

The warehouse stores things for later.

The delivery system moves things between locations.

The manager coordinates everything.

Similarly, a computer has different components working together.

---

## Industrial Example

Imagine an application server:

```text
Server
 |
 +-- CPU → Processes application requests
 |
 +-- RAM → Holds running application data
 |
 +-- Disk → Stores logs, files, configuration
 |
 +-- Network → Communicates with users/databases/APIs
 |
 +-- OS → Manages all resources
 |
 +-- Application → Provides business functionality
```

If CPU becomes overloaded, the application may become slow.

If RAM becomes full, applications may crash or become slow.

If disk becomes full, applications may stop writing logs or data.

If network fails, users may not be able to access the application.

---

# 3. CPU

CPU stands for:

> **Central Processing Unit**

CPU is often called the **brain of the computer**.

It executes instructions.

For example:

```text
Application
    ↓
Instruction
    ↓
CPU
    ↓
Processing
    ↓
Result
```

---

## Real-Life Example

Think about a chef.

You give the chef:

> Make a pizza.

The chef processes the instructions and performs the work.

Similarly:

```text
Program
   ↓
Instructions
   ↓
CPU
   ↓
Execution
```

---

## CPU Cores

A CPU can have multiple cores.

For example:

```text
CPU
 |
 +-- Core 1
 +-- Core 2
 +-- Core 3
 +-- Core 4
```

More cores allow the system to handle more work simultaneously.

---

## Industrial Example

Suppose a production server has:

```text
8 CPU cores
```

And an application suddenly receives thousands of requests.

The CPU may become heavily utilized:

```text
CPU Usage = 95%
```

Users may experience:

```text
Slow response
Timeouts
Delayed requests
```

An Application Support Engineer might check:

```bash
top
```

or:

```bash
htop
```

to understand which processes are consuming CPU.

---

# 4. RAM

RAM stands for:

> **Random Access Memory**

RAM is temporary memory used by running programs.

For example:

```text
Application running
       ↓
Data loaded into RAM
       ↓
CPU uses the data
```

RAM is much faster than disk storage.

---

## Real-Life Example

Think about your study table.

```text
Study Table = RAM
Cupboard    = Storage
```

You keep the books you are currently studying on the table.

Books you don't currently need remain in the cupboard.

Similarly:

```text
Currently running data → RAM
Long-term data         → Disk
```

---

## What Happens When RAM Is Low?

Suppose:

```text
Server RAM = 16 GB
Used RAM   = 15.8 GB
```

The server has very little free memory.

Applications may become slow.

The operating system may start using disk space as virtual memory/swap.

This is slower than RAM.

---

## Industrial Example

Imagine a Java application running on a production server.

```text
Server RAM = 32 GB

Application = 20 GB
Database tools = 5 GB
OS = 4 GB
Other processes = 3 GB
```

Total:

```text
32 GB
```

The system is almost completely using memory.

If the application tries to allocate more memory, problems may occur.

Possible symptoms:

```text
Application becomes slow
OutOfMemory errors
Process killed
Server performance decreases
```

---

# 5. Storage

Storage is where data is kept permanently.

Examples:

- SSD
- HDD
- NVMe
- Cloud storage
- SAN
- NAS

Unlike RAM, storage keeps data after the computer is restarted.

---

## Simple Difference

```text
RAM
↓
Temporary

Storage
↓
Permanent
```

Example:

```text
Power OFF

RAM → Data disappears

Disk → Data remains
```

---

## Real-Life Example

Think about your desk and cupboard.

```text
Desk     → RAM
Cupboard → Storage
```

Things you are currently using stay on your desk.

Things you want to keep for a long time go into the cupboard.

---

# 6. Disk

A disk is a storage device used to store data.

Examples:

```text
HDD
SSD
NVMe
```

Linux systems use disks to store:

- Operating system files
- Application files
- Logs
- Configuration
- Databases
- User files

---

## Industrial Example

Suppose a production server has:

```text
Disk = 100 GB
Used = 98 GB
```

Only 2 GB is left.

This can become a serious problem.

For example:

```text
Application
    ↓
Writes logs
    ↓
Disk Full
    ↓
Cannot write logs
    ↓
Application may fail
```

Some applications may also fail because they need temporary disk space.

As an Application Support Engineer, you should always know how to check disk usage.

Linux command:

```bash
df -h
```

---

# 7. Operating System

An Operating System, or OS, is software that manages computer hardware and provides an environment for applications.

Examples:

- Linux
- Windows
- macOS
- Android
- iOS

For Application Support and DevOps, Linux is especially important.

---

## Simple Explanation

Think of the operating system as a **manager**.

The manager controls:

```text
CPU
RAM
Disk
Network
Processes
Users
Files
Devices
```

Applications don't directly control all hardware themselves.

The operating system manages access to these resources.

---

## Example

Suppose you run:

```bash
python app.py
```

The operating system:

```text
Starts the process
↓
Provides memory
↓
Provides CPU time
↓
Provides file access
↓
Provides network access
↓
Manages the process
```

---

# 8. Kernel

The kernel is the **core part of the operating system**.

It directly interacts with hardware and manages system resources.

Simple architecture:

```text
+----------------------+
|     Applications     |
+----------------------+
|      User Space      |
+----------------------+
|        Kernel        |
+----------------------+
|       Hardware       |
+----------------------+
```

---

## What Does the Kernel Do?

The kernel manages:

- CPU
- Memory
- Processes
- Devices
- Filesystems
- Networking
- System calls

---

## Real-Life Example

Think about a company.

```text
Employees
    ↓
Managers
    ↓
Company management
    ↓
Resources
```

The kernel is like the central management layer that controls access to system resources.

---

## Industrial Example

When an application wants to open a file:

```text
Application
     ↓
System Call
     ↓
Kernel
     ↓
Filesystem
     ↓
Disk
```

The application doesn't simply access the disk directly.

The operating system kernel manages the operation.

---

# 9. User Space

User Space is where normal applications run.

Examples:

```text
Browser
Python application
Java application
Shell
Monitoring tools
Web server
```

A simplified view:

```text
User Space
------------------
Applications
Shell
Utilities
Services
------------------
Kernel
------------------
Hardware
```

---

## Why Is This Important?

If an application crashes, the entire operating system does not necessarily crash.

For example:

```text
Application crashes
        ↓
Kernel continues running
        ↓
Other applications continue running
```

This separation helps make operating systems stable.

---

# 10. Process

A process is:

> **A running instance of a program.**

Suppose you have a Python file:

```text
app.py
```

The file itself is a program.

When you execute:

```bash
python app.py
```

a running process is created.

```text
Program
  ↓
Execute
  ↓
Process
```

---

## Real-Life Example

A recipe is not the cooking process.

```text
Recipe     = Program
Cooking    = Process
```

The recipe is instructions.

When someone starts following the recipe, actual work is happening.

Similarly, when a program starts running, a process is created.

---

## Linux Example

You can check processes using:

```bash
ps
```

or:

```bash
ps aux
```

or:

```bash
top
```

---

# 11. Thread

A thread is a smaller unit of execution inside a process.

A process can contain multiple threads.

```text
Process
 |
 +-- Thread 1
 +-- Thread 2
 +-- Thread 3
 +-- Thread 4
```

---

## Real-Life Example

Think about a restaurant.

```text
Restaurant = Process

Chef       = Thread
Waiter      = Thread
Cashier     = Thread
Delivery    = Thread
```

Everyone is working as part of the same restaurant.

---

## Industrial Example

A web application may receive many requests.

Instead of handling everything with one execution path, the application may use multiple threads.

```text
Application Process
       |
       +-- Thread → Request 1
       +-- Thread → Request 2
       +-- Thread → Request 3
       +-- Thread → Request 4
```

This can allow applications to handle multiple tasks efficiently.

---

# 12. Process vs Thread

| Process | Thread |
|---|---|
| Running program | Execution unit inside process |
| Has its own resources/address space | Shares many resources with process |
| Heavier | Lighter |
| Can contain multiple threads | Belongs to a process |

Easy way:

```text
Process
↓
Container of running application

Thread
↓
Worker inside that application
```

---

# 13. Service

A service is a program that performs a specific function, usually in the background.

Examples:

```text
Web server
Database server
SSH service
Monitoring service
Application service
```

For example:

```text
Nginx
MySQL
PostgreSQL
SSH
```

can run as services.

---

## Industrial Example

Suppose your application depends on:

```text
Application
    ↓
Database Service
```

If the database service stops:

```text
Application
    ↓
Database connection fails
    ↓
Application errors
```

The Application Support Engineer needs to understand service status.

On Linux systems using systemd:

```bash
systemctl status service-name
```

For example:

```bash
systemctl status nginx
```

---

# 14. Daemon

A daemon is a background process that usually runs continuously and performs a particular task.

Linux commonly uses names ending with:

```text
d
```

Examples:

```text
sshd
systemd
cron
```

The `d` often indicates a daemon.

---

## Real-Life Example

Think about security guards in a building.

You don't see them doing work every second.

But they continuously remain available and perform their job when needed.

Similarly, a daemon runs in the background and waits for tasks/events.

---

# 15. Service vs Daemon

These terms are related but not exactly identical.

Simple beginner understanding:

```text
Daemon
↓
Background process

Service
↓
A managed function/application provided by the system
```

For example:

```text
sshd
```

is a daemon that provides SSH functionality.

We can manage the SSH service using:

```bash
systemctl status ssh
```

The exact service name can vary by Linux distribution.

---

# 16. File System

A filesystem is the method used by the operating system to organize and store files.

Think about your computer.

You have:

```text
Documents
Pictures
Videos
Downloads
Projects
```

The operating system needs a structure to organize these files.

Linux has its own filesystem hierarchy.

---

# 17. Linux File System Structure

A simplified Linux filesystem:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── opt
├── proc
├── root
├── tmp
├── usr
└── var
```

The `/` directory is called the **root directory**.

---

# 18. Important Linux Directories

## `/etc`

Contains configuration files.

Example:

```text
/etc/hosts
/etc/ssh/
```

Think:

```text
/etc → Configuration
```

---

## `/var`

Contains variable data.

Examples:

```text
/var/log
/var/cache
```

Logs are commonly found under:

```text
/var/log
```

---

## `/home`

Contains normal users' home directories.

Example:

```text
/home/sahil
/home/student
```

---

## `/tmp`

Temporary files.

Applications often use temporary storage here.

---

## `/var/log`

Very important for Application Support.

This directory commonly contains system/application logs.

Example:

```text
/var/log/
```

---

# 19. Boot Process

Booting means:

> **Starting the computer and loading the operating system.**

A simplified Linux boot process:

```text
Power ON
   ↓
BIOS / UEFI
   ↓
Bootloader
   ↓
Linux Kernel
   ↓
System Initialization
   ↓
Services Start
   ↓
Login
   ↓
System Ready
```

---

# 20. Boot Process — Simple Explanation

Let's understand each stage.

### Step 1 — Power On

Computer receives power.

### Step 2 — BIOS/UEFI

Firmware checks hardware and prepares the system to boot.

### Step 3 — Bootloader

The bootloader loads the operating system kernel.

A common Linux bootloader is:

```text
GRUB
```

### Step 4 — Kernel

The Linux kernel starts.

### Step 5 — System Initialization

The operating system initializes services and resources.

Modern Linux systems commonly use:

```text
systemd
```

### Step 6 — Services Start

Services such as networking, SSH, logging, etc. start.

### Step 7 — Login

The system becomes available to users.

---

# 21. Industrial Example — Server Boot

Imagine a production server restarts.

```text
Server Restart
     ↓
UEFI
     ↓
GRUB
     ↓
Linux Kernel
     ↓
systemd
     ↓
Network starts
     ↓
Database starts
     ↓
Application starts
     ↓
Monitoring starts
     ↓
Server Ready
```

If a required service fails during boot:

```text
Application
    ↓
Database unavailable
```

the application may not start correctly.

This is why Application Support engineers need to understand the boot process.

---

# 22. Environment Variables

Environment variables are values stored by the operating system/shell environment that applications can read.

Example:

```text
JAVA_HOME=/usr/lib/jvm/java
```

Another common example:

```text
PATH=/usr/local/bin:/usr/bin:/bin
```

---

## Real-Life Example

Think about a company employee's ID card.

The ID card contains information about:

```text
Name
Department
Access Level
```

The employee doesn't need to manually provide this information every time.

Similarly, environment variables provide information to applications.

---

# 23. Common Environment Variables

### PATH

Tells the shell where to search for executable programs.

Example:

```bash
echo $PATH
```

### HOME

Shows the user's home directory.

```bash
echo $HOME
```

### USER

Shows the current username.

```bash
echo $USER
```

### SHELL

Shows the current shell.

```bash
echo $SHELL
```

---

# 24. Application Environment Variables

Modern applications commonly use environment variables for configuration.

Example:

```text
DATABASE_HOST=db.production.local
DATABASE_PORT=5432
APP_ENV=production
```

Instead of hardcoding these values inside the application.

---

## Industrial Example

Imagine the same application runs in:

```text
Development
Testing
Staging
Production
```

The application code may be the same.

But database addresses can be different.

```text
Development
DATABASE_HOST=dev-db

Production
DATABASE_HOST=prod-db
```

Environment variables make this easier to manage.

> **Important:** Secrets such as passwords, API keys, and tokens should be handled securely and not casually committed to source code or Git repositories.

---

# 25. System Resources

A computer has limited resources.

The major resources are:

```text
CPU
RAM
Disk
Network
```

Applications compete for these resources.

```text
             Server
                |
     +----------+----------+
     |          |          |
    CPU        RAM        Disk
     |
   Network
```

---

# 26. CPU Utilization

CPU utilization tells us how much CPU is currently being used.

Example:

```text
CPU = 20%
```

means relatively low utilization.

```text
CPU = 95%
```

means the CPU is heavily utilized.

---

## Industrial Example

Suppose your production application becomes slow.

You check:

```text
CPU = 98%
```

Then you investigate:

```text
Which process is consuming CPU?
```

Linux:

```bash
top
```

You might find:

```text
java
```

using most of the CPU.

Now you have a direction for your investigation.

---

# 27. High CPU — Possible Reasons

High CPU can happen because of:

- Heavy traffic
- Infinite loop
- CPU-intensive application
- Background process
- Malware
- Bad code
- Large data processing
- Too many requests

Don't assume:

```text
High CPU = Application problem
```

Always investigate.

---

# 28. Memory Utilization

Memory utilization tells us how much RAM is being used.

Linux command:

```bash
free -h
```

Example:

```text
              total    used    free
Mem:           16G     14G     2G
```

This means the server is using a large portion of its memory.

---

# 29. High Memory — Possible Reasons

Possible reasons include:

- Application memory leak
- Too many processes
- Large workload
- Database memory usage
- Caching
- Insufficient RAM
- Misconfigured application

Example:

```text
Java Application
       ↓
Memory continuously increases
       ↓
RAM becomes full
       ↓
Application crashes
```

---

# 30. Disk Utilization

Disk utilization can mean different things depending on context.

For beginners, first understand:

> **How much disk space is being used?**

Linux command:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used  Avail  Use%
/dev/sda1       100G   95G     5G   95%
```

The disk is 95% full.

---

## Industrial Example

Suppose an application writes logs continuously.

```text
Application
    ↓
Logs
    ↓
Disk
    ↓
Disk becomes full
```

Eventually:

```text
Disk = 100%
```

The application may fail to write:

- Logs
- Temporary files
- Reports
- Uploads
- Database files

This can become a production incident.

---

# 31. Network Utilization

Network utilization tells us how much network capacity is being used.

Applications constantly communicate through networks.

Example:

```text
User
 ↓
Internet
 ↓
Load Balancer
 ↓
Application Server
 ↓
Database
```

If network connectivity is poor, applications may become slow or unavailable.

---

## Industrial Example

Imagine an application server communicates with a database server.

```text
Application
     ↓
Network
     ↓
Database
```

If network connectivity is broken:

```text
Application
     ↓
❌ Cannot reach Database
```

The application may show:

```text
Connection timeout
Database unavailable
Request failed
```

---

# 32. System Performance

System performance means:

> **How well the computer is performing under its workload.**

We usually look at:

```text
CPU
RAM
Disk
Network
Processes
Application response time
```

---

## Example

Suppose users complain:

> "Application is slow."

Don't immediately restart the server.

Check:

```text
CPU      → 30%
RAM      → 40%
Disk     → 50%
Network  → Normal
```

Everything looks normal.

Then check:

```text
Application
Database
API
Logs
```

You may discover a slow database query.

---

# 33. Performance Investigation Mindset

When someone says:

> "The server is slow."

Ask:

```text
What exactly is slow?
```

Then check:

```text
CPU?
RAM?
Disk?
Network?
Application?
Database?
External API?
```

This is much better than blindly restarting services.

---

# 34. Process Management

Process management means:

> **Monitoring and controlling running processes.**

Useful Linux commands include:

```bash
ps
```

```bash
top
```

```bash
htop
```

```bash
pgrep
```

```bash
kill
```

```bash
pkill
```

---

# 35. Checking Processes

Basic command:

```bash
ps
```

More detailed:

```bash
ps aux
```

Example:

```text
USER   PID   %CPU   %MEM   COMMAND
root   1001   2.0    1.5   nginx
app    2001  75.0   10.0   java
```

Important columns:

```text
PID
↓
Process ID

%CPU
↓
CPU usage

%MEM
↓
Memory usage

COMMAND
↓
Process name
```

---

# 36. PID

PID means:

> **Process ID**

Every running process normally has a unique process ID.

Example:

```text
java → PID 2456
```

You can use the PID to manage the process.

For example:

```bash
kill 2456
```

---

# 37. Killing a Process

If a process is stuck, you may need to terminate it.

Normal:

```bash
kill PID
```

Example:

```bash
kill 2456
```

If necessary, a forceful signal can be used:

```bash
kill -9 2456
```

But:

> **Do not use `kill -9` blindly in production.**

First understand what the process is doing.

Killing a database process or critical application process without understanding the impact can make the incident worse.

---

# 38. Service Management

Modern Linux systems commonly use:

```text
systemd
```

The main command is:

```bash
systemctl
```

---

## Check Service

```bash
systemctl status nginx
```

---

## Start Service

```bash
sudo systemctl start nginx
```

---

## Stop Service

```bash
sudo systemctl stop nginx
```

---

## Restart Service

```bash
sudo systemctl restart nginx
```

---

## Enable Service at Boot

```bash
sudo systemctl enable nginx
```

---

## Disable Service at Boot

```bash
sudo systemctl disable nginx
```

---

# 39. Industrial Example — Application Service

Suppose users report:

> "Website is not opening."

You check the server.

```bash
systemctl status nginx
```

You discover:

```text
nginx.service
Active: failed
```

Now you know the web server service has failed.

You should then investigate:

```text
Why did it fail?
```

Check logs.

```bash
journalctl -u nginx
```

After understanding the issue, you can take the appropriate recovery action.

---

# 40. System Logs

Logs are one of the most important things in Application Support.

A log is a record of what happened inside a system.

Examples:

```text
Application started
User logged in
Database connection failed
API request failed
Service stopped
Server restarted
```

Think of logs as:

> **The system's diary.**

---

# 41. Real-Life Example of Logs

Imagine a security guard maintaining a register:

```text
10:00 → Person entered
10:15 → Person exited
10:30 → Door alarm triggered
10:35 → Security checked door
```

Later, if something happens, you can check the register.

Logs work similarly.

---

# 42. Linux System Logs

Many Linux systems use:

```text
systemd-journald
```

You can view logs using:

```bash
journalctl
```

For a particular service:

```bash
journalctl -u nginx
```

Follow logs:

```bash
journalctl -u nginx -f
```

---

# 43. Log Files

Many applications also write logs into files.

Common location:

```text
/var/log
```

You can list files:

```bash
ls /var/log
```

Example:

```text
auth.log
syslog
kern.log
```

Exact files vary between Linux distributions.

---

# 44. Reading Logs

Useful commands:

```bash
cat file.log
```

```bash
less file.log
```

```bash
tail file.log
```

```bash
tail -f file.log
```

---

## Example

```bash
tail -f application.log
```

This allows you to watch new log entries as they appear.

This is extremely useful during troubleshooting.

---

# 45. Searching Logs

Suppose you want to find errors.

Use:

```bash
grep "ERROR" application.log
```

Or:

```bash
grep -i "error" application.log
```

You can search for specific messages:

```bash
grep -i "connection refused" application.log
```

---

# 46. Industrial Troubleshooting Example

User reports:

> "Payment is failing."

You start investigating.

### Step 1

Check application status:

```bash
systemctl status payment-app
```

Service is running.

### Step 2

Check application logs:

```bash
tail -f application.log
```

You see:

```text
ERROR Database connection timeout
```

### Step 3

Check database connectivity.

```text
Application
    ↓
Database
    ↓
Connection Timeout
```

### Step 4

Check network.

### Step 5

Check database health.

### Step 6

Escalate to the Database/Network team if necessary.

This is real Application Support troubleshooting.

---

# 47. System Monitoring

Monitoring means:

> **Continuously watching the health and performance of systems.**

Instead of waiting for users to report:

> "The application is down."

Monitoring tools can detect the issue automatically.

---

# 48. What Do We Monitor?

Common things include:

```text
CPU
RAM
Disk
Network
Processes
Services
Application health
Database health
API response
Error rate
Response time
```

---

# 49. Simple Monitoring Example

Imagine:

```text
CPU > 90%
```

Monitoring system detects it.

Then:

```text
Monitoring
    ↓
Threshold crossed
    ↓
Alert
    ↓
Application Support Engineer
    ↓
Investigation
```

This is much better than discovering the problem after thousands of users complain.

---

# 50. Industrial Monitoring Example

Suppose an e-commerce application has:

```text
CPU
RAM
Disk
Application
Database
API
```

Monitoring dashboard:

```text
+-----------------------------+
| Production Server           |
+-----------------------------+
| CPU       45%      OK       |
| Memory    62%      OK       |
| Disk      71%      OK       |
| Network   40%      OK       |
| API       120ms    OK       |
| Errors    0.2%     OK       |
+-----------------------------+
```

Everything is healthy.

Suddenly:

```text
Disk = 95%
```

Monitoring generates:

```text
⚠ Disk Usage High
```

Application Support can investigate before the disk becomes full.

---

# 51. Monitoring vs Logging

These are related but different.

### Monitoring

Tells you:

> **Something may be wrong.**

### Logging

Tells you:

> **What happened.**

Example:

```text
Monitoring:
CPU = 95%
```

Log:

```text
Application worker failed because CPU resources were exhausted.
```

Both are useful.

---

# 52. Complete Application Support View

Now let's connect everything.

```text
                    USER
                      |
                      ↓
                 APPLICATION
                      |
          +-----------+-----------+
          |           |           |
         CPU         RAM         Disk
          |           |           |
          +-----------+-----------+
                      |
                   NETWORK
                      |
                      ↓
                OPERATING SYSTEM
                      |
                  KERNEL
                      |
                   HARDWARE
```

And Application Support works around all of these:

```text
                    SUPPORT
                       |
       +---------------+---------------+
       |               |               |
    Process          Service          Logs
       |               |               |
       +---------------+---------------+
                       |
                  Monitoring
                       |
                 Troubleshooting
```

---

# 53. Real-World Production Scenario

Let's take one complete example.

Imagine an online shopping website.

A customer reports:

> "The website is very slow."

---

## Step 1 — Check Application

```text
Application → Running
```

So the application isn't completely down.

---

## Step 2 — Check CPU

```text
CPU = 95%
```

This looks suspicious.

---

## Step 3 — Find Process

Run:

```bash
top
```

You discover:

```text
java → 90% CPU
```

---

## Step 4 — Check Logs

```bash
tail -f application.log
```

You discover many:

```text
ERROR
```

messages.

---

## Step 5 — Investigate

Logs show:

```text
Database query timeout
```

---

## Step 6 — Check Database

Database team finds:

```text
Long-running SQL query
```

---

## Step 7 — Immediate Action

According to the organization's approved procedure, the team applies a safe workaround.

Application performance improves.

---

## Step 8 — Root Cause

Later investigation finds:

```text
Bad SQL query
     ↓
Database CPU increased
     ↓
Application requests became slow
     ↓
Application CPU increased
     ↓
Users experienced slow website
```

---

# 54. What Did We Use in This Investigation?

Almost the entire module:

```text
CPU
 ↓
Memory
 ↓
Process
 ↓
Service
 ↓
Logs
 ↓
Disk
 ↓
Network
 ↓
Monitoring
 ↓
Application
```

This is why these fundamentals matter.

---

# 55. Important Linux Commands — Quick Preview

You will learn these commands properly in the Linux modules.

For now, remember:

| Purpose | Command |
|---|---|
| Show current directory | `pwd` |
| List files | `ls` |
| Show processes | `ps` |
| Monitor processes | `top` |
| Memory information | `free -h` |
| Disk space | `df -h` |
| Directory size | `du -sh` |
| Service status | `systemctl status` |
| Start service | `systemctl start` |
| Stop service | `systemctl stop` |
| Restart service | `systemctl restart` |
| View logs | `journalctl` |
| Search logs | `grep` |
| Watch log | `tail -f` |

These commands will become very important during Linux troubleshooting.

---

# 56. Beginner Mental Model

Whenever you look at a server, think like this:

```text
                 SERVER
                    |
       +------------+------------+
       |            |            |
      CPU          RAM          DISK
       |            |            |
       +------------+------------+
                    |
                 NETWORK
                    |
                    ↓
               OPERATING
                 SYSTEM
                    |
                 KERNEL
                    |
       +------------+------------+
       |            |            |
   Processes     Services      Logs
       |            |            |
       +------------+------------+
                    |
               APPLICATION
                    |
                   USER
```

If something goes wrong, move through this model.

---

# 57. Quick Revision

## Computer Architecture

How computer components are organized.

## CPU

Processes instructions.

## RAM

Temporary working memory.

## Storage

Permanent data storage.

## Disk

Physical/virtual storage device used to store data.

## Operating System

Manages hardware and provides an environment for applications.

## Kernel

Core of the operating system.

## User Space

Where normal applications run.

## Process

A running instance of a program.

## Thread

A unit of execution inside a process.

## Service

A program/function managed by the operating system.

## Daemon

A background process that performs a task.

## File System

Organizes and stores files.

## Boot Process

Steps through which a computer starts the operating system.

## Environment Variables

Configuration values available to processes/applications.

## System Resources

CPU, memory, disk, network, etc.

## CPU Utilization

How much CPU is being used.

## Memory Utilization

How much RAM is being used.

## Disk Utilization

How much disk capacity/activity is being used.

## Network Utilization

How much network capacity is being used.

## System Performance

How well the system performs under its workload.

## Process Management

Monitoring and controlling processes.

## Service Management

Starting, stopping, restarting, and monitoring services.

## System Logs

Records of events happening in the system.

## System Monitoring

Continuously checking system health.

---

# 58. Interview Questions

## Beginner

1. What is a computer?
2. What is CPU?
3. What is RAM?
4. What is storage?
5. What is an operating system?
6. What is Linux?
7. What is a kernel?
8. What is a process?
9. What is a thread?
10. What is a service?

---

## Intermediate

11. What is the difference between RAM and storage?
12. What is the difference between a process and a thread?
13. What is a daemon?
14. What is a filesystem?
15. What happens during system boot?
16. What are environment variables?
17. What is CPU utilization?
18. What is memory utilization?
19. How do you check disk usage in Linux?
20. How do you check running processes?

---

## Application Support Questions

21. Application is slow. What will you check?

22. Server CPU is 95%. What will you do?

23. Server memory is almost full. What will you check?

24. Disk is 100% full. What can happen?

25. Application service is down. How will you investigate?

26. How do you check service status?

27. Where can you find Linux logs?

28. How do you search an error in a log?

29. How do you monitor a log in real time?

30. Why are logs important for Application Support?

---

# 59. Practical Lab

Now let's do a simple Linux investigation.

> **Note:** Run these commands on your Linux lab/VM/server, not blindly on a production server.

---

## Task 1 — Check System Information

```bash
uname -a
```

---

## Task 2 — Check CPU

```bash
lscpu
```

---

## Task 3 — Check Memory

```bash
free -h
```

---

## Task 4 — Check Disk

```bash
df -h
```

---

## Task 5 — Check Disk Usage

```bash
du -sh /var/log
```

---

## Task 6 — Check Running Processes

```bash
ps aux
```

---

## Task 7 — Monitor Processes

```bash
top
```

Press:

```text
q
```

to exit.

---

## Task 8 — Check Services

Example:

```bash
systemctl status ssh
```

The service name may differ depending on your Linux distribution.

---

## Task 9 — View Logs

```bash
journalctl
```

---

## Task 10 — Follow Service Logs

Example:

```bash
journalctl -u ssh -f
```

Use `Ctrl + C` to stop following the logs.

---

# 60. Practical Troubleshooting Exercise

Imagine your manager gives you this ticket:

```text
INC-1050

Title:
Production Application is Slow

Description:
Users are reporting slow response from the application.

Impact:
Approximately 500 users are affected.

Environment:
Production
```

Your job is to investigate.

Follow this order:

```text
1. Check CPU
        ↓
2. Check Memory
        ↓
3. Check Disk
        ↓
4. Check Network
        ↓
5. Check Processes
        ↓
6. Check Services
        ↓
7. Check Logs
        ↓
8. Check Application
        ↓
9. Find the problem
        ↓
10. Document findings
```

Write your investigation in the ticket.

Example:

```text
Investigation:

CPU usage is 92%.

The Java application process is consuming approximately
80% CPU.

Application logs show repeated database timeout errors.

Database connectivity is currently being investigated.

Current Status:
Application Support and Database teams are investigating.
```

This is exactly the kind of thinking you will use in real Application Support work.

---

# 61. Final Takeaway

You don't need to become a hardware engineer to work in Application Support.

But you should understand:

```text
How CPU works
How RAM works
How storage works
How Linux works
How processes work
How services work
How files are organized
How the system boots
How applications use resources
How logs work
How monitoring works
```

The most important thing is to develop a **troubleshooting mindset**.

When an application has a problem, don't immediately think:

> "Restart it."

Instead think:

```text
What is happening?
       ↓
What is affected?
       ↓
Is the application running?
       ↓
Is the service running?
       ↓
Is CPU normal?
       ↓
Is memory normal?
       ↓
Is disk healthy?
       ↓
Is network working?
       ↓
What do the logs say?
       ↓
Which process is responsible?
       ↓
What changed recently?
       ↓
What is the safest solution?
```

That mindset is much more valuable than simply memorizing Linux commands.

---

# Module 3 Completed

You have now learned the basic foundation behind a computer and operating system:

```text
Computer Architecture
        ↓
CPU
        ↓
RAM
        ↓
Storage / Disk
        ↓
Operating System
        ↓
Kernel
        ↓
User Space
        ↓
Processes
        ↓
Threads
        ↓
Services
        ↓
Daemons
        ↓
Filesystem
        ↓
Boot Process
        ↓
Environment Variables
        ↓
System Resources
        ↓
Performance
        ↓
Process Management
        ↓
Service Management
        ↓
Logs
        ↓
Monitoring
```

## Next Module

The next step is to start working practically with **Linux**.

We will move from:

> **"How a computer works"**

to:

> **"How to actually work on a Linux server."**

The next module should cover:

```text
Linux Introduction
       ↓
Linux History
       ↓
Why Linux is important
       ↓
Linux Distributions
       ↓
Linux Architecture
       ↓
Terminal
       ↓
Shell
       ↓
Basic Linux Commands
       ↓
Files & Directories
       ↓
Navigation
       ↓
File Operations
       ↓
Practical Linux Exercises
```

From that point onward, the course becomes increasingly hands-on and focused on the type of Linux troubleshooting an Application Support Engineer performs in real production environments.
