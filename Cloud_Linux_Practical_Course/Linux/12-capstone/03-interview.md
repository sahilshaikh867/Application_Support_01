# Linux Administration Capstone — Interview Questions

## Beginner

1. How do you create a Linux user?
2. How do you create a group?
3. How do you add a user to a group?
4. How do you inspect permissions?
5. How do you create a tar.gz backup?

## Intermediate

6. Design permissions for a shared application directory.
7. When would you use ACL instead of changing the group?
8. How would you schedule a health check?
9. How would you find whether an application is listening on port 8080?
10. How would you check whether CPU or memory is the bottleneck?

## Advanced Scenarios

### Scenario 1 — Permission Denied

An application cannot write to `/opt/app/logs`.

Explain your troubleshooting sequence.

### Scenario 2 — Scheduled Job Failure

A script works manually but fails from cron.

Explain at least five things you would investigate.

### Scenario 3 — High CPU

A server suddenly has 100% CPU.

Explain how you would:
1. Identify the process.
2. Confirm whether the process is expected.
3. Investigate logs.
4. Decide whether to stop it.
5. Prevent recurrence.

### Scenario 4 — Network Failure

Port 8080 works from localhost but not from another machine.

Explain the possible causes.

### Scenario 5 — Backup Failure

A backup file exists but restoration fails.

Explain why this proves that file existence is not enough and describe how you would improve backup verification.
