# Module 4 — Linux Fundamentals

> **Course:** Application Support  
> **Module:** 4 — Linux Fundamentals  
> **Level:** Beginner → Industry  
> **File:** `04-Linux-Fundamentals.md`

---

# 1. Introduction to Linux

If you want to work in:

- Application Support
- Production Support
- DevOps
- Cloud
- AWS
- System Administration
- SRE
- Cybersecurity
- Backend Engineering

then Linux is one of the most important technologies you should understand.

A large number of production servers and cloud workloads run on Linux.

For an Application Support Engineer, Linux is especially important because many applications are running on Linux servers.

For example:

```text
Customer
   ↓
Website / Application
   ↓
Load Balancer
   ↓
Linux Server
   ↓
Application
   ↓
Database
```

When the application has a problem, you may need to connect to the Linux server and investigate:

```text
CPU
Memory
Disk
Processes
Services
Logs
Network
Files
Permissions
Users
Configuration
```

So Linux is not just another technology to learn.

> **Linux is one of the main working environments for an Application Support Engineer.**

---

# 2. What is Linux?

Linux is an open-source operating system kernel.

In everyday conversation, people commonly use the word **Linux** to refer to complete operating systems built around the Linux kernel.

Examples include:

- Ubuntu
- Debian
- Fedora
- Rocky Linux
- AlmaLinux
- Red Hat Enterprise Linux
- Amazon Linux
- Arch Linux

A simple view is:

```text
Linux-based Operating System
          |
          +-- Linux Kernel
          |
          +-- System Utilities
          |
          +-- Libraries
          |
          +-- Shell
          |
          +-- Applications
```

---

# 3. Linux Kernel

The **Linux kernel** is the core of the operating system.

It manages communication between software and hardware.

```text
+---------------------------+
|       Applications        |
+---------------------------+
|       Shell / Tools       |
+---------------------------+
|        Linux Kernel       |
+---------------------------+
|         Hardware          |
+---------------------------+
```

The kernel manages things such as:

- CPU
- Memory
- Processes
- Devices
- Filesystems
- Networking
- Security
- System calls

---

# 4. Linux vs Linux Distribution

This is an important concept for beginners.

### Linux Kernel

The kernel is the core component.

### Linux Distribution

A distribution combines the Linux kernel with other software needed to provide a usable operating system.

For example:

```text
Linux Kernel
     +
System Utilities
     +
Libraries
     +
Package Manager
     +
Shell
     +
Applications
     ↓
Linux Distribution
```

Examples:

```text
Ubuntu
Debian
RHEL
Fedora
Amazon Linux
Rocky Linux
AlmaLinux
```

So when someone says:

> "I use Ubuntu Linux."

Ubuntu is the **distribution**, while Linux is the kernel at its core.

---

# 5. Linux History

Understanding a little history helps us understand why Linux became so important.

---

## Unix

Before Linux, Unix was already widely used in universities, research environments, and businesses.

Unix influenced many operating-system concepts that later became important in Linux.

---

## 1991 — Linux Begins

In 1991, **Linus Torvalds** started developing the Linux kernel.

The project grew through contributions from developers around the world.

Linux became a major example of open-source software development.

---

# 6. GNU and Linux

You may also hear the term:

> **GNU/Linux**

The Linux kernel itself is not the complete operating system environment.

GNU provided many important tools and utilities used with Linux systems.

A simplified picture:

```text
GNU Tools + Linux Kernel
          ↓
Linux-based Operating System
```

In everyday conversation, most people simply say:

> Linux

and that is perfectly normal.

---

# 7. Why Did Linux Become Popular?

Linux became popular for many reasons.

### Open Source

Its source code is available for people to inspect, modify, and contribute to.

### Stability

Linux is widely used for systems that need to run for long periods.

### Security

Linux provides strong user, permission, process, and access-control mechanisms.

### Flexibility

Linux can be used for:

- Servers
- Cloud
- Containers
- Embedded systems
- Development
- Networking
- Supercomputers

### Automation

Linux provides powerful command-line tools and scripting capabilities.

### Community

There is a huge ecosystem of documentation, communities, tools, and software.

---

# 8. Linux Benefits

Let's understand the important benefits in simple language.

---

## 8.1 Open Source

Linux is open source.

This means its source code is available under its licensing terms for people to inspect, modify, and distribute according to the applicable license.

For companies, this provides flexibility and a large ecosystem.

---

## 8.2 Stable

Linux is widely used for systems that need high availability.

For example:

```text
Production Server
       ↓
Running continuously
       ↓
Application
       ↓
Customers
```

---

## 8.3 Secure

Linux provides security mechanisms such as:

- Users
- Groups
- File permissions
- Ownership
- Process isolation
- Authentication
- Access controls
- Firewall tools

We will study these in later submodules.

---

## 8.4 Powerful Command Line

Linux provides a powerful command-line environment.

For example:

```bash
ls
cd
pwd
cp
mv
rm
grep
find
ps
top
df
du
systemctl
journalctl
```

These commands allow engineers to perform tasks quickly.

---

## 8.5 Automation

Linux works extremely well with automation.

For example:

```text
Manual Work
     ↓
Write Script
     ↓
Automate Task
     ↓
Run Automatically
```

This becomes extremely important in:

- DevOps
- Cloud
- Application Support
- System Administration

---

## 8.6 Large Server Ecosystem

Linux is heavily used in server environments.

A typical production architecture might look like:

```text
Internet
   ↓
Load Balancer
   ↓
Linux Web Servers
   ↓
Linux Application Servers
   ↓
Linux Database Servers
```

---

# 9. Why Linux is Important for Application Support

This is especially important for our course.

Imagine your company has a production application.

A user reports:

> "The application is not responding."

You may need to log into the Linux server.

Then you may perform:

```text
Check server
    ↓
Check CPU
    ↓
Check memory
    ↓
Check disk
    ↓
Check process
    ↓
Check service
    ↓
Check logs
    ↓
Check network
    ↓
Investigate application
```

Linux gives you the tools to perform this investigation.

---

# 10. Real-Life Example

Imagine you own a restaurant.

You don't just need the menu.

You also need to know:

- Where ingredients are stored
- Who works there
- Who has access
- Which machines are running
- What happened during the day
- Whether the kitchen is overloaded
- Whether equipment is working

A Linux server is similar.

You need to understand:

```text
Files
Users
Permissions
Processes
Services
Logs
Resources
Network
```

---

# 11. Industrial Example

Imagine an e-commerce company.

Its architecture could look like:

```text
                         Customers
                             |
                             ↓
                         Internet
                             |
                             ↓
                       Load Balancer
                             |
              +--------------+--------------+
              |              |              |
              ↓              ↓              ↓
          Linux Server   Linux Server   Linux Server
              |              |              |
              +--------------+--------------+
                             |
                             ↓
                       Database Server
```

An Application Support Engineer may receive a ticket:

> "Customers cannot place orders."

The engineer may need to investigate:

```text
Application service
       ↓
Processes
       ↓
CPU / RAM
       ↓
Application logs
       ↓
Network
       ↓
Database connectivity
```

Linux knowledge becomes essential.

---

# 12. Where is Linux Used?

Linux is used in many areas.

---

## Servers

Web servers, application servers, database servers, file servers, etc.

---

## Cloud

Linux is widely used in cloud environments.

For example:

```text
AWS
Azure
Google Cloud
```

Cloud platforms provide many Linux-based server options.

---

## DevOps

Linux is commonly used with:

- Docker
- Kubernetes
- Jenkins
- Git
- Ansible
- Terraform
- CI/CD tools

---

## Containers

Many container workloads use Linux technologies underneath.

For example:

```text
Docker
   ↓
Linux-based environment
   ↓
Application
```

---

## Networking

Linux is used in:

- Network appliances
- Firewalls
- Routers
- Proxies
- DNS systems

---

## Cybersecurity

Linux is widely used in security testing, monitoring, analysis, and security infrastructure.

---

## Databases

Many database workloads run on Linux servers.

Examples:

- PostgreSQL
- MySQL
- MariaDB
- MongoDB
- Oracle Database

---

# 13. Linux in Cloud Computing

Linux is extremely important in cloud environments.

For example, you can create a Linux virtual machine in AWS.

Conceptually:

```text
AWS Cloud
   |
   +-- Virtual Machine
           |
           +-- Linux
                 |
                 +-- Application
                 +-- Web Server
                 +-- Monitoring
                 +-- Logs
```

As an Application Support Engineer, you may connect to that machine using SSH.

---

# 14. Linux in DevOps

DevOps engineers frequently work with Linux.

A typical DevOps environment could look like:

```text
Developer
   ↓
Git
   ↓
CI/CD
   ↓
Build
   ↓
Test
   ↓
Deploy
   ↓
Linux Server
   ↓
Application
```

Linux provides the environment where many of these tools run.

---

# 15. Linux Architecture

A simplified Linux architecture:

```text
+--------------------------------+
|          Applications          |
+--------------------------------+
|        Shell / Utilities       |
+--------------------------------+
|         System Libraries       |
+--------------------------------+
|          Linux Kernel          |
+--------------------------------+
|            Hardware            |
+--------------------------------+
```

Let's understand it from bottom to top.

---

## Hardware

Physical resources:

```text
CPU
RAM
Disk
Network
Devices
```

---

## Kernel

Controls access to hardware and manages system resources.

---

## Libraries and Utilities

Provide functionality used by applications and system tools.

---

## Shell

Allows users and scripts to interact with the operating system.

---

## Applications

Examples:

```text
Web Server
Database
Java Application
Python Application
Monitoring Agent
```

---

# 16. Linux Distributions

A Linux distribution is a complete operating system built around the Linux kernel.

There are many distributions.

Some popular ones are:

```text
Ubuntu
Debian
Fedora
RHEL
Rocky Linux
AlmaLinux
Amazon Linux
Arch Linux
openSUSE
```

Different distributions have different:

- Package managers
- Release models
- Default configurations
- Support models
- Target users

---

# 17. Important Linux Distribution Families

Understanding families is useful.

### Debian Family

Examples:

```text
Debian
Ubuntu
Linux Mint
```

Common package manager:

```text
apt
```

Example:

```bash
sudo apt update
```

---

### Red Hat Family

Examples:

```text
RHEL
Fedora
Rocky Linux
AlmaLinux
```

Common package management tools include:

```text
dnf
rpm
```

Example:

```bash
sudo dnf update
```

---

### Arch Family

Example:

```text
Arch Linux
```

Package manager:

```text
pacman
```

---

# 18. Why Are There So Many Distributions?

Different users have different requirements.

For example:

```text
Beginner Desktop User
        ↓
Ubuntu

Enterprise Server
        ↓
RHEL

Cloud Workload
        ↓
Amazon Linux / Ubuntu / RHEL / others

Advanced Linux User
        ↓
Arch Linux
```

There is no single distribution that is best for every situation.

---

# 19. Linux File System

Linux uses a hierarchical filesystem.

At the top is:

```text
/
```

This is called the:

> **Root directory**

Everything is organized underneath it.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── opt
├── proc
├── root
├── sbin
├── tmp
├── usr
└── var
```

We will study these directories in detail in a dedicated submodule.

---

# 20. Important Linux Directories

For now, remember their basic purpose.

| Directory | Simple Meaning |
|---|---|
| `/` | Root of filesystem |
| `/etc` | Configuration |
| `/var` | Variable data |
| `/var/log` | Logs |
| `/home` | User home directories |
| `/tmp` | Temporary files |
| `/opt` | Optional/third-party software |
| `/usr` | User-space programs and data |
| `/bin` | Essential user commands on systems using this layout |
| `/sbin` | System administration commands on systems using this layout |
| `/proc` | Kernel/process information |
| `/dev` | Device files |
| `/root` | Root user's home directory |
| `/boot` | Boot-related files |

> **Note:** Modern Linux distributions may merge directories such as `/bin` and `/sbin` into `/usr/bin` and `/usr/sbin`. You will learn this properly in the filesystem submodule.

---

# 21. Linux Command Line

One of the most important parts of Linux is the command line.

Instead of clicking buttons, we can type commands.

For example:

```bash
ls
```

shows files.

```bash
pwd
```

shows the current directory.

```bash
cd /var/log
```

moves to another directory.

This is called the:

> **Command-Line Interface (CLI)**

---

# 22. CLI vs GUI

### GUI

Graphical User Interface.

You interact using:

- Mouse
- Windows
- Buttons
- Menus
- Icons

Example:

```text
File Manager
Browser
Desktop
```

---

### CLI

Command-Line Interface.

You type commands.

Example:

```bash
ls
cd
mkdir
grep
ps
```

---

## Why is CLI Important in Linux?

Servers often don't need a graphical desktop.

For example:

```text
Production Server
        ↓
No GUI
        ↓
SSH
        ↓
Terminal
        ↓
Commands
```

This makes CLI knowledge extremely important for Application Support.

---

# 23. Shell

A shell is a program that allows you to interact with the operating system through commands.

Simple flow:

```text
You
 ↓
Shell
 ↓
Operating System
 ↓
Kernel
 ↓
Hardware
```

You type:

```bash
ls
```

The shell interprets the command and requests the operating system to perform the operation.

---

# 24. Types of Shells

There are several shells available in Linux.

Common examples:

```text
Bash
Zsh
sh
Fish
Ksh
```

The most common shell you will encounter in Linux administration and Application Support is:

> **Bash**

---

# 25. Bash

Bash stands for:

> **Bourne Again SHell**

It is one of the most widely used shells on Linux systems.

You can check your current shell:

```bash
echo $SHELL
```

Example:

```text
/bin/bash
```

---

# 26. Shell Scripting

Once you understand Linux commands, you can combine them into scripts.

Example:

```bash
#!/bin/bash

echo "Checking server"
df -h
free -h
uptime
```

Instead of manually running three commands:

```bash
df -h
free -h
uptime
```

you can run one script.

This is the beginning of Linux automation.

---

# 27. Linux Users

Linux is a multi-user operating system.

This means multiple users can exist on the same system.

Example:

```text
root
sahil
developer
support
student
```

Each user can have different permissions.

---

# 28. Linux Groups

Groups allow users to be organized and permissions to be managed more easily.

Example:

```text
Developers
   |
   +-- Rahul
   +-- Priya
   +-- Amit
```

You can give permissions to the group instead of individually configuring every user.

---

# 29. Linux File Ownership

Every file has ownership information.

Typically:

```text
Owner
Group
Others
```

Example:

```text
-rw-r--r--  sahil  developers  application.log
```

This tells us:

```text
Owner  → sahil
Group  → developers
```

Permissions control what each category can do.

---

# 30. Linux File Permissions

Linux permissions commonly use:

```text
r
w
x
```

Meaning:

```text
r = read
w = write
x = execute
```

And they apply to:

```text
Owner
Group
Others
```

Example:

```text
-rwxr-xr--
```

You will learn permissions deeply in a dedicated submodule.

---

# 31. Linux Processes

A process is a running program.

Example:

```text
Java Application
      ↓
Running
      ↓
Process
```

You can view processes using:

```bash
ps
```

or:

```bash
top
```

Process management is very important in Application Support.

---

# 32. Linux Services

Services are programs that provide functionality in the background.

Examples:

```text
SSH
Nginx
Apache
Database
Monitoring Agent
Application
```

You can manage many services using:

```bash
systemctl
```

Example:

```bash
systemctl status nginx
```

---

# 33. Systemd

Modern Linux distributions commonly use:

> **systemd**

as the system and service manager.

It is responsible for many system-management tasks, including:

- Starting services
- Stopping services
- Managing services
- Starting services during boot
- Managing system state
- Providing access to system logs through journald

Example:

```bash
systemctl status nginx
```

Logs:

```bash
journalctl
```

We will study this properly in the Process and Task Management submodule.

---

# 34. Package Management

Linux software is commonly installed and managed through package managers.

Examples:

### Debian/Ubuntu

```bash
apt
```

### RHEL/Fedora/Rocky/AlmaLinux

```bash
dnf
```

### RPM-based low-level package tool

```bash
rpm
```

### Arch

```bash
pacman
```

Example:

```bash
sudo apt install nginx
```

The package manager can handle:

- Installation
- Removal
- Updates
- Dependencies
- Package information

---

# 35. Environment Variables

Linux applications often use environment variables for configuration.

Example:

```bash
export APP_ENV=production
```

Then:

```bash
echo $APP_ENV
```

Output:

```text
production
```

Applications can use these values during execution.

We will cover environment variables in more detail in the Linux Basics submodule.

---

# 36. SSH

SSH stands for:

> **Secure Shell**

SSH allows you to securely connect to a remote system.

Example:

```text
Your Computer
      |
      | SSH
      ↓
Linux Server
```

Example command:

```bash
ssh username@server-ip
```

SSH is extremely important for Application Support and cloud environments.

---

# 37. SCP

SCP stands for:

> **Secure Copy Protocol**

It can be used to copy files between systems over SSH.

Example:

```bash
scp file.txt user@server:/tmp/
```

You might use it to transfer:

- Logs
- Configuration files
- Scripts
- Application packages
- Diagnostic files

We will study SCP properly in the Remote Server Management submodule.

---

# 38. Linux Troubleshooting

Linux troubleshooting is one of the most important skills for Application Support.

Suppose:

> "Application is down."

Don't immediately restart it.

Follow a process.

```text
Application Down
      ↓
Check Service
      ↓
Check Process
      ↓
Check CPU
      ↓
Check Memory
      ↓
Check Disk
      ↓
Check Network
      ↓
Check Logs
      ↓
Check Configuration
      ↓
Identify Root Cause
      ↓
Apply Safe Fix
      ↓
Validate
      ↓
Document
```

This approach will be practiced throughout the Linux modules.

---

# 39. What We Need to Learn in Linux

Linux is a large topic.

If we put everything into one `.md` file, the notes will become too large and difficult to study.

So we will divide Linux into smaller practical submodules.

This will make learning easier.

---

# Linux Course Structure

```text
MODULE 4
Linux Fundamentals
│
├── Submodule 4.1
│   Linux Basics
│
├── Submodule 4.2
│   User & Group Management
│
├── Submodule 4.3
│   File Permissions & Ownership
│
├── Submodule 4.4
│   Network Management
│
├── Submodule 4.5
│   Process & Task Management
│
├── Submodule 4.6
│   File & Directory Management
│
├── Submodule 4.7
│   Services & Systemd
│
├── Submodule 4.8
│   Package Management
│
├── Submodule 4.9
│   Environment Variables & Shell
│
├── Submodule 4.10
│   Shell Scripting Fundamentals
│
├── Submodule 4.11
│   SSH, SCP & Remote Server Management
│
└── Submodule 4.12
    Linux Troubleshooting
```

---

# 40. Submodule 4.1 — Linux Basics

This will be our starting point.

We will learn:

```text
Linux Introduction
Linux History
Linux Distributions
Linux Architecture
Terminal
CLI vs GUI
Shell
Types of Shell
Bash
Linux Commands
Command Syntax
Options
Arguments
Absolute Path
Relative Path
Command History
Manual Pages
Getting Help
Aliases
Basic System Information
```

### Basic Commands

We will practice commands such as:

```bash
pwd
ls
cd
clear
whoami
id
hostname
uname
date
cal
history
man
help
echo
```

The goal is to become comfortable with the Linux terminal.

---

# 41. Submodule 4.2 — User & Group Management

Linux is a multi-user operating system.

We will learn:

```text
Users
Groups
Root User
Normal Users
User IDs
Group IDs
Home Directories
Creating Users
Deleting Users
Modifying Users
Creating Groups
Deleting Groups
Adding Users to Groups
Removing Users from Groups
Password Management
Switching Users
sudo
su
```

Important commands will include:

```bash
useradd
adduser
usermod
userdel
groupadd
groupdel
passwd
id
whoami
who
su
sudo
```

### Practical Goal

Students should be able to perform:

```text
Create user
      ↓
Create group
      ↓
Add user to group
      ↓
Set password
      ↓
Modify user
      ↓
Remove user
```

---

# 42. Submodule 4.3 — File Permissions & Ownership

This is one of the most important Linux topics.

We will learn:

```text
File Ownership
Owner
Group
Others

Read
Write
Execute

Numeric Permissions
Symbolic Permissions

chmod
chown
chgrp

Special Permissions
SUID
SGID
Sticky Bit
```

Example:

```text
-rwxr-xr--
```

Students will learn how to understand and modify permissions safely.

---

# 43. Submodule 4.4 — Network Management

Linux servers communicate constantly with other systems.

We will learn:

```text
IP Address
IPv4
IPv6
MAC Address
Hostname
DNS
Gateway
Ports
TCP
UDP
Routing
Network Interfaces
Connectivity Testing
Network Troubleshooting
```

Commands will include:

```bash
ip
ping
ss
curl
traceroute
nslookup
dig
hostname
```

Depending on the distribution, some additional networking tools may need to be installed.

### Practical Example

```text
Application
    ↓
API Server
    ↓
Network
    ↓
Database
```

If the application cannot connect to the database, we will learn how to investigate the network path.

---

# 44. Submodule 4.5 — Process & Task Management

We will learn how Linux manages running programs.

Topics:

```text
Process
PID
PPID
Foreground Process
Background Process
Process States
CPU Usage
Memory Usage
Signals
kill
Process Priority
nice
renice
top
ps
htop
jobs
bg
fg
```

We will also learn scheduled tasks:

```text
cron
crontab
at
systemd timers
```

### Practical Example

```text
Every night at 2 AM
        ↓
Backup script runs
        ↓
Backup completed
        ↓
Log generated
```

This is very common in real production environments.

---

# 45. Submodule 4.6 — File & Directory Management

We will work extensively with the Linux filesystem.

Topics:

```text
Linux filesystem
Root directory
Directories
Files
Absolute paths
Relative paths
Creating files
Creating directories
Copying
Moving
Renaming
Deleting
Searching
Finding files
Viewing files
File metadata
Disk usage
Archives
Compression
```

Important commands:

```bash
ls
pwd
cd
mkdir
touch
cp
mv
rm
find
locate
file
stat
cat
less
head
tail
du
df
tar
gzip
zip
unzip
```

We will also study important directories:

```text
/
/etc
/var
/var/log
/home
/tmp
/opt
/usr
/bin
/sbin
/proc
/dev
/root
/boot
```

---

# 46. Submodule 4.7 — Services & Systemd

We will learn how Linux services work.

Topics:

```text
What is a service?
What is a daemon?
systemd
systemctl
Service status
Start
Stop
Restart
Reload
Enable
Disable
Service dependencies
Boot-time services
Service logs
```

Commands:

```bash
systemctl status
systemctl start
systemctl stop
systemctl restart
systemctl reload
systemctl enable
systemctl disable
systemctl is-active
systemctl is-enabled
```

---

# 47. Submodule 4.8 — Package Management

Different distributions use different package management systems.

We will learn:

```text
APT
DNF
YUM
RPM
DEB
Package repositories
Installing packages
Removing packages
Updating packages
Searching packages
Checking installed packages
Package dependencies
```

Examples:

```bash
apt
dnf
rpm
```

### Industrial Example

Suppose an application requires:

```text
Java
Nginx
Python
curl
git
```

Instead of manually downloading everything, package managers can install and maintain software using repositories.

---

# 48. Submodule 4.9 — Environment Variables & Shell

We will learn:

```text
Environment Variables
PATH
HOME
USER
SHELL
PWD
Export
Shell Variables
Environment Variables vs Shell Variables
.profile
.bashrc
/etc/environment
```

Example:

```bash
echo $PATH
```

and:

```bash
export APP_ENV=production
```

This is important because real applications frequently depend on environment configuration.

---

# 49. Submodule 4.10 — Shell Scripting Fundamentals

Once students know commands, we will start automation.

Topics:

```text
What is Shell Scripting?
Why use scripts?
Variables
Input
Output
Conditions
if
else
elif
Loops
for
while
Functions
Arguments
Exit Codes
Command Substitution
Reading Files
Checking Processes
Checking Disk
Error Handling
```

Example:

```bash
#!/bin/bash

echo "Checking disk usage"

df -h
```

Later:

```text
Script
  ↓
Check Disk
  ↓
If usage > 80%
  ↓
Generate Alert
```

This is directly useful for Application Support.

---

# 50. Submodule 4.11 — SSH, SCP & Remote Server Management

This submodule focuses on working with remote Linux servers.

Topics:

```text
SSH
SSH Keys
Password Authentication
Public Key Authentication
SSH Configuration
SCP
Remote Commands
Remote File Transfer
Known Hosts
SSH Troubleshooting
```

Example:

```bash
ssh user@server
```

File transfer:

```bash
scp application.log user@server:/tmp/
```

---

# 51. Submodule 4.12 — Linux Troubleshooting

This will bring everything together.

We will troubleshoot real Application Support scenarios.

Examples:

### Scenario 1

```text
Application is down
```

### Scenario 2

```text
CPU is 100%
```

### Scenario 3

```text
Memory is full
```

### Scenario 4

```text
Disk is full
```

### Scenario 5

```text
Service failed
```

### Scenario 6

```text
Application cannot connect to database
```

### Scenario 7

```text
Permission denied
```

### Scenario 8

```text
User cannot login
```

### Scenario 9

```text
Application is slow
```

### Scenario 10

```text
Server is not reachable
```

---

# 52. Linux Troubleshooting Framework

We will develop a standard troubleshooting process:

```text
1. Understand the problem
        ↓
2. Check impact
        ↓
3. Check server availability
        ↓
4. Check CPU
        ↓
5. Check memory
        ↓
6. Check disk
        ↓
7. Check processes
        ↓
8. Check services
        ↓
9. Check network
        ↓
10. Check logs
        ↓
11. Check configuration
        ↓
12. Identify root cause
        ↓
13. Apply safe fix
        ↓
14. Validate
        ↓
15. Document
```

This process will connect Linux knowledge with the ITSM concepts learned in Module 2.

---

# 53. How Module 2 and Linux Connect

Earlier we learned:

```text
Incident
Problem
Service Request
Change
SLA
Priority
Escalation
RCA
```

Now Linux gives us the technical skills to investigate those tickets.

Example:

```text
ITSM Ticket
     ↓
"Application is down"
     ↓
Linux Server
     ↓
Check Service
     ↓
Check Process
     ↓
Check Logs
     ↓
Check Disk
     ↓
Find Root Cause
     ↓
Fix
     ↓
Update Ticket
```

This is exactly how theory becomes practical Application Support work.

---

# 54. Linux Learning Roadmap

Our complete learning path will be:

```text
Module 4
Linux Fundamentals
        |
        ↓
4.1 Linux Basics
        |
        ↓
4.2 User & Group Management
        |
        ↓
4.3 Permissions & Ownership
        |
        ↓
4.4 Network Management
        |
        ↓
4.5 Process & Task Management
        |
        ↓
4.6 File & Directory Management
        |
        ↓
4.7 Services & Systemd
        |
        ↓
4.8 Package Management
        |
        ↓
4.9 Environment Variables & Shell
        |
        ↓
4.10 Shell Scripting
        |
        ↓
4.11 SSH / SCP / Remote Management
        |
        ↓
4.12 Linux Troubleshooting
```

---

# 55. What Students Should Be Able to Do After Module 4

By the end of the Linux module, students should be able to:

### Basic Linux

```text
Use terminal
Run commands
Navigate filesystem
Find files
Read files
Understand paths
```

### Users & Groups

```text
Create users
Create groups
Modify users
Delete users
Manage group membership
Manage passwords
Use sudo
```

### Permissions

```text
Understand r/w/x
Change permissions
Change ownership
Use chmod
Use chown
Understand special permissions
```

### Networking

```text
Check IP
Check connectivity
Check ports
Check DNS
Check routes
Troubleshoot basic network issues
```

### Processes

```text
Find processes
Check CPU usage
Check memory usage
Stop processes
Manage background tasks
Schedule jobs
```

### Files

```text
Create
Copy
Move
Rename
Delete
Search
Archive
Compress
Check disk usage
```

### Services

```text
Check service
Start service
Stop service
Restart service
Enable service
Read service logs
```

### Packages

```text
Install software
Remove software
Update software
Search packages
Understand repositories
```

### Shell

```text
Understand Bash
Use variables
Use environment variables
Run commands
Create scripts
```

### Remote Management

```text
Connect through SSH
Use SSH keys
Copy files using SCP
Run remote commands
```

### Troubleshooting

```text
Check CPU
Check RAM
Check disk
Check processes
Check services
Check network
Check logs
Find root cause
Document findings
```

---

# 56. Important Linux Mindset

While learning Linux, don't focus only on:

> "Which command should I type?"

Instead ask:

> **"Why am I running this command?"**

For example:

Don't just memorize:

```bash
df -h
```

Understand:

> "I use `df -h` when I want to understand filesystem disk-space usage."

Don't just memorize:

```bash
ps aux
```

Understand:

> "I use `ps aux` to inspect running processes and their resource usage."

Don't just memorize:

```bash
systemctl status nginx
```

Understand:

> "I use it to check whether the Nginx service is running and inspect its current status."

This way, commands become tools for troubleshooting instead of random things to memorize.

---

# 57. Real Industry Mindset

Imagine it is 2:00 AM.

You are the Application Support Engineer on call.

You receive:

```text
CRITICAL INCIDENT

Production application is down.

Thousands of customers are affected.
```

You connect to the server.

You don't panic.

You start thinking:

```text
Is the server reachable?
        ↓
Is the application service running?
        ↓
Is the process running?
        ↓
Is CPU normal?
        ↓
Is memory normal?
        ↓
Is disk full?
        ↓
Is network working?
        ↓
What do the logs say?
        ↓
What changed recently?
        ↓
Can I safely restore the service?
        ↓
Do I need to escalate?
```

That is the purpose of learning Linux in this course.

---

# 58. Final Summary

Linux is much more than a collection of commands.

It is an operating-system environment that provides the foundation for many:

- Servers
- Applications
- Cloud workloads
- Containers
- DevOps tools
- Databases
- Network systems
- Production environments

For an Application Support Engineer, Linux helps you understand and troubleshoot:

```text
Users
   ↓
Applications
   ↓
Processes
   ↓
Services
   ↓
Files
   ↓
Permissions
   ↓
Network
   ↓
CPU
   ↓
Memory
   ↓
Disk
   ↓
Logs
```

The Linux module will therefore be taught as **small practical submodules**, rather than one huge document.

---

# 59. Module 4 Submodule List

| No. | Submodule | Main Focus |
|---:|---|---|
| 4.1 | Linux Basics | Linux, terminal, commands, shell, Bash |
| 4.2 | User & Group Management | Users, groups, root, sudo |
| 4.3 | Permissions & Ownership | r/w/x, chmod, chown, special permissions |
| 4.4 | Network Management | IP, DNS, ports, connectivity, troubleshooting |
| 4.5 | Process & Task Management | Processes, signals, jobs, cron |
| 4.6 | File & Directory Management | Files, directories, search, storage, archives |
| 4.7 | Services & Systemd | Services, daemons, systemctl, journalctl |
| 4.8 | Package Management | apt, dnf, rpm, packages, repositories |
| 4.9 | Environment Variables & Shell | Variables, PATH, Bash configuration |
| 4.10 | Shell Scripting Fundamentals | Scripts, conditions, loops, functions |
| 4.11 | SSH, SCP & Remote Management | Remote Linux administration |
| 4.12 | Linux Troubleshooting | Real production troubleshooting |

---

# 60. Recommended Learning Order

The order matters.

We will **not** jump directly into advanced commands.

We will learn like this:

```text
                    LINUX
                      |
                      ↓
              Linux Basics
                      |
                      ↓
             Files & Directories
                      |
                      ↓
             Users & Groups
                      |
                      ↓
          Permissions & Ownership
                      |
                      ↓
             Network Management
                      |
                      ↓
          Processes & Task Management
                      |
                      ↓
             Services & Systemd
                      |
                      ↓
             Package Management
                      |
                      ↓
        Environment Variables & Shell
                      |
                      ↓
             Shell Scripting
                      |
                      ↓
              SSH / SCP
                      |
                      ↓
          Remote Server Management
                      |
                      ↓
            Linux Troubleshooting
```

By following this order, each topic will build on the previous one.

---

# Module 4 — Main Module Completed

The main Linux module has now established:

```text
What is Linux?
        ↓
Linux History
        ↓
Why Linux is Important
        ↓
Linux Benefits
        ↓
Linux Architecture
        ↓
Linux Kernel
        ↓
Linux Distributions
        ↓
Linux Filesystem Overview
        ↓
Linux Users & Groups
        ↓
Linux Permissions
        ↓
Linux Processes
        ↓
Linux Services
        ↓
Linux Networking
        ↓
Linux Shell
        ↓
Linux Package Management
        ↓
Linux Remote Management
        ↓
Linux Troubleshooting
```

The detailed hands-on learning will now happen inside the **12 separate Linux submodules** listed above.

> **Next:** `04.1-Linux-Basics.md`
