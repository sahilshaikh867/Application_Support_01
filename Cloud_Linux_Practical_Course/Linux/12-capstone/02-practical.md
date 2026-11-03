# Linux Administration Capstone — Full Practical

## Phase 1 — Users and Groups

Create:

```bash
sudo useradd -m -s /bin/bash appadmin
sudo useradd -m -s /bin/bash developer1
sudo useradd -m -s /bin/bash developer2
sudo groupadd appteam
sudo usermod -aG appteam developer1
sudo usermod -aG appteam developer2
```

Verify:

```bash
id developer1
id developer2
getent group appteam
```

## Phase 2 — Application Directory

```bash
sudo mkdir -p /opt/training-app/{config,logs,backup}
sudo chown -R appadmin:appteam /opt/training-app
sudo chmod -R 2770 /opt/training-app
```

Verify:

```bash
ls -ld /opt/training-app
```

## Phase 3 — ACL

Create a report:

```bash
sudo touch /opt/training-app/config/application.conf
sudo setfacl -m u:developer1:r /opt/training-app/config/application.conf
getfacl /opt/training-app/config/application.conf
```

## Phase 4 — Health Check

Create:

```bash
sudo nano /opt/training-app/health-check.sh
```

Example:

```bash
#!/bin/bash
echo "$(date) - application health check" >> /opt/training-app/logs/health.log
```

Make executable:

```bash
sudo chmod 750 /opt/training-app/health-check.sh
```

Run manually:

```bash
sudo /opt/training-app/health-check.sh
```

## Phase 5 — Schedule

Use the `appadmin` user's crontab or a controlled system scheduler.

Example:

```text
*/5 * * * * /opt/training-app/health-check.sh
```

Use a short lab interval only. Remove it after testing.

## Phase 6 — Archive

```bash
sudo tar -czf /opt/training-app/backup/app-$(date +%Y%m%d-%H%M).tar.gz /opt/training-app/config /opt/training-app/logs
```

List:

```bash
sudo tar -tzf /opt/training-app/backup/*.tar.gz
```

## Phase 7 — Networking

```bash
ip addr
ip route
ss -tulpn
curl -I https://example.com
```

## Final Deliverables

Create a report containing:

1. Users created.
2. Groups created.
3. Permission model.
4. ACL rules.
5. Scheduled job.
6. Backup command.
7. Network checks.
8. Troubleshooting steps.
9. Cleanup commands.

## Cleanup

Remove only resources created for this lab.
