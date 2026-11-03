# Linux Basics — Practical Lab

## Goal

Learn to navigate Linux, inspect the system, work with files and use basic administration commands.

## Step 1 — Connect

```bash
ssh user@SERVER_IP
```

## Step 2 — Identify the System

Run:

```bash
whoami
hostname
pwd
uname -a
cat /etc/os-release
```

Record the output.

## Step 3 — Navigation

```bash
cd /
ls
cd /tmp
pwd
cd ~
pwd
```

## Step 4 — Create a Lab Workspace

```bash
mkdir -p ~/linux-lab/basics
cd ~/linux-lab/basics
```

Create files:

```bash
touch file1.txt file2.txt
echo "Linux practical" > file1.txt
echo "Second line" >> file1.txt
```

Verify:

```bash
cat file1.txt
ls -l
```

## Step 5 — Pipes and Search

```bash
cat /etc/passwd | head
grep "root" /etc/passwd
```

## Step 6 — Package Exercise

Check whether Nginx is installed:

```bash
dpkg -l | grep nginx
```

If it is appropriate for your lab:

```bash
sudo apt update
sudo apt install nginx -y
```

## Step 7 — Service

```bash
sudo systemctl status nginx
```

## Step 8 — Verification Questions

Answer without looking up:

1. What is the difference between `/` and `~`?
2. What does `sudo` do?
3. What does `>>` do?
4. What does a pipe do?
5. Where can you read OS information?

## Cleanup

```bash
rm -rf ~/linux-lab/basics
```

Only remove the directory you created for this lab.
