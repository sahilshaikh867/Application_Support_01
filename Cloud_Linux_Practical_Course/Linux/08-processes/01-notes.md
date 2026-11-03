# Linux Processes — Notes

A process is a running instance of a program.

## Process Information

```bash
ps
ps aux
top
htop
pgrep
pidof
```

## Important Concepts

- PID — Process ID
- PPID — Parent Process ID
- foreground process
- background process
- daemon
- signal

## Signals

Common signals:

```text
SIGTERM  15 — request graceful termination
SIGKILL   9 — force termination
SIGHUP    1 — commonly used to reload/re-read configuration
```

Use `SIGKILL` carefully because the process cannot clean up normally.

## Commands

```bash
kill PID
kill -TERM PID
kill -KILL PID
pkill process_name
```

## Background Jobs

```bash
command &
jobs
fg
bg
```

## Resource Investigation

```bash
free -h
uptime
df -h
du -sh
top
```

A good troubleshooter identifies whether the issue is CPU, memory, disk, network or the application itself.
