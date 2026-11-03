# Linux Networking — Practical Lab

## Goal

Inspect interfaces, routes, ports and DNS.

## Step 1 — Interface

```bash
ip addr
```

Record:
- Interface name
- IPv4 address
- Interface state

## Step 2 — Route

```bash
ip route
```

Identify the default route.

## Step 3 — Connectivity

```bash
ping -c 4 8.8.8.8
```

Then test DNS:

```bash
ping -c 4 example.com
```

If IP works but hostname fails, investigate DNS.

## Step 4 — DNS

```bash
dig example.com
```

or:

```bash
nslookup example.com
```

## Step 5 — HTTP

```bash
curl -I https://example.com
```

## Step 6 — Listening Ports

```bash
ss -tulpn
```

If Nginx is installed:

```bash
sudo ss -tulpn | grep ':80'
```

## Step 7 — Troubleshooting Scenario

A user reports:

> "The server has internet, but the application on port 8080 is unreachable."

Create a troubleshooting checklist:

1. Is the application running?
2. Is port 8080 listening?
3. Is it bound to `127.0.0.1` or the expected interface?
4. Is the OS firewall blocking it?
5. Is the cloud security group blocking it?
6. Is the route correct?
7. Is DNS resolving correctly?

## Verification

Explain each command you used and what evidence it provided.
