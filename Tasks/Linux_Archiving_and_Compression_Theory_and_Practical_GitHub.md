# Linux Archiving and Compression --- Theory & Practical

**Level:** Absolute Beginner to Industry Practice\
**Environment:** Ubuntu/Debian and RHEL-based Linux\
**Module:** Linux OS Foundation --- Final Topic

## 1. Learning objectives

After completing this guide, you should be able to:

-   Explain archiving and compression in simple language.
-   Create, inspect, extract, and test archives.
-   Use `tar`, `gzip`, `bzip2`, `xz`, `zip`, and `unzip`.
-   Exclude files from archives.
-   Create timestamped application backups.
-   Automate backups with a Bash script and cron.
-   Troubleshoot common errors and follow safe backup practices.

## 2. Why do we need archiving and compression?

Imagine that an application server contains hundreds of files. You need
to move the application to another server or keep a copy before a
deployment. Sending each file separately is inconvenient.

**Archiving** combines multiple files and directories into one package.
**Compression** reduces the storage space required by data.

Think of packing clothes into a suitcase: putting many items into one
suitcase is like archiving; vacuum-packing them to take up less space is
like compression.

  Archiving                          Compression
  ---------------------------------- -------------------------------------
  Combines files into one package    Reduces data size
  Does not necessarily reduce size   Usually reduces size
  Common tool: `tar`                 Common tools: `gzip`, `bzip2`, `xz`

A `.tar` file is an archive. A `.tar.gz` file is a tar archive
compressed with gzip. A `.zip` file is an archive format that usually
supports compression.

## 3. Meet the tools

### tar

`tar` means Tape Archive. It is commonly used to package directories and
create backups.

``` bash
tar --version
```

### gzip

Compresses a file and normally creates a `.gz` file:

``` bash
gzip report.txt
```

Keep the original file:

``` bash
gzip -k report.txt
```

Decompress:

``` bash
gunzip report.txt.gz
# or
gzip -d report.txt.gz
```

### bzip2

``` bash
bzip2 -k report.txt
bunzip2 report.txt.bz2
```

### xz

``` bash
xz -k report.txt
unxz report.txt.xz
```

`xz` can produce a smaller result for some data, but may take more CPU
time and memory. Results depend on the data.

### zip and unzip

Ubuntu/Debian:

``` bash
sudo apt update
sudo apt install zip unzip
```

RHEL-based systems:

``` bash
sudo dnf install zip unzip
```

Create and extract a ZIP archive:

``` bash
zip -r project.zip project/
unzip -l project.zip
unzip project.zip -d restore/
```

## 4. Understand tar options

General syntax:

``` bash
tar [options] archive-name source-files-or-directories
```

  Option        Meaning
  ------------- ---------------------------------------------------
  `-c`          Create archive
  `-x`          Extract archive
  `-t`          List archive contents
  `-v`          Verbose output; show filenames
  `-f`          Specify archive filename
  `-z`          Use gzip
  `-j`          Use bzip2
  `-J`          Use xz
  `-C`          Change directory before processing
  `-r`          Append to an uncompressed tar archive
  `-u`          Append newer files to an uncompressed tar archive
  `--exclude`   Exclude matching paths

### Create a plain tar archive

``` bash
tar -cvf project.tar project/
```

-   `-c` creates an archive.
-   `-v` displays files as they are processed.
-   `-f` identifies the archive filename.
-   `project.tar` is the output file.
-   `project/` is the source directory.

### List without extracting

``` bash
tar -tvf project.tar
```

### Extract

``` bash
tar -xvf project.tar
```

Extract into a chosen directory (create it first):

``` bash
mkdir -p /tmp/restore
tar -xvf project.tar -C /tmp/restore
```

### Compressed tar formats

  -----------------------------------------------------------------------------------------
  Format                  Create                               Extract
  ----------------------- ------------------------------------ ----------------------------
  `.tar.gz`               `tar -czvf backup.tar.gz folder/`    `tar -xzvf backup.tar.gz`

  `.tar.bz2`              `tar -cjvf backup.tar.bz2 folder/`   `tar -xjvf backup.tar.bz2`

  `.tar.xz`               `tar -cJvf backup.tar.xz folder/`    `tar -xJvf backup.tar.xz`
  -----------------------------------------------------------------------------------------

To list a gzip-compressed tar archive:

``` bash
tar -tzvf backup.tar.gz
```

## 5. Practical Lab 1 --- Your first archive

**Objective:** Create sample files, archive them, inspect the archive,
and restore them.

### Step 1: Create a workspace

``` bash
mkdir -p ~/archive-lab/project/{app,config,logs,uploads}
cd ~/archive-lab
```

### Step 2: Create sample files

``` bash
echo "Application started" > project/app/app.log
echo "PORT=3000" > project/config/app.conf
echo "Database connection successful" > project/logs/database.log
echo "Sample user upload" > project/uploads/readme.txt
```

The `>` operator writes text to a file. If the file exists, its previous
content is replaced.

### Step 3: Inspect the directory

``` bash
find project -type f -print
```

### Step 4: Create and inspect the archive

``` bash
tar -cvf project-backup.tar project/
ls -lh project-backup.tar
file project-backup.tar
tar -tvf project-backup.tar
```

### Step 5: Restore to another directory

``` bash
mkdir -p restore
tar -xvf project-backup.tar -C restore
find restore -type f -print
```

Check a restored file:

``` bash
cat restore/project/config/app.conf
```

**Expected result:** The archive contains the project files, and the
restored directory contains copies of them.

## 6. Practical Lab 2 --- Compare compression formats

**Objective:** Compare gzip, bzip2, and xz using the same sample data.

``` bash
cd ~/archive-lab
mkdir -p compression-lab
yes "Linux archiving and compression practice data" | head -n 100000 > compression-lab/sample.txt
```

Check the original size:

``` bash
ls -lh compression-lab/sample.txt
du -h compression-lab/sample.txt
```

Create compressed copies while keeping the original:

``` bash
gzip -k compression-lab/sample.txt
bzip2 -k compression-lab/sample.txt
xz -k compression-lab/sample.txt
```

Compare sizes:

``` bash
ls -lh compression-lab/
```

You should see `.gz`, `.bz2`, and `.xz` versions. The exact sizes depend
on the data and software versions. A higher compression ratio is not
always worth slower compression or greater CPU and memory use.

Test and decompress a copy:

``` bash
gzip -t compression-lab/sample.txt.gz
cp compression-lab/sample.txt.gz compression-lab/test.txt.gz
gunzip compression-lab/test.txt.gz
cmp compression-lab/sample.txt compression-lab/test.txt
```

If `cmp` prints nothing and exits successfully, the files are identical.

## 7. Practical Lab 3 --- ZIP and UNZIP

``` bash
cd ~/archive-lab
zip -r project.zip project/
unzip -l project.zip
mkdir -p zip-restore
unzip project.zip -d zip-restore/
find zip-restore -type f -print
cat zip-restore/project/config/app.conf
```

`zip -r` includes a directory recursively. `unzip -l` lists the archive.
`-d` chooses the extraction destination.

## 8. Exclude files and directories

Application folders may contain temporary files, caches, or logs that
should not be included in every backup.

Exclude one directory:

``` bash
tar -czvf project-clean.tar.gz \
  --exclude='project/logs' \
  project/
```

Exclude several paths or patterns:

``` bash
tar -czvf project-clean.tar.gz \
  --exclude='project/logs' \
  --exclude='project/cache' \
  --exclude='*.tmp' \
  project/
```

Inspect the result:

``` bash
tar -tzf project-clean.tar.gz
```

You can also archive selected files only:

``` bash
tar -czvf selected-files.tar.gz \
  project/config/app.conf \
  project/app/app.log
```

Do not automatically exclude logs, uploads, or caches. Decide based on
the application's recovery requirements.

## 9. Append and update archives

For an **uncompressed tar archive**, append a file:

``` bash
echo "New report" > new-report.txt
tar -rvf project-backup.tar new-report.txt
tar -tvf project-backup.tar
```

Update newer files in an uncompressed tar archive:

``` bash
tar -uvf project-backup.tar project/
```

Traditional append and update operations are for uncompressed tar
archives. For `.tar.gz`, `.tar.bz2`, and `.tar.xz`, create a new archive
instead.

## 10. Inspect and test archives

Identify a file:

``` bash
file backup.tar.gz
```

List archive contents:

``` bash
tar -tzf backup.tar.gz
unzip -l backup.zip
```

Test gzip integrity:

``` bash
gzip -t backup.tar.gz
```

Test ZIP integrity:

``` bash
unzip -t backup.zip
```

Check the previous command's exit status:

``` bash
echo $?
```

An exit status of `0` normally means success. A non-zero value indicates
an error. Always read the command output too.

## 11. Safe extraction

Never extract an archive from an unknown source directly into a
sensitive system directory.

A safer workflow is:

1.  List the archive contents.
2.  Create a new, empty destination.
3.  Extract into that destination.
4.  Inspect the restored files.
5.  Move approved files to their final location only after review.

Example:

``` bash
mkdir -p ~/safe-restore
tar -tzf downloaded-backup.tar.gz
tar -xzf downloaded-backup.tar.gz -C ~/safe-restore
find ~/safe-restore -maxdepth 4 -print
```

Avoid extracting as root unless it is genuinely required. Privileged
extraction can overwrite or create files in sensitive locations.

## 12. Useful supporting commands

  Command                 Purpose
  ----------------------- ----------------------------
  `ls -lh`                Show readable file sizes
  `du -sh folder/`        Show total directory size
  `df -h`                 Show filesystem free space
  `file filename`         Identify file format
  `stat filename`         Show file metadata
  `find folder -type f`   List files recursively
  `cmp file1 file2`       Compare files byte by byte
  `sha256sum filename`    Calculate SHA-256 checksum

Generate and verify a checksum:

``` bash
sha256sum backup.tar.gz > backup.tar.gz.sha256
sha256sum -c backup.tar.gz.sha256
```

A checksum can detect accidental changes. A checksum stored beside an
archive does not prove who created it; an attacker who can replace both
files can replace the checksum too. Use a trusted signature or
separately trusted checksum source when authenticity matters.

## 13. Industry scenario --- Back up an application

Assume an application lives at `/opt/shop-app/`:

``` text
/opt/shop-app/
├── app/
├── config/
├── uploads/
├── logs/
└── cache/
```

Check its size and contents:

``` bash
sudo ls -lah /opt/shop-app
sudo du -sh /opt/shop-app
```

Create a backup directory:

``` bash
sudo mkdir -p /var/backups/shop-app
```

Create a timestamped archive:

``` bash
sudo tar -czvf \
  "/var/backups/shop-app/shop-app-$(date +%Y%m%d-%H%M%S).tar.gz" \
  -C /opt shop-app
```

`date` creates a timestamp such as `20261118-184500`. `$(...)` runs the
date command and inserts the result into the filename. `-C /opt` tells
tar to work from `/opt`, and `shop-app` is the directory being archived.

Check the backup:

``` bash
sudo ls -lh /var/backups/shop-app/
```

Inspect the archive:

``` bash
sudo tar -tzf /var/backups/shop-app/ACTUAL_BACKUP_NAME.tar.gz
```

Replace `ACTUAL_BACKUP_NAME` with the actual filename.

Restore into a test directory:

``` bash
sudo mkdir -p /tmp/shop-app-restore
sudo tar -xzvf /var/backups/shop-app/ACTUAL_BACKUP_NAME.tar.gz \
  -C /tmp/shop-app-restore
```

A successful archive command does not prove that an application can be
restored. Test the application after restoration. Databases and actively
changing files may require application-aware backup procedures.

## 14. Create a reusable backup script

Create the script:

``` bash
sudo nano /usr/local/bin/shop-app-backup.sh
```

Paste:

``` bash
#!/usr/bin/env bash
set -Eeuo pipefail

SOURCE_DIR="/opt/shop-app"
BACKUP_DIR="/var/backups/shop-app"
TIMESTAMP="$(date +%Y%m%d-%H%M%S)"
ARCHIVE="${BACKUP_DIR}/shop-app-${TIMESTAMP}.tar.gz"
LOG_FILE="${BACKUP_DIR}/backup.log"

mkdir -p "$BACKUP_DIR"

if [[ ! -d "$SOURCE_DIR" ]]; then
    echo "ERROR: Source directory does not exist: $SOURCE_DIR" | tee -a "$LOG_FILE"
    exit 1
fi

echo "Starting backup: $(date)" | tee -a "$LOG_FILE"

if tar -czf "$ARCHIVE" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"; then
    if gzip -t "$ARCHIVE"; then
        echo "SUCCESS: Backup created and tested: $ARCHIVE" | tee -a "$LOG_FILE"
    else
        echo "ERROR: Archive integrity test failed: $ARCHIVE" | tee -a "$LOG_FILE"
        exit 1
    fi
else
    echo "ERROR: Backup command failed" | tee -a "$LOG_FILE"
    exit 1
fi

echo "Backup size: $(du -h "$ARCHIVE" | awk '{print $1}')" | tee -a "$LOG_FILE"
```

Make it executable and run it:

``` bash
sudo chmod 750 /usr/local/bin/shop-app-backup.sh
sudo /usr/local/bin/shop-app-backup.sh
```

Check the result:

``` bash
sudo ls -lh /var/backups/shop-app/
sudo tail -n 20 /var/backups/shop-app/backup.log
```

### Understand the script

-   `#!/usr/bin/env bash` selects Bash.
-   `set -Eeuo pipefail` helps the script stop on many errors instead of
    silently continuing.
-   Quoted variables protect paths containing spaces.
-   The script checks that the source directory exists.
-   It tests the gzip stream after creating the archive.
-   It records progress and errors in a log.

This is a learning script, not a complete enterprise backup platform. It
does not coordinate database writes, encrypt backups, copy them
off-site, enforce retention, or send alerts.

## 15. Schedule the backup using cron

Open the current user's crontab:

``` bash
crontab -e
```

If the script needs root privileges, edit root's crontab:

``` bash
sudo crontab -e
```

Run the backup daily at 2:00 AM:

``` cron
0 2 * * * /usr/local/bin/shop-app-backup.sh
```

Cron format:

``` text
minute hour day-of-month month day-of-week command
```

  Schedule                          Expression
  --------------------------------- ---------------
  Every 5 minutes                   `*/5 * * * *`
  Every hour                        `0 * * * *`
  Daily at 2 AM                     `0 2 * * *`
  Every Sunday at 3 AM              `0 3 * * 0`
  First day of each month at 1 AM   `0 1 1 * *`

View the crontab:

``` bash
crontab -l
sudo crontab -l
```

Service names differ by distribution.

Ubuntu/Debian:

``` bash
sudo systemctl status cron
sudo systemctl enable --now cron
```

RHEL-based systems:

``` bash
sudo systemctl status crond
sudo systemctl enable --now crond
```

Run and test the script manually before scheduling it.

## 16. Troubleshooting common errors

### `tar: command not found`

Ubuntu/Debian:

``` bash
sudo apt update
sudo apt install tar
```

RHEL-based systems:

``` bash
sudo dnf install tar
```

### `Permission denied`

Check source and destination permissions:

``` bash
ls -ld /path/to/source
ls -ld /path/to/backup-directory
```

Use `sudo` only when elevated privileges are required. Do not make
everything world-writable to bypass permission problems.

### `No such file or directory`

Check your current directory and path:

``` bash
pwd
ls -lah
```

Linux paths are case-sensitive: `Backup` and `backup` are different.

### `gzip: stdin: not in gzip format`

The file may not be gzip-compressed, may have the wrong extension, or
may be damaged.

``` bash
file backup-file
```

Do not choose a decompression method based only on the filename.

### Archive extraction fails

``` bash
tar -tf backup.tar
tar -tzf backup.tar.gz
unzip -t backup.zip
df -h
```

Check archive contents, free disk space, and destination permissions.

### Backup is unexpectedly large

Inspect directory sizes:

``` bash
du -sh /path/to/source
du -ah /path/to/source | sort -h | tail -n 20
```

Logs, caches, generated files, and already-compressed media may
contribute to the size. JPEG, MP4, and ZIP files often do not compress
much further.

## 17. Industry best practices

1.  Know exactly which paths are included and excluded.
2.  Use timestamped filenames to avoid accidental overwrites.
3.  Test the archive after creation.
4.  Test restoration regularly.
5.  Keep another copy on separate storage or off-site.
6.  Protect confidential backups with suitable access controls and
    encryption.
7.  Define retention rules for daily, weekly, and monthly backups.
8.  Monitor scheduled jobs and alert someone when they fail.
9.  Use application-aware procedures for databases and changing data.
10. Remember: compression is not encryption. A `.tar.gz` file is not
    password-protected merely because it is compressed.

A useful starting principle is **3-2-1 backup**:

-   Keep at least 3 copies of important data.
-   Use at least 2 different types of storage.
-   Keep at least 1 copy off-site.

Adapt this to the organisation's recovery time, recovery point,
security, and compliance needs.

## 18. Practice exercises

### Beginner

1.  Create `my-first-backup` with `documents`, `images`, and `logs`
    subdirectories.
2.  Add a text file to each directory.
3.  Create a plain tar archive.
4.  List its contents without extracting.
5.  Extract it into a new directory.
6.  Create a `.tar.gz` archive.
7.  Compare the sizes of the two archives.

### Intermediate

1.  Create a ZIP archive and extract it elsewhere.
2.  Create a `.tar.gz` archive excluding the logs directory.
3.  Compress a text file using gzip while keeping the original.
4.  Test a gzip archive using `gzip -t`.
5.  Calculate a SHA-256 checksum.
6.  Restore an archive and compare the files.

### Industry-level

1.  Create a backup directory under `/var/backups`.
2.  Write a script that creates timestamped archives.
3.  Log success and failure.
4.  Test the archive after creation.
5.  Schedule the script with cron after manual testing.
6.  Restore a backup into a temporary directory.
7.  Write a short report describing what was included, excluded, and
    verified.

## 19. Interview questions

**1. What is archiving?**\
Combining multiple files and directories into one package. `tar` is a
common Linux archiving tool.

**2. What is compression?**\
Reducing the amount of space required to store data. Common tools
include gzip, bzip2, and xz.

**3. What is the difference between `.tar` and `.tar.gz`?**\
`.tar` packages files. `.tar.gz` packages them and then compresses the
archive using gzip.

**4. Does tar compress by default?**\
No. Use `-z`, `-j`, or `-J` to select gzip, bzip2, or xz.

**5. How do you list a `.tar.gz` archive?**

``` bash
tar -tzf backup.tar.gz
```

**6. How do you extract into a chosen directory?**

``` bash
tar -xzf backup.tar.gz -C /path/to/destination
```

The destination should exist first.

**7. What is the difference between gzip and ZIP?**\
Gzip normally compresses a single file or stream. ZIP is an archive
format that can package multiple files and directories.

**8. Which format should you choose?**\
Choose based on compatibility, compression ratio, speed, CPU and memory
use, and the receiving system.

**9. How do you test a gzip file?**

``` bash
gzip -t filename.gz
```

**10. Why test backups?**\
A backup is only useful if it can be restored. Integrity checks and
restore tests find problems before recovery is needed.

**11. Is a compressed archive encrypted?**\
No. Compression and encryption solve different problems.

**12. Why use timestamps in backup filenames?**\
They identify when the backup was created and reduce the chance of
overwriting an earlier copy.

## 20. Final challenge --- Application backup and recovery

You are a Linux administrator for a small e-commerce company. The
application team needs a backup before a release.

Complete these tasks:

1.  Create a sample application directory with `app`, `config`,
    `uploads`, `logs`, and `cache`.
2.  Add sample files.
3.  Decide and document which paths belong in the release backup.
4.  Create a timestamped `.tar.gz` archive.
5.  List the archive contents.
6.  Test the gzip stream.
7.  Generate a SHA-256 checksum.
8.  Restore the archive into a separate directory.
9.  Compare original and restored files.
10. Create a Bash script to repeat the backup.
11. Schedule it with cron only after manual testing.
12. Write an incident note explaining how you would recover if the
    original directory were deleted.

Your submission should include commands, explanations, archive name and
size, integrity-test result, checksum, restore evidence, included and
excluded paths, and problems encountered.

## 21. Linux OS Foundation --- Module completion

You have completed **Archiving and Compression**, the final topic in
this Linux OS Foundation sequence.

Suggested next learning path:

1.  Shell scripting
2.  Git and GitHub
3.  Networking fundamentals
4.  Docker
5.  CI/CD
6.  AWS and cloud fundamentals
7.  Terraform
8.  Kubernetes
9.  Monitoring and observability

Do not aim only to memorise commands. Understand what problem each
command solves, what it changes, and how to verify the result.

## 22. Quick command cheat sheet

``` bash
# Create a plain tar archive
tar -cvf backup.tar folder/

# List a tar archive
tar -tvf backup.tar

# Extract a tar archive
tar -xvf backup.tar

# Create a gzip-compressed tar archive
tar -czvf backup.tar.gz folder/

# List a gzip-compressed tar archive
tar -tzf backup.tar.gz

# Extract a gzip-compressed tar archive
tar -xzf backup.tar.gz

# Create and extract bzip2 archive
tar -cjvf backup.tar.bz2 folder/
tar -xjf backup.tar.bz2

# Create and extract xz archive
tar -cJvf backup.tar.xz folder/
tar -xJf backup.tar.xz

# Compress a file and keep the original
gzip -k file.txt

# Decompress gzip
gunzip file.txt.gz

# Create, list, and extract ZIP
zip -r backup.zip folder/
unzip -l backup.zip
unzip backup.zip -d restore/

# Test gzip and ZIP
gzip -t backup.tar.gz
unzip -t backup.zip

# Inspect size and file type
ls -lh backup.tar.gz
file backup.tar.gz

# Generate and verify checksum
sha256sum backup.tar.gz
sha256sum -c backup.tar.gz.sha256
```

------------------------------------------------------------------------

**End of Module --- Linux Archiving and Compression**
