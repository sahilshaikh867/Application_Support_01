# Linux Groups — Notes

Groups make permission management easier by allowing multiple users to share access.

## Commands

```bash
groups
id
getent group
groupadd
groupmod
groupdel
usermod -aG
gpasswd
```

Create:

```bash
sudo groupadd developers
```

Add user:

```bash
sudo usermod -aG developers appuser
```

Check:

```bash
id appuser
groups appuser
```

A user may have:
- One primary group
- Multiple supplementary groups

## Why Groups Matter

Instead of granting access separately to 20 users, create a group and manage permissions for the group.

This is easier to audit and maintain.
