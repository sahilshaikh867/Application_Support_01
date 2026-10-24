# 🐧 Linux Users & Groups Management — Practical Industry Lab

> **Level:** Beginner → Intermediate  
> **Module:** Linux Administration / Application Support / DevOps  
> **Topic:** Users, Groups, UID, GID, Permissions & Access Management  
> **Environment:** Ubuntu / Debian / RHEL / CentOS / Amazon Linux  
> **Lab Type:** Hands-on Industry Scenario  
> **Trainer:** ASE Trainer — **Mr. Sahil Shaikh**

---

# 📌 1. Lab Objective

In this practical lab, you will learn how Linux manages:

- Root user
- System users
- Normal users
- User IDs — UID
- Group IDs — GID
- Home directories
- User shells
- User permissions
- User ownership
- Groups
- Group membership
- Creating users
- Switching users
- Modifying users
- Renaming users
- Changing UID/GID
- Creating groups
- Adding/removing group members
- Checking user permissions
- Locking/unlocking users
- Deleting users
- Checking login information
- Understanding `/etc/passwd`
- Understanding `/etc/shadow`
- Understanding `/etc/group`
- Understanding `/etc/gshadow`

At the end, you will perform a complete **real-world production server access-management scenario**.

---

# 🏢 2. Real-World Industry Scenario

Imagine that you are a **Linux Administrator / DevOps Engineer** working for a company.

Your company has an application server:

```text
Production Application Server
        |
        +----------------------+
        |                      |
     Developers             Support Team
        |                      |
     developer1             support1
     developer2             support2
        |
     devops1
```

The server contains an application:

```text
/opt/company-app/
```

Different employees need different levels of access.

### Business requirement

| User | Role | Required Access |
|---|---|---|
| root | System Administrator | Complete system access |
| devops1 | DevOps Engineer | Application + deployment access |
| developer1 | Developer | Application source access |
| developer2 | Developer | Application source access |
| support1 | Support Engineer | Logs/read access |
| support2 | Support Engineer | Logs/read access |
| appsvc | Application Service | Run application only |

We will configure this environment throughout the lab.

---

# 🧰 3. Lab Requirements

You can perform this lab on:

- Ubuntu
- Debian
- Rocky Linux
- AlmaLinux
- CentOS
- Amazon Linux

Recommended:

```text
Ubuntu Server 22.04+
```

You should have:

```text
Terminal
SSH access
sudo/root access
```

Check your operating system:

```bash
cat /etc/os-release
```

Example:

```text
NAME="Ubuntu"
VERSION="22.04..."
```

---

# 🔴 PART 1 — Understanding the Root User

## 4. What is root?

`root` is the Linux superuser.

Root has almost unrestricted access to the operating system.

Root can:

- Create users
- Delete users
- Modify users
- Change passwords
- Install software
- Modify system files
- Change ownership
- Change permissions
- Stop services
- Start services
- Access protected directories
- Modify networking
- Modify system configuration

Root UID is normally:

```text
UID = 0
```

---

# 5. Check Current User

Run:

```bash
whoami
```

Example:

```text
root
```

Another command:

```bash
id
```

Example:

```text
uid=0(root) gid=0(root) groups=0(root)
```

This confirms:

```text
Username = root
UID      = 0
GID      = 0
```

---

# 6. Check Root User Information

Run:

```bash
getent passwd root
```

Example:

```text
root:x:0:0:root:/root:/bin/bash
```

Structure:

```text
root:x:0:0:root:/root:/bin/bash
 |   | | |  |    |       |
 |   | | |  |    |       +-- Login shell
 |   | | |  |    +---------- Home directory
 |   | | |  +--------------- User description
 |   | | +------------------ Primary GID
 |   | +-------------------- UID
 |   +---------------------- Password field
 +-------------------------- Username
```

---

# 7. Check Root Home Directory

Root's home directory is:

```text
/root
```

Check:

```bash
ls -ld /root
```

Example:

```text
drwx------ 5 root root 4096 Oct 23 19:00 /root
```

Notice:

```text
Owner = root
Group = root
```

---

# 8. Enter Root Home Directory

If you are root:

```bash
cd /root
```

Check:

```bash
pwd
```

Output:

```text
/root
```

List files:

```bash
ls -la
```

You may see files such as:

```text
.bashrc
.profile
.bash_history
```

These are root's personal configuration files.

---

# 9. Check Root Shell

Run:

```bash
getent passwd root
```

The last field tells us the shell:

```text
/bin/bash
```

You can also run:

```bash
echo $SHELL
```

---

# 🟠 PART 2 — Understanding System Users

# 10. What is a System User?

A **system user** is generally created for running services or system processes.

Examples:

```text
www-data
nginx
apache
postgres
redis
mysql
daemon
```

These accounts normally aren't intended for human interactive login.

For example:

```text
nginx
```

may run the Nginx web server.

Instead of running the web server as root:

```text
root
 |
 +-- nginx service
```

Linux can run it under:

```text
nginx
```

This follows the security principle:

> Give a service only the privileges it needs.

---

# 11. List System Users

On many Linux systems, system users have lower UIDs.

Check:

```bash
awk -F: '$3 < 1000 {print $1, $3}' /etc/passwd
```

Example:

```text
root 0
daemon 1
bin 2
sys 3
sync 4
games 5
man 6
lp 7
mail 8
www-data 33
```

The exact UID range varies between distributions.

Do **not** assume every Linux distribution uses exactly the same boundary.

---

# 12. Check a System User

Try:

```bash
getent passwd www-data
```

Example:

```text
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

Important:

```text
Username = www-data
UID      = 33
GID      = 33
Home     = /var/www
Shell    = /usr/sbin/nologin
```

---

# 13. Why `/usr/sbin/nologin`?

A service account often has:

```text
/usr/sbin/nologin
```

This prevents normal interactive login.

Example:

```bash
su - www-data
```

may return:

```text
This account is currently not available.
```

This is a security feature.

---

# 14. Find System User Home Directories

Run:

```bash
getent passwd | awk -F: '$3 < 1000 {print $1, $6}'
```

Example:

```text
root /root
daemon /usr/sbin
bin /bin
www-data /var/www
```

Remember:

> A system user may have a home-directory field, but the directory does not necessarily exist or behave like a normal user's home.

---

# 🟢 PART 3 — Normal Users

# 15. What is a Normal User?

A normal user usually represents a human user.

Examples:

```text
sahil
developer1
developer2
devops1
support1
```

Normal users typically have:

```text
UID >= normal user threshold
Home directory
Login shell
Primary group
Password
```

---

# 16. Create a Normal User

Create:

```bash
sudo useradd -m -s /bin/bash developer1
```

Explanation:

```text
useradd      → create user
-m           → create home directory
-s           → specify login shell
/bin/bash    → Bash shell
```

---

# 17. Set Password

```bash
sudo passwd developer1
```

Enter the password when prompted.

---

# 18. Check User

```bash
id developer1
```

Example:

```text
uid=1001(developer1) gid=1001(developer1) groups=1001(developer1)
```

---

# 19. Check User Information

```bash
getent passwd developer1
```

Example:

```text
developer1:x:1001:1001::/home/developer1:/bin/bash
```

---

# 20. Check Home Directory

```bash
ls -ld /home/developer1
```

Example:

```text
drwxr-x--- 2 developer1 developer1 4096 Oct 23 19:10 /home/developer1
```

The home directory is:

```text
/home/developer1
```

---

# 21. Switch to the User

Run:

```bash
su - developer1
```

Now check:

```bash
whoami
```

Output:

```text
developer1
```

Check:

```bash
pwd
```

Output:

```text
/home/developer1
```

Check identity:

```bash
id
```

---

# 22. Return to Root

Run:

```bash
exit
```

Check:

```bash
whoami
```

---

# 🔵 PART 4 — Understanding `/etc/passwd`

# 23. What is `/etc/passwd`?

Linux stores basic user account information in:

```text
/etc/passwd
```

View it:

```bash
cat /etc/passwd
```

Better:

```bash
less /etc/passwd
```

---

# 24. `/etc/passwd` Structure

Example:

```text
developer1:x:1001:1001::/home/developer1:/bin/bash
```

There are seven fields:

```text
username
password placeholder
UID
GID
comment
home directory
login shell
```

Separated by:

```text
:
```

---

# 25. Extract User Information

Run:

```bash
awk -F: '{print $1, $3, $4, $6, $7}' /etc/passwd
```

This displays:

```text
USERNAME UID GID HOME SHELL
```

---

# 🟣 PART 5 — `/etc/shadow`

# 26. What is `/etc/shadow`?

Password-related authentication information is stored in:

```text
/etc/shadow
```

Check:

```bash
sudo cat /etc/shadow
```

Do not casually modify this file.

Its permissions are intentionally restrictive.

Check:

```bash
ls -l /etc/shadow
```

---

# 27. Why is `/etc/shadow` Protected?

Passwords and authentication data are sensitive.

Linux therefore restricts access to this file.

This is one reason we should not:

```bash
chmod 777 /etc/shadow
```

Never do that in a production system.

---

# 🟡 PART 6 — UID and GID

# 28. What is UID?

UID means:

```text
User ID
```

Linux internally identifies users using numbers.

Example:

```text
developer1 → UID 1001
```

Linux permissions operate on ownership identities, ultimately based on UID/GID.

---

# 29. Check UID

```bash
id -u developer1
```

Example:

```text
1001
```

---

# 30. Check Full Identity

```bash
id developer1
```

Example:

```text
uid=1001(developer1) gid=1001(developer1) groups=1001(developer1)
```

---

# 31. What is GID?

GID means:

```text
Group ID
```

Check:

```bash
id -g developer1
```

---

# 32. Check Group Name

```bash
id -gn developer1
```

Example:

```text
developer1
```

---

# 33. Check All Groups

```bash
id -Gn developer1
```

Example:

```text
developer1 developers docker
```

---

# 🔐 PART 7 — Linux Permission Model

# 34. Check File Permissions

Create a file:

```bash
touch testfile
```

Run:

```bash
ls -l testfile
```

Example:

```text
-rw-r--r-- 1 root root 0 Oct 23 19:20 testfile
```

Break it down:

```text
-rw-r--r--
 ||| ||| |||
 ||| ||| +++--- Others
 ||| +++------- Group
 +++----------- Owner
```

Meaning:

```text
Owner  → rw-
Group  → r--
Others → r--
```

---

# 35. Permission Values

Linux uses:

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
rwx = 7
rw- = 6
r-x = 5
r-- = 4
-wx = 3
-w- = 2
--x = 1
--- = 0
```

---

# 36. Common Permission Examples

```bash
chmod 755 script.sh
```

Means:

```text
Owner  = rwx
Group  = r-x
Others = r-x
```

Another:

```bash
chmod 640 application.conf
```

Means:

```text
Owner  = rw-
Group  = r--
Others = ---
```

---

# 🟤 PART 8 — Create Industry Groups

Our company needs these groups:

```text
developers
devops
support
appadmins
```

Create them:

```bash
sudo groupadd developers
sudo groupadd devops
sudo groupadd support
sudo groupadd appadmins
```

---

# 37. Check Groups

```bash
getent group developers
getent group devops
getent group support
getent group appadmins
```

---

# 38. List All Groups

```bash
getent group
```

Or:

```bash
cat /etc/group
```

---

# 39. `/etc/group`

Example:

```text
developers:x:1002:developer1,developer2
```

Structure:

```text
group_name
password_placeholder
GID
members
```

---

# 🟢 PART 9 — Add Users to Groups

Create another developer:

```bash
sudo useradd -m -s /bin/bash developer2
sudo passwd developer2
```

Create DevOps user:

```bash
sudo useradd -m -s /bin/bash devops1
sudo passwd devops1
```

Create support user:

```bash
sudo useradd -m -s /bin/bash support1
sudo passwd support1
```

---

# 40. Add Developers to Developers Group

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
```

Important:

```text
-a = append
-G = supplementary groups
```

Never casually use:

```bash
usermod -G developers developer1
```

because it can replace the user's supplementary group list.

Safer:

```bash
usermod -aG developers developer1
```

---

# 41. Add DevOps User

```bash
sudo usermod -aG devops devops1
sudo usermod -aG appadmins devops1
```

---

# 42. Add Support User

```bash
sudo usermod -aG support support1
```

---

# 43. Verify Membership

```bash
id developer1
```

Expected:

```text
groups=...,developers
```

Run:

```bash
groups developer1
```

---

# 44. List Members of a Group

```bash
getent group developers
```

Example:

```text
developers:x:1002:developer1,developer2
```

---

# 45. Check Current User's Groups

Switch:

```bash
su - developer1
```

Then:

```bash
groups
```

Or:

```bash
id
```

Return:

```bash
exit
```

---

# 🔴 PART 10 — Modify Users

# 46. Change User Shell

Change developer1's shell:

```bash
sudo usermod -s /bin/bash developer1
```

Check:

```bash
getent passwd developer1
```

---

# 47. Change User Home Directory

Example:

```bash
sudo usermod -d /home/developer1-new developer1
```

However, changing the home path does **not automatically move existing files**.

For moving the home directory:

```bash
sudo usermod -d /home/developer1-new -m developer1
```

The `-m` option attempts to move the existing home contents.

---

# 48. Rename a User

Rename:

```bash
sudo usermod -l developer-main developer1
```

Check:

```bash
getent passwd developer-main
```

Important:

`-l` changes the login name.

It does not automatically solve every dependency involving the old username.

Production systems may have:

- cron jobs
- scripts
- file ownership
- SSH keys
- application configuration
- monitoring configuration

Therefore user renaming should be planned carefully.

---

# 49. Change UID

Example:

```bash
sudo usermod -u 1500 developer-main
```

Check:

```bash
id developer-main
```

Important:

Changing a UID can create ownership inconsistencies.

Files previously owned by the old numeric UID may need ownership correction.

Find files by old UID:

```bash
sudo find / -uid OLD_UID 2>/dev/null
```

Then update ownership carefully.

---

# 50. Change Primary Group

Create:

```bash
sudo groupadd projectteam
```

Set primary group:

```bash
sudo usermod -g projectteam developer-main
```

Check:

```bash
id developer-main
```

---

# 🟠 PART 11 — Check User Permissions

# 51. Check Home Directory Permissions

```bash
ls -ld /home/developer-main
```

Example:

```text
drwxr-x--- developer-main developer-main /home/developer-main
```

---

# 52. Check Ownership

```bash
stat /home/developer-main
```

You can see:

```text
Uid
Gid
Access
Modify
Change
```

---

# 53. Check Effective Access

A useful command:

```bash
namei -l /home/developer-main
```

This helps investigate permissions across the complete path.

---

# 54. Check Sudo Privileges

For the current user:

```bash
sudo -l
```

This shows what commands the user may run through sudo.

Example:

```text
User devops1 may run:
    (ALL) /usr/bin/systemctl
```

Exact output depends on configuration.

---

# 🔵 PART 12 — Switching Users

# 55. `su`

Switch user:

```bash
su - developer-main
```

The `-` creates a login-style environment.

Check:

```bash
whoami
pwd
echo $HOME
```

---

# 56. `sudo`

Run one command as root:

```bash
sudo systemctl status nginx
```

Run a root shell:

```bash
sudo -i
```

Check:

```bash
whoami
```

---

# 57. Difference Between `su` and `sudo`

### `su`

```bash
su - username
```

You become another user.

### `sudo`

```bash
sudo command
```

You execute a command with elevated privileges.

In enterprise environments, `sudo` is usually preferred for controlled administrative access because it supports auditing and fine-grained authorization.

---

# 🟣 PART 13 — Remove User From Group

Remove:

```bash
sudo gpasswd -d developer2 developers
```

Verify:

```bash
groups developer2
```

---

# 58. Alternative Method

You can use:

```bash
sudo deluser developer2 developers
```

On distributions that provide `deluser`.

For portability, understand the `gpasswd` approach:

```bash
gpasswd -d USER GROUP
```

---

# 🔴 PART 14 — Lock and Unlock Users

## 59. Lock a User

```bash
sudo passwd -l developer2
```

Check status:

```bash
sudo passwd -S developer2
```

---

# 60. Unlock User

```bash
sudo passwd -u developer2
```

---

# 61. Expire Password

```bash
sudo passwd -e developer2
```

The user will be required to change the password at next login.

---

# 62. Check Password Aging

```bash
sudo chage -l developer2
```

Example information:

```text
Last password change
Password expires
Password inactive
Account expires
Minimum number of days
Maximum number of days
```

---

# 🟢 PART 15 — Delete Users

## 63. Delete User

```bash
sudo userdel developer2
```

This removes the account but may leave the home directory.

---

# 64. Delete User and Home Directory

```bash
sudo userdel -r developer2
```

The `-r` removes the user's home directory and mail spool where applicable.

⚠️ **Production Warning**

Never execute:

```bash
userdel -r username
```

without verifying:

```bash
id username
ls -la /home/username
find / -user username
```

A production account may own:

- Application files
- Cron jobs
- SSH keys
- Logs
- Deployment files
- Configuration files

---

# 🏢 PART 16 — REAL INDUSTRY PRACTICAL

# Production Access Management Lab

Now we will build a realistic company access structure.

---

# 65. Company Structure

Create:

```text
Company
│
├── developers
│   ├── developer1
│   └── developer2
│
├── devops
│   └── devops1
│
├── support
│   └── support1
│
└── appadmins
    └── devops1
```

---

# 66. Create Users

Run:

```bash
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
sudo useradd -m -s /bin/bash devops1
sudo useradd -m -s /bin/bash support1
```

Set passwords:

```bash
sudo passwd developer1
sudo passwd developer2
sudo passwd devops1
sudo passwd support1
```

---

# 67. Create Service User

Create an application service account:

```bash
sudo useradd \
  --system \
  --no-create-home \
  --shell /usr/sbin/nologin \
  appsvc
```

Check:

```bash
id appsvc
```

Check:

```bash
getent passwd appsvc
```

This account is intended to run the application, not for normal human login.

---

# 68. Create Groups

```bash
sudo groupadd developers
sudo groupadd devops
sudo groupadd support
sudo groupadd appadmins
```

---

# 69. Add Users

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2

sudo usermod -aG devops devops1
sudo usermod -aG appadmins devops1

sudo usermod -aG support support1
```

---

# 70. Verify Everything

Run:

```bash
id developer1
id developer2
id devops1
id support1
id appsvc
```

Then:

```bash
groups developer1
groups developer2
groups devops1
groups support1
```

---

# 📁 PART 17 — Create Application Directory

Create:

```bash
sudo mkdir -p /opt/company-app
```

Create application files:

```bash
sudo touch /opt/company-app/app.py
sudo touch /opt/company-app/config.conf
sudo touch /opt/company-app/application.log
```

Check:

```bash
ls -l /opt/company-app
```

---

# 71. Create Developer Workspace

```bash
sudo mkdir -p /opt/company-app/src
```

Create files:

```bash
sudo touch /opt/company-app/src/main.py
sudo touch /opt/company-app/src/database.py
sudo touch /opt/company-app/src/api.py
```

---

# 72. Set Group Ownership

Make developers group owner:

```bash
sudo chown -R root:developers /opt/company-app/src
```

Check:

```bash
ls -l /opt/company-app/src
```

---

# 73. Set Developer Permissions

```bash
sudo chmod -R 2770 /opt/company-app/src
```

Why `2770`?

```text
Owner = rwx
Group = rwx
Others = ---
Setgid = enabled
```

The setgid bit helps newly created files/directories inherit the directory's group in typical Unix/Linux filesystem behavior.

Check:

```bash
ls -ld /opt/company-app/src
```

You may see:

```text
drwxrws---
```

The `s` indicates setgid.

---

# 🛠️ PART 18 — Support Team Access

Support engineers need logs but should not modify source code.

Create log directory:

```bash
sudo mkdir -p /opt/company-app/logs
```

Create log:

```bash
sudo touch /opt/company-app/logs/application.log
```

Set ownership:

```bash
sudo chown -R root:support /opt/company-app/logs
```

Set permissions:

```bash
sudo chmod -R 2750 /opt/company-app/logs
```

Now:

```text
root      → full access
support   → read/execute
others    → no access
```

---

# 🔐 PART 19 — Test Access

## Developer Test

Switch:

```bash
su - developer1
```

Try:

```bash
cd /opt/company-app/src
```

Check:

```bash
ls
```

Create a file:

```bash
touch developer-test.txt
```

Check:

```bash
ls -l
```

Return:

```bash
exit
```

---

# 74. Support Test

Switch:

```bash
su - support1
```

Try:

```bash
cd /opt/company-app/logs
```

Read:

```bash
cat application.log
```

Then try:

```bash
touch test.log
```

Depending on the configured permissions, this should fail because support has read/execute rather than write access.

This is the point of Linux permissions:

> Access is granted based on business requirements, not friendship with the server. 😄

Return:

```bash
exit
```

---

# 🔥 PART 20 — Troubleshooting User Access

Imagine developer1 says:

> "I cannot access the project directory."

Start troubleshooting.

---

# Step 1 — Check User

```bash
whoami
```

---

# Step 2 — Check Identity

```bash
id developer1
```

---

# Step 3 — Check Groups

```bash
groups developer1
```

---

# Step 4 — Check Directory Ownership

```bash
ls -ld /opt/company-app/src
```

---

# Step 5 — Check Detailed Path Permissions

```bash
namei -l /opt/company-app/src
```

---

# Step 6 — Check ACLs if applicable

```bash
getfacl /opt/company-app/src
```

If `getfacl` is unavailable, install the appropriate ACL package for your distribution.

---

# Step 7 — Check Current Session

If the user was recently added to a group, their existing login session may not have refreshed group membership.

Check:

```bash
id
```

The safest approach is usually to start a fresh login session.

---

# 🧪 PART 21 — UID Change Practical

Create:

```bash
sudo useradd -m uidtest
sudo passwd uidtest
```

Check:

```bash
id uidtest
```

Example:

```text
uid=1005(uidtest) gid=1005(uidtest)
```

Create a file:

```bash
sudo -u uidtest touch /home/uidtest/test.txt
```

Check:

```bash
ls -ln /home/uidtest/test.txt
```

Notice the numeric UID/GID.

Now change UID:

```bash
sudo usermod -u 2000 uidtest
```

Check:

```bash
id uidtest
```

Important lesson:

> Linux filesystem ownership is fundamentally tied to numeric UID/GID values.

Changing IDs requires careful ownership management.

---

# 🧪 PART 22 — GID Change Practical

Create:

```bash
sudo groupadd projectteam
```

Check:

```bash
getent group projectteam
```

Change primary group:

```bash
sudo usermod -g projectteam developer1
```

Verify:

```bash
id developer1
```

---

# 📋 PART 23 — Useful User Management Commands

| Command | Purpose |
|---|---|
| `whoami` | Show current user |
| `id` | Show UID/GID/groups |
| `id username` | Show another user's identity |
| `users` | Show logged-in users |
| `who` | Show logged-in sessions |
| `w` | Show logged-in users and activity |
| `last` | Login history |
| `getent passwd` | Query user database |
| `getent group` | Query group database |
| `useradd` | Create user |
| `usermod` | Modify user |
| `userdel` | Delete user |
| `passwd` | Manage passwords |
| `chage` | Password aging |
| `groupadd` | Create group |
| `groupmod` | Modify group |
| `groupdel` | Delete group |
| `gpasswd` | Manage group membership |
| `groups` | Show group membership |
| `su` | Switch user |
| `sudo` | Execute with elevated privileges |
| `chown` | Change ownership |
| `chgrp` | Change group |
| `chmod` | Change permissions |
| `ls -l` | View permissions |
| `stat` | Detailed file information |
| `namei -l` | Inspect path permissions |

---

# 📚 PART 24 — Important Files

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password/authentication information |
| `/etc/group` | Group information |
| `/etc/gshadow` | Secure group information |
| `/etc/login.defs` | Login/account defaults |
| `/etc/skel` | Default files for new users |
| `/etc/sudoers` | Sudo configuration |
| `/etc/sudoers.d/` | Additional sudo configuration |

---

# 🧠 PART 25 — Understanding `/etc/skel`

When creating users with a home directory, Linux can copy default files from:

```text
/etc/skel
```

Check:

```bash
ls -la /etc/skel
```

Typical files:

```text
.bashrc
.profile
```

Create a new user:

```bash
sudo useradd -m newuser
```

The home directory can receive files from `/etc/skel`.

This is useful when administrators want standardized user environments.

---

# 🏭 PART 26 — INDUSTRY CHALLENGE

You are now the Linux administrator for:

```text
ABC Technologies
```

The company has deployed a production application.

Your manager gives you the following ticket.

---

## 🎫 Ticket: Production User Access Request

### Request

Create the following accounts:

```text
rahul      → Developer
priya      → Developer
amit       → DevOps
neha       → Support
```

Create groups:

```text
developers
devops
support
appadmins
```

Requirements:

### Rahul

```text
developers
```

Access:

```text
Application source
Read/write
```

### Priya

```text
developers
```

Access:

```text
Application source
Read/write
```

### Amit

```text
devops
appadmins
```

Access:

```text
Application
Deployment
Administration
```

### Neha

```text
support
```

Access:

```text
Application logs
Read-only
```

### Application Service

Create:

```text
appsvc
```

Requirements:

```text
System account
No interactive shell
No normal home directory
```

---

# 📝 Your Task

Perform the complete configuration.

You should be able to demonstrate:

```text
1. Create groups
2. Create users
3. Set passwords
4. Check UID
5. Check GID
6. Check home directory
7. Check shell
8. Add users to groups
9. Verify group membership
10. Create application directories
11. Configure ownership
12. Configure permissions
13. Test developer access
14. Test support access
15. Test service account
16. Lock a user
17. Unlock a user
18. Rename a user
19. Modify UID
20. Modify primary GID
21. Remove a user from a group
22. Delete a test user
```

---

# 🚨 PART 27 — Production Safety Rules

Before modifying users on a production server:

### Always verify the account

```bash
id username
```

### Check processes

```bash
ps -u username
```

### Check files

```bash
sudo find / -user username 2>/dev/null
```

### Check cron jobs

```bash
sudo crontab -u username -l
```

### Check SSH keys

```bash
ls -la /home/username/.ssh/
```

### Check sudo access

```bash
sudo -l -U username
```

### Never blindly execute:

```bash
userdel -r
```

### Never blindly modify:

```text
UID
GID
home directory
shell
sudo permissions
```

on a production account.

---

# 🎯 PART 28 — Interview Questions

## Q1. What is UID?

UID is the numeric identifier used by Linux to identify a user.

---

## Q2. What is GID?

GID is the numeric identifier used to identify a group.

---

## Q3. What is UID of root?

Normally:

```text
0
```

---

## Q4. Where is user information stored?

```text
/etc/passwd
```

---

## Q5. Where is password-related information stored?

```text
/etc/shadow
```

---

## Q6. Where are groups stored?

```text
/etc/group
```

---

## Q7. What is the difference between system and normal users?

System users are generally intended for services and system processes, while normal users generally represent human users.

---

## Q8. Why use a service account?

To run applications/services with only the privileges they require instead of running them as root.

---

## Q9. What does this mean?

```bash
usermod -aG developers rahul
```

It adds `rahul` to the supplementary `developers` group without replacing existing supplementary groups.

---

## Q10. What is the danger of this?

```bash
usermod -G developers rahul
```

It can replace the user's existing supplementary groups.

---

## Q11. What does UID 0 mean?

It identifies the root/superuser account.

---

## Q12. What is `/usr/sbin/nologin`?

It is commonly assigned to accounts that should not have an interactive login shell.

---

## Q13. How do you check a user's groups?

```bash
groups username
```

or:

```bash
id username
```

---

## Q14. How do you check file ownership?

```bash
ls -l filename
```

or:

```bash
stat filename
```

---

## Q15. How do you change ownership?

```bash
chown user:group file
```

---

# 🏆 FINAL LAB CHECKLIST

Before considering this lab complete, verify that you can perform all of these without copying commands blindly:

```text
[ ] Identify current user
[ ] Identify root
[ ] Explain UID 0
[ ] Find root home directory
[ ] Explain system users
[ ] Identify service accounts
[ ] Explain normal users
[ ] Create a user
[ ] Set a password
[ ] Check UID
[ ] Check GID
[ ] Check home directory
[ ] Check shell
[ ] Switch users
[ ] Explain /etc/passwd
[ ] Explain /etc/shadow
[ ] Explain /etc/group
[ ] Create groups
[ ] Add group members
[ ] Remove group members
[ ] List group members
[ ] Change primary group
[ ] Change supplementary groups
[ ] Rename a user
[ ] Change UID
[ ] Change home directory
[ ] Change shell
[ ] Lock account
[ ] Unlock account
[ ] Delete account
[ ] Understand file ownership
[ ] Understand permissions
[ ] Configure application access
[ ] Troubleshoot permission problems
[ ] Explain service-account security
```

---

# 🚀 Final Industry Takeaway

Linux user management is not simply:

```bash
useradd
passwd
usermod
userdel
```

In a real production environment, user management is an **access-control process**.

A good administrator asks:

```text
Who is this user?
        ↓
What is their role?
        ↓
What resources do they need?
        ↓
Which group should provide that access?
        ↓
What permissions should that group have?
        ↓
Can the access be limited further?
        ↓
How will the access be audited?
        ↓
What happens when the employee leaves?
```

The goal is not:

> "Give the user access."

The goal is:

> **Give the user exactly the access they need — and nothing more.**

This is the foundation of **Linux administration, DevOps, SRE, cloud infrastructure, application support and production security.**

---

# 🧑‍💻 Lab Completion Statement

After completing this lab, you should be able to confidently manage Linux identities and access in a production-style environment involving:

```text
Users
   ↓
UID
   ↓
Groups
   ↓
GID
   ↓
Ownership
   ↓
Permissions
   ↓
Application Access
   ↓
Security
   ↓
Auditing
```

**Happy Linux Administration! 🐧**

> _"In Linux, access is not a privilege you casually give away; it is a responsibility you carefully control."_
