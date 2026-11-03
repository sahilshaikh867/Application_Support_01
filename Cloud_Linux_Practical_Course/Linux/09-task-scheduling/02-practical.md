# Linux Task Scheduling — Practical Lab

## Goal

Create a safe recurring job and verify its execution.

## Step 1 — Script

```bash
mkdir -p ~/linux-lab/cron
nano ~/linux-lab/cron/health-check.sh
```

Add:

```bash
#!/bin/bash
echo "$(date) - Health check executed" >> "$HOME/linux-lab/cron/health.log"
```

Make executable:

```bash
chmod +x ~/linux-lab/cron/health-check.sh
```

Test manually:

```bash
~/linux-lab/cron/health-check.sh
cat ~/linux-lab/cron/health.log
```

## Step 2 — Schedule

```bash
crontab -e
```

Add a temporary every-minute entry:

```text
* * * * * /home/YOUR_USER/linux-lab/cron/health-check.sh
```

Replace `YOUR_USER` with the actual username.

Wait 1–2 minutes.

Check:

```bash
cat ~/linux-lab/cron/health.log
```

## Step 3 — Remove the Job

```bash
crontab -e
```

Delete the temporary entry.

## Troubleshooting Questions

- Did the script have execute permission?
- Did you use an absolute path?
- Is cron running?
- Is the log path writable?
- Does the script work manually?

## Cleanup

```bash
rm -rf ~/linux-lab/cron
```
