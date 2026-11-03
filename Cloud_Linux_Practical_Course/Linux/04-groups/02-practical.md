# Linux Groups — Practical Lab

## Goal

Create a team-style access model.

## Step 1

Create users:

```bash
sudo useradd -m -s /bin/bash dev1
sudo useradd -m -s /bin/bash dev2
sudo passwd dev1
sudo passwd dev2
```

Create group:

```bash
sudo groupadd developers
```

Add members:

```bash
sudo usermod -aG developers dev1
sudo usermod -aG developers dev2
```

Verify:

```bash
id dev1
id dev2
getent group developers
```

## Step 2 — Shared Directory

```bash
sudo mkdir -p /opt/project
sudo chown root:developers /opt/project
sudo chmod 2770 /opt/project
```

The `2` in `2770` sets the setgid bit so newly created files inherit the directory's group.

## Step 3 — Test

Switch users:

```bash
sudo -iu dev1
```

Create:

```bash
touch /opt/project/dev1.txt
exit
```

Then:

```bash
sudo -iu dev2
ls -l /opt/project
```

## Cleanup

```bash
sudo rm -rf /opt/project
sudo userdel -r dev1
sudo userdel -r dev2
sudo groupdel developers
```
