# Linux Task Scheduling with Cron and Crontab

## Complete Theory, Commands, Step-by-Step Practicals, and Troubleshooting

**Level:** Absolute Beginner to Industry Practice\
**Platforms:** Ubuntu/Debian and RHEL-based Linux (Rocky Linux,
AlmaLinux, RHEL, Amazon Linux)\
**Module:** Linux Administration / Linux OS Foundation\
**Goal:** Learn how to schedule, create, view, update, disable, delete,
test, and troubleshoot automated tasks using cron.

------------------------------------------------------------------------

# 1. Learning objectives

By the end of this module, you should be able to:

-   Explain task scheduling and why Linux administrators use it.
-   Understand cron, the cron daemon, and crontab.
-   Check whether the cron service is installed and running.
-   Read and write crontab schedules.
-   Create one-time-in-the-minute, hourly, daily, weekly, and monthly
    recurring jobs.
-   Add, update, comment out, disable, and remove scheduled jobs.
-   Schedule shell scripts and capture their output in log files.
-   Use user-specific and system-wide cron configurations.
-   Troubleshoot jobs that do not run.
-   Build practical automation for backups, health checks, reports, and
    cleanup.

> **Important:** Cron schedules recurring tasks. It is not the same as
> `at`, which is commonly used to schedule a one-time command.

------------------------------------------------------------------------

# 2. What is task scheduling?

Task scheduling means asking the operating system to execute a command
or script automatically at a defined time or interval.

Imagine that you manage a Linux server for an online shopping
application. Every day, you may need to:

-   Back up application files.
-   Check disk usage.
-   Generate a server resource report.
-   Clean temporary files.
-   Check whether an application process is running.
-   Rotate or archive old logs.

Doing all of this manually every day wastes time and can lead to missed
tasks.

Task scheduling helps administrators automate repeatable work.

## Everyday example

Think of an alarm clock:

-   You choose a time.
-   You choose when it should repeat.
-   The clock alerts you automatically.

Cron works in a similar way, but instead of ringing an alarm, it runs a
Linux command or script.

## Why do companies use task scheduling?

  -----------------------------------------------------------------------
  Requirement                         Example
  ----------------------------------- -----------------------------------
  Backup                              Back up application files every
                                      night

  Monitoring                          Check disk usage every 10 minutes

  Cleanup                             Remove approved temporary files
                                      every Sunday

  Reporting                           Generate a report every morning

  Maintenance                         Run a maintenance script during a
                                      quiet period

  Automation                          Execute a recurring shell script
                                      without manual intervention
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 3. What are Cron, Crontab, and the Cron Daemon?

These terms are related, but they do not mean exactly the same thing.

## 3.1 Cron

Cron is the time-based job scheduling system commonly used on Linux and
Unix-like operating systems.

It checks configured schedules and starts commands when their scheduled
time arrives.

## 3.2 Crontab

`crontab` is the command used to manage a user's cron schedule.

You use it to:

-   Edit scheduled jobs.
-   List existing jobs.
-   Remove a user's crontab.
-   Manage another user's crontab when you have sufficient privileges.

## 3.3 Cron daemon

A daemon is a background process that provides a service.

The cron daemon runs in the background and checks schedules. Its service
name differs across Linux distributions:

  Distribution family                       Common service name
  ----------------------------------------- ---------------------
  Ubuntu / Debian                           `cron`
  RHEL / Rocky / AlmaLinux / Amazon Linux   `crond`

## How cron works

``` text
Administrator
     |
     | creates or edits a schedule
     v
  Crontab
     |
     | stores the job definition
     v
 Cron daemon (cron / crond)
     |
     | checks the current time
     v
 Scheduled time matches
     |
     v
 Command or script runs
     |
     v
 Output / logs / result
```

Cron does not automatically know what you want to do. You must define
the command and schedule.

------------------------------------------------------------------------

# 4. Check your Linux distribution

Before using cron, identify your Linux distribution.

``` bash
cat /etc/os-release
```

This displays operating system information such as the distribution name
and version.

Check whether the crontab command is available:

``` bash
crontab -V
```

Some implementations may not support `-V`. If it is unsupported, use:

``` bash
command -v crontab
```

If the command is not found, install the appropriate cron package.

## 4.1 Install cron on Ubuntu / Debian

``` bash
sudo apt update
sudo apt install cron
```

Enable and start the service:

``` bash
sudo systemctl enable --now cron
```

Check its status:

``` bash
systemctl status cron
```

## 4.2 Install cron on RHEL-based distributions

On many RHEL-based systems, including Rocky Linux, AlmaLinux, and Amazon
Linux:

``` bash
sudo dnf install cronie
```

Enable and start the service:

``` bash
sudo systemctl enable --now crond
```

Check its status:

``` bash
systemctl status crond
```

Older systems may use `yum` instead of `dnf`:

``` bash
sudo yum install cronie
```

> **Remember:** Do not copy a service name from another distribution
> without checking. Ubuntu commonly uses `cron`; RHEL-based systems
> commonly use `crond`.

------------------------------------------------------------------------

# 5. Important crontab commands

These are the commands you will use most often.

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `crontab -e`                        Edit the current user's crontab

  `crontab -l`                        List the current user's cron jobs

  `crontab -r`                        Remove the current user's entire
                                      crontab

  `crontab -u username -l`            List another user's crontab
                                      (requires suitable privileges)

  `sudo crontab -e`                   Edit root's crontab

  `sudo crontab -l`                   List root's crontab

  `sudo crontab -u username -e`       Edit another user's crontab
                                      (requires root privileges)
  -----------------------------------------------------------------------

## 5.1 Open the crontab editor

``` bash
crontab -e
```

The first time, Linux may ask you to choose a text editor. Select one
you know. Nano is often easier for beginners.

A crontab file may initially be empty or contain comments.

## 5.2 List scheduled jobs

``` bash
crontab -l
```

This displays jobs configured for the current user.

To list root's jobs:

``` bash
sudo crontab -l
```

To list another user's jobs:

``` bash
sudo crontab -u username -l
```

Replace `username` with the real Linux account name.

## 5.3 Remove all jobs for the current user

``` bash
crontab -r
```

**Warning:** This removes the entire current user's crontab, not just
one selected line. It is easy to delete important schedules
accidentally.

A safer workflow is to edit the crontab and remove only the required
line:

``` bash
crontab -e
```

If you really need to remove the whole crontab, first save a copy:

``` bash
crontab -l > "$HOME/crontab-backup.txt"
```

Then remove it:

``` bash
crontab -r
```

Restore the saved copy if needed:

``` bash
crontab "$HOME/crontab-backup.txt"
```

Do not use `crontab -r` as the normal way to modify one job.

------------------------------------------------------------------------

# 6. Understand the crontab syntax

A standard user crontab entry has five time fields followed by the
command.

``` text
MINUTE HOUR DAY-OF-MONTH MONTH DAY-OF-WEEK COMMAND
```

Example:

``` cron
30 2 * * * /home/user/scripts/backup.sh
```

This runs the script every day at 2:30 AM, using the cron daemon's local
time zone.

## 6.1 Understand each field

  Field          Allowed values   Meaning
  -------------- ---------------- --------------------------------------------
  Minute         `0-59`           Minute of the hour
  Hour           `0-23`           Hour of the day
  Day of month   `1-31`           Calendar day
  Month          `1-12`           Month
  Day of week    `0-7`            Day of week; Sunday is commonly `0` or `7`

Example:

``` cron
15 10 * * 1 /home/user/scripts/report.sh
```

Explanation:

-   `15` --- at minute 15.
-   `10` --- at 10 AM.
-   `*` --- every day of the month.
-   `*` --- every month.
-   `1` --- Monday on common cron implementations.
-   Final field --- the command to run.

This runs every Monday at 10:15 AM.

> Day-of-week numbering and some advanced syntax can vary between cron
> implementations. The examples here use common Linux cron conventions.

## 6.2 Special characters

  Symbol   Meaning                Example
  -------- ---------------------- ----------------
  `*`      Every possible value   `* * * * *`
  `,`      Specific values        `0 9,17 * * *`
  `-`      Range of values        `0 9-17 * * *`
  `/`      Step interval          `*/5 * * * *`

### Asterisk `*`

``` cron
* * * * * command
```

Runs every minute.

### Comma `,`

``` cron
0 9,17 * * * command
```

Runs at 9:00 AM and 5:00 PM every day.

### Hyphen `-`

``` cron
0 9-17 * * * command
```

Runs at the start of every hour from 9 AM through 5 PM.

### Slash `/`

``` cron
*/5 * * * * command
```

Runs every five minutes.

------------------------------------------------------------------------

# 7. Common cron schedules

  Requirement                           Cron expression
  ------------------------------------- -----------------
  Every minute                          `* * * * *`
  Every 5 minutes                       `*/5 * * * *`
  Every 10 minutes                      `*/10 * * * *`
  Every 15 minutes                      `*/15 * * * *`
  Every 30 minutes                      `*/30 * * * *`
  Every hour                            `0 * * * *`
  Every 2 hours                         `0 */2 * * *`
  Every day at midnight                 `0 0 * * *`
  Every day at 2:30 AM                  `30 2 * * *`
  Every day at 6 PM                     `0 18 * * *`
  Every Monday at 9 AM                  `0 9 * * 1`
  Every Sunday at 3 AM                  `0 3 * * 0`
  First day of each month at midnight   `0 0 1 * *`
  Every weekday at 9 AM                 `0 9 * * 1-5`
  Every Saturday at 11:30 PM            `30 23 * * 6`

Cron uses the server's configured local time zone unless configured
otherwise. Always verify the server's time before scheduling important
jobs:

``` bash
date
timedatectl
```

## 7.1 Special schedule strings

Many Linux cron implementations support these shortcuts:

  -----------------------------------------------------------------------
  Shortcut                            Meaning
  ----------------------------------- -----------------------------------
  `@reboot`                           Run when the cron daemon starts /
                                      system boots, depending on
                                      implementation

  `@yearly` or `@annually`            Once a year

  `@monthly`                          Once a month

  `@weekly`                           Once a week

  `@daily` or `@midnight`             Once a day

  `@hourly`                           Once an hour
  -----------------------------------------------------------------------

Examples:

``` cron
@daily /home/user/scripts/backup.sh
@weekly /home/user/scripts/cleanup.sh
@reboot /home/user/scripts/startup-check.sh
```

`@reboot` is useful for startup tasks, but it does not guarantee that
every network or application dependency is ready when the script starts.
A robust script should handle dependencies and retries.

------------------------------------------------------------------------

# 8. Your first cron job --- run a command every minute

We will create a simple job that records the current date and time in a
file.

## Step 1: Create a practice directory

``` bash
mkdir -p ~/cron-lab/logs
```

## Step 2: Create the output file

``` bash
touch ~/cron-lab/logs/cron-test.log
```

## Step 3: Open your crontab

``` bash
crontab -e
```

## Step 4: Add this line

Use your actual home directory path. The following example assumes the
user's home directory is `/home/sahil`:

``` cron
* * * * * /usr/bin/date >> /home/sahil/cron-lab/logs/cron-test.log 2>&1
```

If your username is not `sahil`, replace `/home/sahil` with the correct
absolute path. You can find your home directory with:

``` bash
echo "$HOME"
```

## Step 5: Save and exit

If you are using Nano:

-   Press `Ctrl + O` to save.
-   Press `Enter` to confirm the filename.
-   Press `Ctrl + X` to exit.

## Step 6: Confirm that the job is saved

``` bash
crontab -l
```

## Step 7: Wait for the next minute and check the output

``` bash
cat ~/cron-lab/logs/cron-test.log
```

You should see date and time entries being added.

## Step 8: Watch the file update

``` bash
tail -f ~/cron-lab/logs/cron-test.log
```

Press `Ctrl + C` to stop watching. This does not stop the cron job.

## Step 9: Remove only the test job

``` bash
crontab -e
```

Delete the line containing the `cron-test.log` command, save, and exit.

Check:

``` bash
crontab -l
```

**Learning point:** A cron job can run successfully even when you are
not logged into the terminal. Its output must be directed to a file or
another destination if you want to review it later.

------------------------------------------------------------------------

# 9. Create, update, disable, and delete cron jobs

This section is especially important. Managing a job is more than
creating it once.

## 9.1 Create a new job

Open the crontab:

``` bash
crontab -e
```

Add:

``` cron
*/5 * * * * /usr/bin/date >> /home/sahil/cron-lab/logs/every-five-minutes.log 2>&1
```

This runs every five minutes.

## 9.2 Update an existing job

Open the same crontab:

``` bash
crontab -e
```

Find the existing line and change it.

For example, change:

``` cron
*/5 * * * * /usr/bin/date >> /home/sahil/cron-lab/logs/every-five-minutes.log 2>&1
```

to:

``` cron
*/10 * * * * /usr/bin/date >> /home/sahil/cron-lab/logs/every-five-minutes.log 2>&1
```

Now it runs every ten minutes.

Save and exit. Cron normally notices the updated crontab automatically.

Verify the saved configuration:

``` bash
crontab -l
```

## 9.3 Temporarily disable a job

Put `#` at the beginning of the job line:

``` cron
# */10 * * * * /usr/bin/date >> /home/sahil/cron-lab/logs/every-five-minutes.log 2>&1
```

A line beginning with `#` is a comment, so cron ignores it.

To enable the job again, remove the `#` and save.

## 9.4 Delete one job

Open the crontab:

``` bash
crontab -e
```

Delete only the line you no longer need. Save and exit.

## 9.5 Delete the entire crontab

``` bash
crontab -r
```

This removes every job belonging to the current user. Use it only when
you intentionally want to remove the complete crontab.

### Summary: job management

  Action                How
  --------------------- -------------------------------------------
  Create                Add a new line using `crontab -e`
  View                  `crontab -l`
  Update                Edit the existing line using `crontab -e`
  Disable temporarily   Comment the line using `#`
  Re-enable             Remove the leading `#`
  Delete one job        Remove its line in `crontab -e`
  Delete all jobs       `crontab -r` --- use with caution

------------------------------------------------------------------------

# 10. Important: User crontab vs root crontab

Each Linux user can have their own crontab.

For the current user:

``` bash
crontab -e
crontab -l
```

For root:

``` bash
sudo crontab -e
sudo crontab -l
```

For another user, with appropriate privileges:

``` bash
sudo crontab -u username -e
sudo crontab -u username -l
```

## When should you use root's crontab?

Use root's crontab only when the task genuinely needs root privileges,
such as a carefully designed system maintenance operation.

For a script that reads and writes only files owned by your normal user,
use that user's crontab.

**Security principle:** Follow least privilege. Do not run every
scheduled script as root.

------------------------------------------------------------------------

# 11. Important: Absolute paths and cron's environment

A command that works in your interactive terminal may fail in cron.

Why?

Cron starts jobs with a limited environment. It may not load the same
shell profile, `PATH`, working directory, or environment variables that
your interactive terminal uses.

## Use absolute paths

Instead of:

``` cron
* * * * * python3 script.py
```

Prefer:

``` cron
* * * * * /usr/bin/python3 /home/sahil/cron-lab/script.py
```

Find the full path to a command:

``` bash
command -v python3
command -v date
command -v bash
```

Find your home directory:

``` bash
echo "$HOME"
```

## Set PATH explicitly when needed

You can define environment variables at the top of a user crontab:

``` cron
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
SHELL=/bin/bash
```

Then add your job below them.

Do not assume a `.bashrc` or `.profile` file will be loaded by cron. If
your script needs environment variables, define or load them
deliberately and securely.

------------------------------------------------------------------------

# 12. Capture output and errors in log files

Cron jobs often run without a visible terminal. Logging is essential for
understanding what happened.

## 12.1 Redirect normal output

``` cron
*/5 * * * * /home/sahil/cron-lab/health-check.sh >> /home/sahil/cron-lab/logs/health-check.log
```

`>>` appends normal output to the log instead of replacing it.

## 12.2 Redirect both output and errors

``` cron
*/5 * * * * /home/sahil/cron-lab/health-check.sh >> /home/sahil/cron-lab/logs/health-check.log 2>&1
```

Explanation:

-   `>` redirects standard output.
-   `>>` appends standard output.
-   `2>` redirects standard error.
-   `2>&1` sends standard error to the same destination as standard
    output.

## 12.3 Replace the log on every run

``` cron
0 2 * * * /home/sahil/cron-lab/backup.sh > /home/sahil/cron-lab/logs/backup.log 2>&1
```

This replaces the log contents each time. Use `>>` if you want to keep
appending.

## 12.4 Check logs

``` bash
tail -n 50 ~/cron-lab/logs/health-check.log
```

Follow new entries:

``` bash
tail -f ~/cron-lab/logs/health-check.log
```

Plan log rotation or cleanup for long-running systems so log files do
not grow forever.

------------------------------------------------------------------------

# 13. Practical Lab 2 --- Create an automated server health report

**Scenario:** You are a junior Linux administrator. Your team wants a
report containing the server's date, hostname, uptime, disk usage,
memory usage, and load average every five minutes.

## Step 1: Create the lab folders

``` bash
mkdir -p ~/cron-lab/{scripts,logs,reports}
```

## Step 2: Create the script

``` bash
nano ~/cron-lab/scripts/health-report.sh
```

Paste:

``` bash
#!/usr/bin/env bash
set -u

REPORT_DIR="$HOME/cron-lab/reports"
REPORT_FILE="$REPORT_DIR/health-report-$(date +%Y%m%d).log"

mkdir -p "$REPORT_DIR"

{
    echo "========================================"
    echo "SERVER HEALTH REPORT"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "========================================"

    echo
    echo "UPTIME"
    uptime

    echo
    echo "DISK USAGE"
    df -h

    echo
    echo "MEMORY USAGE"
    free -h

    echo
    echo "LOAD AVERAGE"
    cat /proc/loadavg

    echo
    echo "END OF REPORT"
    echo
} >> "$REPORT_FILE"
```

## Step 3: Make it executable

``` bash
chmod 750 ~/cron-lab/scripts/health-report.sh
```

## Step 4: Run it manually first

``` bash
~/cron-lab/scripts/health-report.sh
```

## Step 5: Verify the report

``` bash
ls -lh ~/cron-lab/reports/
tail -n 40 ~/cron-lab/reports/health-report-$(date +%Y%m%d).log
```

Do not schedule a script until it works manually.

## Step 6: Schedule it every five minutes

Open:

``` bash
crontab -e
```

Add:

``` cron
*/5 * * * * /home/sahil/cron-lab/scripts/health-report.sh
```

Replace `/home/sahil` with your actual home directory.

## Step 7: Verify the job

``` bash
crontab -l
```

Wait for the next five-minute interval, then inspect the report:

``` bash
tail -n 80 ~/cron-lab/reports/health-report-$(date +%Y%m%d).log
```

## Step 8: Update the schedule to every ten minutes

Edit the crontab:

``` bash
crontab -e
```

Change:

``` cron
*/5 * * * * /home/sahil/cron-lab/scripts/health-report.sh
```

to:

``` cron
*/10 * * * * /home/sahil/cron-lab/scripts/health-report.sh
```

Save and verify:

``` bash
crontab -l
```

## Step 9: Disable it temporarily

Add `#` before the line:

``` cron
# */10 * * * * /home/sahil/cron-lab/scripts/health-report.sh
```

Save and check that the line is commented:

``` bash
crontab -l
```

## Step 10: Enable it again

Remove the `#` and save.

## Step 11: Delete only this job

Open `crontab -e`, remove the health-report line, save, and verify:

``` bash
crontab -l
```

**Expected result:** You have created a scheduled task, tested it,
updated its frequency, disabled it, enabled it, and removed it.

------------------------------------------------------------------------

# 14. Practical Lab 3 --- Automated application backup

**Scenario:** A small company stores its application in `/opt/demo-app`.
The administrator wants a compressed backup every day at 2 AM.

> This is a controlled lab. If you do not have permission to write under
> `/opt`, use a directory inside your home folder instead.

## Step 1: Create sample application data

For a home-directory lab:

``` bash
mkdir -p ~/cron-lab/demo-app/{app,config,uploads}
echo "Demo application" > ~/cron-lab/demo-app/app/app.txt
echo "PORT=8080" > ~/cron-lab/demo-app/config/app.conf
echo "Sample upload" > ~/cron-lab/demo-app/uploads/readme.txt
```

## Step 2: Create a backup directory

``` bash
mkdir -p ~/cron-lab/backups
```

## Step 3: Create the backup script

``` bash
nano ~/cron-lab/scripts/backup.sh
```

Paste:

``` bash
#!/usr/bin/env bash
set -Eeuo pipefail

SOURCE_DIR="$HOME/cron-lab/demo-app"
BACKUP_DIR="$HOME/cron-lab/backups"
TIMESTAMP="$(date +%Y%m%d-%H%M%S)"
ARCHIVE="$BACKUP_DIR/demo-app-$TIMESTAMP.tar.gz"
LOG_FILE="$HOME/cron-lab/logs/backup.log"

mkdir -p "$BACKUP_DIR" "$(dirname "$LOG_FILE")"

if [[ ! -d "$SOURCE_DIR" ]]; then
    echo "ERROR: Source directory not found: $SOURCE_DIR" | tee -a "$LOG_FILE"
    exit 1
fi

echo "Starting backup at $(date)" | tee -a "$LOG_FILE"

tar -czf "$ARCHIVE" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"

gzip -t "$ARCHIVE"

echo "SUCCESS: $ARCHIVE" | tee -a "$LOG_FILE"
echo "Size: $(du -h "$ARCHIVE" | awk '{print $1}')" | tee -a "$LOG_FILE"
```

## Step 4: Make it executable

``` bash
chmod 750 ~/cron-lab/scripts/backup.sh
```

## Step 5: Run it manually

``` bash
~/cron-lab/scripts/backup.sh
```

## Step 6: Check the backup

``` bash
ls -lh ~/cron-lab/backups/
cat ~/cron-lab/logs/backup.log
```

## Step 7: Test restoration

Create a restore directory:

``` bash
mkdir -p ~/cron-lab/restore
```

Choose the archive you just created and extract it:

``` bash
tar -tzf ~/cron-lab/backups/ACTUAL_ARCHIVE_NAME.tar.gz
tar -xzf ~/cron-lab/backups/ACTUAL_ARCHIVE_NAME.tar.gz -C ~/cron-lab/restore
```

Replace `ACTUAL_ARCHIVE_NAME` with the actual archive filename.

## Step 8: Schedule daily at 2 AM

Open:

``` bash
crontab -e
```

Add:

``` cron
0 2 * * * /home/sahil/cron-lab/scripts/backup.sh
```

Replace `/home/sahil` with your real home directory.

## Step 9: Verify

``` bash
crontab -l
```

**Production reminder:** This example backs up files, not a live
database consistently. Real database backups should use database-aware
backup tools and procedures. Production backups should also consider
encryption, off-site copies, retention, monitoring, and restore testing.

------------------------------------------------------------------------

# 15. Practical Lab 4 --- Scheduled temporary-file cleanup

**Scenario:** A team creates temporary `.tmp` files in a dedicated lab
folder. You need to delete only files older than seven days, every
Sunday at 1 AM.

Do not practise destructive cleanup in `/tmp`, `/var`, or a production
directory. Use the dedicated lab directory below.

## Step 1: Create the folder

``` bash
mkdir -p ~/cron-lab/temp
```

## Step 2: Create sample files

``` bash
touch ~/cron-lab/temp/today.tmp
touch ~/cron-lab/temp/old.tmp
```

Make one file appear older than seven days:

``` bash
touch -d "10 days ago" ~/cron-lab/temp/old.tmp
```

Check timestamps:

``` bash
ls -l --time-style=long-iso ~/cron-lab/temp/
```

## Step 3: Test the find command without deleting anything

``` bash
find ~/cron-lab/temp -type f -name '*.tmp' -mtime +7 -print
```

This displays matching files. It does not delete them.

## Step 4: Create a cleanup script

``` bash
nano ~/cron-lab/scripts/cleanup.sh
```

Paste:

``` bash
#!/usr/bin/env bash
set -Eeuo pipefail

TARGET_DIR="$HOME/cron-lab/temp"
LOG_FILE="$HOME/cron-lab/logs/cleanup.log"

mkdir -p "$(dirname "$LOG_FILE")"

if [[ ! -d "$TARGET_DIR" ]]; then
    echo "ERROR: Target directory missing: $TARGET_DIR" >> "$LOG_FILE"
    exit 1
fi

echo "Cleanup started: $(date)" >> "$LOG_FILE"

find "$TARGET_DIR" -type f -name '*.tmp' -mtime +7 -print -delete >> "$LOG_FILE"

echo "Cleanup completed: $(date)" >> "$LOG_FILE"
```

## Step 5: Make it executable and test it

``` bash
chmod 750 ~/cron-lab/scripts/cleanup.sh
~/cron-lab/scripts/cleanup.sh
```

Check the result:

``` bash
ls -lah ~/cron-lab/temp/
cat ~/cron-lab/logs/cleanup.log
```

## Step 6: Schedule it for Sunday at 1 AM

``` cron
0 1 * * 0 /home/sahil/cron-lab/scripts/cleanup.sh
```

Replace `/home/sahil` with your actual home directory.

**Important:** `find -mtime +7` uses file modification-time age in
24-hour periods, with day rounding behaviour. Test the selection with
`-print` before adding `-delete`. Keep the target path narrow and
explicit.

------------------------------------------------------------------------

# 16. Practical Lab 5 --- Check whether an application is running

**Scenario:** A simple service check should run every five minutes and
record whether a process is present.

This lab checks a process name as a learning exercise. In production,
use the application's service manager, health endpoint, or monitoring
system where possible.

## Step 1: Create a script

``` bash
nano ~/cron-lab/scripts/process-check.sh
```

Paste:

``` bash
#!/usr/bin/env bash
set -u

LOG_FILE="$HOME/cron-lab/logs/process-check.log"
mkdir -p "$(dirname "$LOG_FILE")"

if pgrep -x "sshd" > /dev/null; then
    echo "$(date): sshd process is running" >> "$LOG_FILE"
else
    echo "$(date): WARNING: sshd process was not found" >> "$LOG_FILE"
fi
```

The process name `sshd` may not exist on every machine. Check available
processes first:

``` bash
pgrep -a sshd
```

If your system uses a different process or service, adjust the example.

## Step 2: Make it executable and run it

``` bash
chmod 750 ~/cron-lab/scripts/process-check.sh
~/cron-lab/scripts/process-check.sh
```

## Step 3: Schedule it

``` cron
*/5 * * * * /home/sahil/cron-lab/scripts/process-check.sh
```

Verify the output:

``` bash
tail -n 20 ~/cron-lab/logs/process-check.log
```

------------------------------------------------------------------------

# 17. System-wide cron configuration

Most beginners should start with `crontab -e`. Linux also provides
system-wide scheduling locations.

Common locations include:

``` text
/etc/crontab
/etc/cron.d/
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
```

The exact directories and how they are run depend on the distribution
and installed packages.

## 17.1 Difference between user crontab and /etc/crontab

A user crontab has five time fields followed by a command:

``` cron
0 2 * * * /usr/local/bin/backup.sh
```

The system crontab `/etc/crontab` includes an additional username field:

``` cron
0 2 * * * root /usr/local/bin/backup.sh
```

The username tells cron which account should run the command.

Do not add a username field to a normal user crontab created with
`crontab -e`; it will be interpreted as part of the command and may
fail.

## 17.2 Inspect system cron locations

``` bash
ls -lah /etc/cron.d/
ls -lah /etc/cron.daily/
cat /etc/crontab
```

Editing system cron files usually requires root privileges. Follow your
organisation's change-management process before modifying them.

------------------------------------------------------------------------

# 18. Cron access control

Some Linux systems use these files to control which users may use cron:

``` text
/etc/cron.allow
/etc/cron.deny
```

Their availability and exact behaviour depend on the cron implementation
and distribution.

-   `cron.allow` can define users permitted to use cron.
-   `cron.deny` can define users blocked from using cron.

Do not create or modify these files casually on a shared server. Check
your distribution's documentation and local security policy first.

------------------------------------------------------------------------

# 19. Troubleshooting: Why is my cron job not running?

A job can be correctly written but still fail. Troubleshooting should be
systematic.

## Problem 1: Cron service is not running

Ubuntu/Debian:

``` bash
systemctl status cron
sudo systemctl enable --now cron
```

RHEL-based systems:

``` bash
systemctl status crond
sudo systemctl enable --now crond
```

## Problem 2: Wrong command path

Find the executable path:

``` bash
command -v python3
command -v bash
command -v date
```

Use absolute paths in cron entries and scripts.

## Problem 3: Script is not executable

Check:

``` bash
ls -l ~/cron-lab/scripts/
```

Add execute permission for the owner:

``` bash
chmod u+x ~/cron-lab/scripts/health-report.sh
```

## Problem 4: Incorrect script interpreter

Make sure the first line is correct, for example:

``` bash
#!/usr/bin/env bash
```

Find Bash:

``` bash
command -v bash
```

## Problem 5: Incorrect permissions

Check the script and directories:

``` bash
ls -ld "$HOME"
ls -ld ~/cron-lab
ls -l ~/cron-lab/scripts/health-report.sh
```

The cron user needs permission to access the script, its parent
directories, and the output destination.

## Problem 6: No output or error logging

Add redirection:

``` cron
*/5 * * * * /home/sahil/cron-lab/scripts/health-report.sh >> /home/sahil/cron-lab/logs/cron-error.log 2>&1
```

## Problem 7: Wrong schedule

Use a schedule reference table and verify each field carefully.

For example:

``` cron
0 2 * * * command
```

means every day at 2:00 AM, not every two hours.

## Problem 8: Wrong time zone

Check:

``` bash
date
timedatectl
```

Cron uses the system's configured local time unless a supported
time-zone setting is configured. Do not assume it uses your laptop's
time zone.

## Problem 9: Inspect cron logs

On Ubuntu/Debian, cron messages are often available through:

``` bash
sudo grep CRON /var/log/syslog
```

On RHEL-based systems, check:

``` bash
sudo grep CRON /var/log/cron
```

The log file may not exist or may be managed differently. You can also
inspect the system journal:

``` bash
sudo journalctl -u cron
sudo journalctl -u crond
```

Use the service name that exists on your system.

## Problem 10: Test the script manually

Run the exact script as the same user who owns the crontab:

``` bash
/home/sahil/cron-lab/scripts/health-report.sh
```

A manual test helps separate script errors from cron scheduling errors.

------------------------------------------------------------------------

# 20. Cron limitations and important production considerations

Cron is useful, but it is not a complete workflow orchestration
platform.

-   **Missed run:** Traditional cron does not automatically replay every
    job missed while the machine was powered off. Some systems use
    anacron or other schedulers for catch-up behaviour.
-   **Overlapping jobs:** If a job takes longer than its interval,
    another run may start while the previous one is still running. Use
    locking, such as `flock`, where appropriate.
-   **Environment:** Cron provides a limited environment. Define paths
    and required variables explicitly.
-   **Time zones and daylight saving:** A schedule based on local time
    may behave differently around clock changes. Understand the server's
    time-zone policy.
-   **Output:** Capture logs and errors. Do not assume a successful
    schedule means a successful task.
-   **Security:** Use least privilege and protect scripts, credentials,
    and log files.
-   **Monitoring:** Important jobs should report success and failure to
    a monitoring or alerting system.
-   **Backups:** A backup job is not proven until restoration has been
    tested.

## Prevent overlapping executions with flock

On many Linux systems, `flock` is available through the util-linux
package.

Example:

``` cron
*/5 * * * * /usr/bin/flock -n /tmp/health-report.lock /home/sahil/cron-lab/scripts/health-report.sh >> /home/sahil/cron-lab/logs/health-report.log 2>&1
```

This requests a non-blocking lock. If another instance holds the lock,
the new run does not start.

Find its path:

``` bash
command -v flock
```

Use a lock-file location and permissions appropriate for the user
running the job. Do not share one lock file between unrelated jobs.

------------------------------------------------------------------------

# 21. Optional: One-time scheduling with `at`

Cron is designed for recurring schedules. The `at` command is commonly
used to run a command once at a future time.

Check whether it is installed:

``` bash
command -v at
```

Ubuntu/Debian installation:

``` bash
sudo apt install at
sudo systemctl enable --now atd
```

RHEL-based installation:

``` bash
sudo dnf install at
sudo systemctl enable --now atd
```

Schedule a one-time command:

``` bash
echo 'date >> "$HOME/at-test.log"' | at now + 2 minutes
```

List pending jobs:

``` bash
atq
```

Remove a pending job using its job number:

``` bash
atrm JOB_NUMBER
```

Replace `JOB_NUMBER` with the number shown by `atq`.

Use cron for recurring tasks and `at` for one-time future tasks.

------------------------------------------------------------------------

# 22. Final comprehensive practical --- Linux Automation Administrator

## Scenario

You have joined a small company as a Junior Linux Administrator. The
company has a Linux server supporting an internal application.

The team wants the following tasks automated:

  Task                                         Schedule
  -------------------------------------------- ----------------------
  Server health report                         Every 5 minutes
  Application file backup                      Every day at 2 AM
  Temporary `.tmp` cleanup older than 7 days   Every Sunday at 1 AM
  Process check                                Every 10 minutes

## Your mission

### Part A --- Prepare the environment

``` bash
mkdir -p ~/cron-lab/{scripts,logs,reports,backups,temp,demo-app}
```

Create sample application files:

``` bash
mkdir -p ~/cron-lab/demo-app/{app,config,uploads}
echo "Demo app" > ~/cron-lab/demo-app/app/app.txt
echo "PORT=8080" > ~/cron-lab/demo-app/config/app.conf
echo "Sample upload" > ~/cron-lab/demo-app/uploads/readme.txt
```

### Part B --- Create the scripts

Create these scripts using the examples in this module:

``` text
~/cron-lab/scripts/
├── health-report.sh
├── backup.sh
├── cleanup.sh
└── process-check.sh
```

For every script:

1.  Write the script.
2.  Check its contents.
3.  Make it executable.
4.  Run it manually.
5.  Inspect its output and log.
6.  Fix any errors before scheduling it.

### Part C --- Add all four jobs

Open:

``` bash
crontab -e
```

Add the following entries, replacing `/home/sahil` with your actual home
directory:

``` cron
*/5 * * * * /home/sahil/cron-lab/scripts/health-report.sh >> /home/sahil/cron-lab/logs/health-cron.log 2>&1
0 2 * * * /home/sahil/cron-lab/scripts/backup.sh >> /home/sahil/cron-lab/logs/backup-cron.log 2>&1
0 1 * * 0 /home/sahil/cron-lab/scripts/cleanup.sh >> /home/sahil/cron-lab/logs/cleanup-cron.log 2>&1
*/10 * * * * /home/sahil/cron-lab/scripts/process-check.sh >> /home/sahil/cron-lab/logs/process-cron.log 2>&1
```

### Part D --- Verify all jobs

``` bash
crontab -l
```

Check logs:

``` bash
ls -lh ~/cron-lab/logs/
tail -n 30 ~/cron-lab/logs/health-cron.log
tail -n 30 ~/cron-lab/logs/backup-cron.log
tail -n 30 ~/cron-lab/logs/cleanup-cron.log
tail -n 30 ~/cron-lab/logs/process-cron.log
```

Wait for the scheduled intervals. Daily and weekly jobs will not run
immediately unless their scheduled time arrives. Test those scripts
manually rather than waiting for the calendar.

### Part E --- Modify schedules

Change the health report from every five minutes to every fifteen
minutes.

Change the process check from every ten minutes to every thirty minutes.

Use:

``` bash
crontab -e
```

Update the expressions and verify:

``` bash
crontab -l
```

### Part F --- Disable and re-enable a task

Temporarily disable the process check by commenting out its line:

``` cron
# */30 * * * * /home/sahil/cron-lab/scripts/process-check.sh >> /home/sahil/cron-lab/logs/process-cron.log 2>&1
```

Then re-enable it by removing `#`.

### Part G --- Remove one task safely

Remove only the process-check line using:

``` bash
crontab -e
```

Do not remove the entire crontab.

### Part H --- Recovery and documentation

Write a short report containing:

-   The Linux distribution and cron service name.
-   The four scripts you created.
-   The final schedule for each script.
-   Evidence that each script ran manually.
-   Relevant log output.
-   One example of a schedule you modified.
-   One example of a job you disabled and re-enabled.
-   One example of a job you deleted.
-   Any errors and how you fixed them.

## Expected learning outcome

After this project, you should be comfortable creating, scheduling,
verifying, updating, disabling, re-enabling, and deleting cron jobs. You
should also understand how to troubleshoot a job that does not run.

------------------------------------------------------------------------

# 23. Interview questions and answers

### Q1. What is cron?

Cron is a time-based job scheduler used to run commands and scripts
automatically at specified recurring times.

### Q2. What is crontab?

Crontab is the configuration and command interface used to manage
scheduled cron jobs for a user.

### Q3. What is a cron daemon?

It is the background service that checks schedules and starts jobs when
their scheduled times arrive.

### Q4. What is the difference between cron and crontab?

Cron is the scheduling service/system. Crontab is the schedule
configuration managed using the `crontab` command.

### Q5. How do you list cron jobs?

``` bash
crontab -l
```

### Q6. How do you edit cron jobs?

``` bash
crontab -e
```

### Q7. How do you remove one cron job?

Open `crontab -e`, delete the required line, and save.

### Q8. How do you remove all cron jobs for a user?

``` bash
crontab -r
```

This removes the user's entire crontab. Use it carefully.

### Q9. How do you schedule a job every five minutes?

``` cron
*/5 * * * * command
```

### Q10. How do you schedule a job every day at 2 AM?

``` cron
0 2 * * * command
```

### Q11. What does `@reboot` mean?

It is a cron shortcut used to run a job when the cron daemon starts or
the system boots, depending on the implementation.

### Q12. Why should absolute paths be used in cron?

Cron has a limited environment and may not have the same `PATH` or
working directory as an interactive terminal.

### Q13. How do you capture errors from a cron job?

``` cron
* * * * * command >> /path/to/job.log 2>&1
```

### Q14. What is the difference between a user crontab and `/etc/crontab`?

A user crontab has five time fields followed by the command.
`/etc/crontab` includes an additional username field.

### Q15. Does cron automatically run a job missed while the server is off?

Traditional cron generally does not replay missed runs. Other scheduling
tools or anacron may provide catch-up behaviour.

### Q16. How can you prevent overlapping cron jobs?

Use a locking mechanism such as `flock`, where available and
appropriate.

### Q17. What is the difference between cron and `at`?

Cron is generally used for recurring jobs. `at` is used for one-time
future jobs.

------------------------------------------------------------------------

# 24. Quick revision cheat sheet

``` bash
# Identify Linux distribution
cat /etc/os-release

# Check crontab command
command -v crontab

# Edit current user's cron jobs
crontab -e

# List current user's cron jobs
crontab -l

# Back up current crontab
crontab -l > "$HOME/crontab-backup.txt"

# Restore a saved crontab
crontab "$HOME/crontab-backup.txt"

# Remove entire current user's crontab (use caution)
crontab -r

# Edit root's crontab
sudo crontab -e

# List root's crontab
sudo crontab -l

# Check server time
date
timedatectl

# Check cron service on Ubuntu/Debian
systemctl status cron

# Start and enable cron on Ubuntu/Debian
sudo systemctl enable --now cron

# Check cron service on RHEL-based systems
systemctl status crond

# Start and enable crond on RHEL-based systems
sudo systemctl enable --now crond

# Check script permissions
ls -l /path/to/script.sh

# Make a script executable
chmod u+x /path/to/script.sh

# Find a command's full path
command -v python3

# Inspect recent cron logs (Ubuntu/Debian, if using syslog)
sudo grep CRON /var/log/syslog

# Inspect recent cron logs (RHEL-based systems, if present)
sudo grep CRON /var/log/cron

# Inspect system journal
sudo journalctl -u cron
sudo journalctl -u crond
```

## Common schedule examples

``` cron
# Every minute
* * * * * command

# Every five minutes
*/5 * * * * command

# Every fifteen minutes
*/15 * * * * command

# Every hour
0 * * * * command

# Every day at 2 AM
0 2 * * * command

# Every Monday at 9 AM
0 9 * * 1 command

# Every Sunday at 1 AM
0 1 * * 0 command

# Every weekday at 9 AM
0 9 * * 1-5 command

# At cron startup / system boot, depending on implementation
@reboot command
```

------------------------------------------------------------------------

# 25. Module completion

You have completed **Linux Task Scheduling using Cron and Crontab**.

You have learned how to:

-   Understand cron and the cron daemon.
-   Install and check the cron service.
-   Read and write cron expressions.
-   Create recurring jobs.
-   Update schedules.
-   Disable and re-enable jobs.
-   Delete one job or an entire crontab.
-   Schedule scripts and capture logs.
-   Troubleshoot common failures.
-   Build practical Linux automation.

## Suggested next learning path

After Linux fundamentals and task scheduling, continue with:

1.  Shell scripting in depth
2.  Git and GitHub
3.  Linux networking and troubleshooting
4.  Docker and container fundamentals
5.  CI/CD pipelines
6.  AWS cloud fundamentals
7.  Terraform and Infrastructure as Code
8.  Kubernetes
9.  Monitoring and observability

**Final advice:** Do not just memorise cron expressions. Practise
creating a job, waiting for it to run, checking its logs, changing its
schedule, disabling it, and safely removing it. That is how scheduling
becomes a real administration skill.

------------------------------------------------------------------------

**End of Module --- Linux Task Scheduling with Cron and Crontab**
