# Linux Networking — Notes

Linux networking knowledge is essential for DevOps, SRE and cloud roles.

## Important Commands

Modern systems commonly use:

```bash
ip addr
ip route
ip link
ss -tulpn
ping
curl
dig
nslookup
traceroute
hostname
```

## IP Address

Check:

```bash
ip addr
```

## Routing

```bash
ip route
```

The routing table determines where packets should go.

## Ports

Check listening sockets:

```bash
ss -tulpn
```

A port is associated with a network service.

Common examples:
- SSH: 22
- HTTP: 80
- HTTPS: 443
- DNS: 53

## DNS

DNS maps names to network addresses.

```bash
dig example.com
```

## HTTP Testing

```bash
curl -I https://example.com
```

## Troubleshooting Model

When a service is unreachable:

```text
DNS
 ↓
IP connectivity
 ↓
Routing
 ↓
Firewall/security rules
 ↓
Port listening
 ↓
Application
```

Do not jump directly to application logs before checking basic connectivity.
