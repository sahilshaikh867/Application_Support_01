# Linux Directories & File Management — Notes

## Important Directories

```text
/          Root of filesystem
/home      User home directories
/etc       Configuration
/var       Variable data such as logs
/tmp       Temporary files
/usr       User-space programs and data
/opt       Optional application software
/dev       Device files
/proc      Process/kernel information
```

## Common Commands

```bash
mkdir
touch
cp
mv
rm
find
file
stat
```

## Copy

```bash
cp source.txt backup.txt
cp -r app/ app-backup/
```

## Move/Rename

```bash
mv old.txt new.txt
```

## Delete

```bash
rm file.txt
rm -r directory
```

Use `rm -rf` carefully. It can remove data without an interactive recovery path.

## Find

```bash
find /var/log -name "*.log"
find ~/linux-lab -type f
```

## File Metadata

```bash
stat file.txt
```

Understand:
- Size
- Permissions
- Owner
- Group
- Timestamps
