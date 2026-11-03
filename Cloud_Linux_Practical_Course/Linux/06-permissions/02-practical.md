# Linux Permissions — Practical Lab

## Goal

Understand ownership and permission modes.

## Step 1

```bash
mkdir -p ~/linux-lab/permissions
cd ~/linux-lab/permissions
echo "secret training data" > secret.txt
```

## Step 2 — Inspect

```bash
ls -l secret.txt
```

## Step 3 — Change Permissions

```bash
chmod 600 secret.txt
ls -l secret.txt
```

## Step 4 — Experiment

Try:

```bash
chmod 644 secret.txt
chmod 640 secret.txt
chmod 755 secret.txt
```

Explain the difference before moving on.

## Step 5 — Ownership

Create a test user if needed:

```bash
sudo useradd -m permissionuser
sudo passwd permissionuser
```

Change ownership:

```bash
sudo chown permissionuser:permissionuser secret.txt
```

Verify:

```bash
ls -l secret.txt
```

## Step 6 — Directory Permission Experiment

```bash
mkdir shared
chmod 750 shared
ls -ld shared
```

## Questions

1. Why is `755` common for directories?
2. Why is `777` usually a bad default?
3. What does `x` mean on a directory?

## Cleanup

```bash
cd ~
rm -rf ~/linux-lab/permissions
sudo userdel -r permissionuser
```
