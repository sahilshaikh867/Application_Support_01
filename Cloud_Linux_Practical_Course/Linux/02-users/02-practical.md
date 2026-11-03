# Linux Users — Practical Lab

## Goal

Create, inspect, modify and safely remove test users.

## Step 1 — Check Current User

```bash
whoami
id
```

## Step 2 — Create Users

```bash
sudo useradd -m -s /bin/bash appuser
sudo useradd -m -s /bin/bash testuser
sudo passwd appuser
sudo passwd testuser
```

## Step 3 — Inspect

```bash
id appuser
id testuser
getent passwd appuser
```

## Step 4 — Test Login

From another terminal:

```bash
ssh appuser@SERVER_IP
```

## Step 5 — Modify Shell

```bash
sudo usermod -s /bin/bash appuser
```

## Step 6 — Lock and Unlock

```bash
sudo passwd -l testuser
sudo passwd -u testuser
```

## Step 7 — Expiration Information

```bash
sudo chage -l appuser
```

## Step 8 — Cleanup

```bash
sudo userdel -r appuser
sudo userdel -r testuser
```

## Verification

Explain:
- Where users are listed.
- Where password hashes are protected.
- Difference between `useradd` and `passwd`.
- Why test users should be used instead of modifying production accounts.
