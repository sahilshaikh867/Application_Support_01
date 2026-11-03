# Linux Permissions — Notes

Linux permissions control access to files and directories.

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
-    file type
rwx  owner
r-x  group
r--  others
```

## Permission Values

```text
r = 4
w = 2
x = 1
```

Common modes:

```text
755 = rwxr-xr-x
644 = rw-r--r--
700 = rwx------
600 = rw-------
```

## Commands

```bash
chmod
chown
chgrp
umask
```

Example:

```bash
chmod 640 file.txt
sudo chown appuser:developers file.txt
```

## Directory Permissions

For directories:
- `r` = list entries
- `w` = create/delete/rename entries
- `x` = enter/traverse directory

This is a common source of confusion.

## Special Permissions

- setuid
- setgid
- sticky bit

Example sticky directory:

```text
/tmp
```

The sticky bit restricts deletion of files to appropriate owners/privileged users.
