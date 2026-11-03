# Linux Processes — Practical Lab

## Goal

Start, inspect and safely stop processes.

## Step 1

```bash
sleep 300 &
```

Find it:

```bash
pgrep sleep
ps aux | grep sleep
```

## Step 2

Check jobs:

```bash
jobs
```

## Step 3 — Graceful Stop

```bash
kill <PID>
```

Verify:

```bash
pgrep sleep
```

## Step 4 — CPU Experiment

```bash
yes > /dev/null &
```

Find:

```bash
pgrep yes
top
```

Stop it:

```bash
kill <PID>
```

If required:

```bash
kill -9 <PID>
```

Only use `-9` after normal termination is unsuccessful.

## Step 5 — Memory and Disk

```bash
free -h
df -h
du -sh ~/linux-lab
```

## Investigation Exercise

Imagine CPU is at 100%.

Write the troubleshooting sequence before running commands.

## Cleanup

Make sure no `yes` process remains:

```bash
pgrep yes
```

If any lab process remains, stop it.
