# Linux Task Scheduling — Notes

Linux can run commands automatically at scheduled times.

Two important concepts:

- cron — recurring scheduled tasks
- systemd timers — scheduling integrated with systemd

## Cron

Edit the current user's crontab:

```bash
crontab -e
```

List entries:

```bash
crontab -l
```

Cron format:

```text
minute hour day-of-month month day-of-week command
```

Example:

```text
0 2 * * * /home/user/backup.sh
```

This means approximately 2:00 AM every day.

## Best Practices

- Use absolute paths.
- Log output.
- Make scripts executable.
- Test scripts manually first.
- Avoid hard-coded secrets.
- Use appropriate permissions.
- Consider systemd timers for modern service-oriented systems.

## Logging

A scheduled script should make it possible to determine:
- Did it run?
- When did it run?
- Did it succeed?
- What failed?
