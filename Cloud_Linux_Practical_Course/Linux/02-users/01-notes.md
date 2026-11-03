# Linux Users — Notes

Linux is a multi-user operating system. Different users can have different identities, permissions and resource access.

## Important Files

```text
/etc/passwd
/etc/shadow
/etc/group
```

Do not edit `/etc/shadow` manually.

## Identity Commands

```bash
whoami
id
who
w
```

## User Creation

```bash
sudo useradd -m -s /bin/bash trainee
sudo passwd trainee
```

On Ubuntu, `adduser` is also commonly used for interactive user creation:

```bash
sudo adduser trainee
```

## Modify User

```bash
sudo usermod -s /bin/bash trainee
sudo usermod -aG sudo trainee
```

Be careful with `usermod -aG`; omitting `-a` can replace supplementary group memberships.

## Delete User

```bash
sudo userdel -r trainee
```

Use `-r` only when you intentionally want the user's home directory and related local files removed.

## User Lifecycle

A practical administrator thinks about:
- Creation
- Authentication
- Group membership
- Permissions
- Password policy
- Access review
- Deactivation
- Removal
