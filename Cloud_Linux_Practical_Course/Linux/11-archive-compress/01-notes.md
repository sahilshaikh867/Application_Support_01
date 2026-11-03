# Linux Archive & Compression — Notes

Archiving combines multiple files into one archive. Compression reduces the amount of storage needed.

## tar

Create archive:

```bash
tar -cf backup.tar directory/
```

Create gzip-compressed archive:

```bash
tar -czf backup.tar.gz directory/
```

List:

```bash
tar -tf backup.tar.gz
```

Extract:

```bash
tar -xzf backup.tar.gz
```

## gzip

Compress a file:

```bash
gzip file.txt
```

Decompress:

```bash
gunzip file.txt.gz
```

## zip

```bash
zip -r backup.zip directory/
unzip backup.zip
```

## Important Distinction

`tar` primarily archives.

`gzip` primarily compresses.

`tar.gz` commonly means:
1. Files are archived using tar.
2. The resulting archive is compressed with gzip.

## Backup Thinking

A backup is not automatically useful just because an archive exists.

A good backup process considers:
- Naming
- Location
- Retention
- Integrity
- Encryption where required
- Restoration testing
