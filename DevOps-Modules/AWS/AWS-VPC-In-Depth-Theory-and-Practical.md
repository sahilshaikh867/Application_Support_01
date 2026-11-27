# AWS VPC – In-Depth Theory, Architecture and Practical Guide

**Cloud & DevOps Training | Beginner to Advanced**  
**Topic:** Amazon Virtual Private Cloud (VPC)  
**Trainer notes:** Sahil Shaikh

> Teacher-style notes: understand the concept, study the example, trace the network path, and then perform the lab. All diagrams below are ordinary Markdown code blocks, not Mermaid, so they render on GitHub.

## Learning Objectives

By the end of this lesson, students should be able to:

1. Explain VPC, Regions, Availability Zones and CIDR.
2. Design public, private and isolated subnets.
3. Explain route tables, Internet Gateways and NAT Gateways.
4. Differentiate Security Groups and Network ACLs.
5. Understand Elastic IPs, ENIs and VPC Endpoints.
6. Explain VPC Peering, Transit Gateway, VPN and Direct Connect.
7. Understand DNS, DHCP options and VPC Flow Logs.
8. Build and troubleshoot a basic multi-tier VPC.

---

# 1. What is Amazon VPC?

Amazon VPC stands for **Amazon Virtual Private Cloud**. It is a logically isolated virtual network in an AWS Region that you define. You choose IP address ranges, create subnets, configure routes, and control connectivity.

Think about a company campus:

- The campus is the VPC.
- Departments or buildings are subnets.
- Roads and signboards are route tables.
- The public entrance is like an Internet Gateway.
- A controlled exit for internal departments is like a NAT Gateway.
- Network access rules are like Security Groups and Network ACLs.

This is an analogy; AWS networking is implemented through virtual networking infrastructure.

Imagine an e-commerce application with a website, application servers and a database. Should every server be directly reachable from the internet? No. A VPC lets us separate these resources into tiers and control traffic between them.

### Example: three-tier application

```text
                         INTERNET USERS
                               |
                               v
                    +----------------------+
                    | Internet-facing      |
                    | Application Load     |
                    | Balancer              |
                    +----------------------+
                               |
                 +-------------+-------------+
                 |                           |
                 v                           v
          +-------------+             +-------------+
          | App Server  |             | App Server  |
          | Private AZ-A|             | Private AZ-B|
          +-------------+             +-------------+
                 |                           |
                 +-------------+-------------+
                               |
                               v
                    +----------------------+
                    | RDS Database         |
                    | Private DB subnets   |
                    +----------------------+

                  All resources are inside one VPC
```

The load balancer receives public traffic, application servers remain private, and the database is not directly exposed.

**Key point:** A VPC is regional. It can contain subnets in multiple Availability Zones in that Region.

Official reference: [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

---

# 2. AWS Region and Availability Zone

Before designing a VPC, understand where AWS resources live.

## 2.1 Region

An AWS Region is a separate geographic area where AWS operates infrastructure.

Examples:
- Mumbai: `ap-south-1`
- Singapore: `ap-southeast-1`
- N. Virginia: `us-east-1`
- Frankfurt: `eu-central-1`

Choose a Region by considering user latency, compliance, service availability, disaster recovery and cost.

## 2.2 Availability Zone (AZ)

An Availability Zone is an isolated location within an AWS Region, made up of one or more data centers with independent infrastructure.

```text
                 AWS REGION: ap-south-1
                      (Mumbai)
        +-----------------------------------+
        |                                   |
        |   +---------------+               |
        |   | Availability  |               |
        |   | Zone A        |               |
        |   | Subnet A      |               |
        |   +---------------+               |
        |                                   |
        |   +---------------+               |
        |   | Availability  |               |
        |   | Zone B        |               |
        |   | Subnet B      |               |
        |   +---------------+               |
        |                                   |
        |   +---------------+               |
        |   | Availability  |               |
        |   | Zone C        |               |
        |   | Subnet C      |               |
        |   +---------------+               |
        +-----------------------------------+
```

| Region | Availability Zone |
|---|---|
| Geographic AWS area | Isolated location within a Region |
| Contains multiple AZs | Belongs to one Region |
| Example: Mumbai | One AZ within Mumbai |

A VPC can span multiple AZs, but an individual subnet belongs to exactly one AZ.

### What is multi-AZ architecture?

Multi-AZ means distributing resources across two or more Availability Zones to reduce the impact of an AZ failure.

For example:
- Load balancer spans AZ-A and AZ-B.
- Application servers run in both AZs.
- Database uses a supported Multi-AZ deployment.

Creating two subnets alone does not make an application highly available. The application, load balancing, data layer and failover design must also support it.

---

# 3. IP Addressing and CIDR

This is one of the most important networking concepts in VPC.

## 3.1 What is an IP address?

An IP address identifies a network interface on an IP network.

Example IPv4 address:

```text
10.0.1.25
```

IPv4 contains 32 bits and is commonly written as four decimal octets. Each octet ranges from 0 to 255.

## 3.2 Private and Public IPv4 addresses

Private IPv4 addresses are used inside private networks and are not globally routable on the public internet.

The RFC 1918 private ranges are:

| Private range | CIDR |
|---|---|
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` |

Examples: `10.0.1.10`, `172.16.5.20`, `192.168.1.100`.

A public IPv4 address is globally routable, subject to routing and security configuration.

**Important:** Having a public IP alone does not guarantee connectivity. Routes, gateways, security rules, operating-system firewalls and application configuration must also allow traffic.

## 3.3 What is CIDR?

CIDR stands for **Classless Inter-Domain Routing**. It represents an IP range using an address and prefix length.

Example:

```text
10.0.0.0/16
```

`/16` means the first 16 bits identify the network portion. The remaining bits identify addresses within the range.

A smaller prefix number represents a larger IPv4 range.

| CIDR | Total IPv4 addresses |
|---|---:|
| `/16` | 65,536 |
| `/20` | 4,096 |
| `/24` | 256 |
| `/25` | 128 |
| `/26` | 64 |
| `/27` | 32 |
| `/28` | 16 |

These are total addresses, not the number assignable to resources in an AWS subnet.

## 3.4 What does `/24` mean?

`10.0.1.0/24` contains 256 addresses, from `10.0.1.0` through `10.0.1.255`.

AWS reserves five IPv4 addresses in each subnet:

1. First address: network address.
2. Second address: VPC router.
3. Third address: Amazon-provided DNS.
4. Fourth address: reserved for future use.
5. Last address: reserved network address.

Therefore, an AWS IPv4 subnet with `/24` has:

```text
256 total addresses
- 5 AWS-reserved addresses
= 251 usable IPv4 addresses
```

## 3.5 What is a VPC CIDR block?

When creating a VPC, you specify its IPv4 CIDR block.

Example:

```text
VPC CIDR: 10.0.0.0/16
```

This contains 65,536 IPv4 addresses. AWS allows a primary IPv4 VPC CIDR from `/16` through `/28`, subject to AWS restrictions. Additional CIDR blocks can be associated where supported.

For a first lab, use a private range such as `10.0.0.0/16`.

## 3.6 Dividing a VPC into subnets

Suppose the VPC CIDR is `10.0.0.0/16`.

| Subnet | CIDR | Planned purpose |
|---|---|---|
| Public-A | `10.0.1.0/24` | Load balancer / NAT Gateway |
| Public-B | `10.0.2.0/24` | Load balancer / NAT Gateway |
| Private-App-A | `10.0.11.0/24` | Application servers |
| Private-App-B | `10.0.12.0/24` | Application servers |
| Private-DB-A | `10.0.21.0/24` | Database subnet group |
| Private-DB-B | `10.0.22.0/24` | Database subnet group |

All subnet ranges must fit within the VPC address ranges and must not overlap.

**Real-world tip:** Plan IP ranges before creating infrastructure. Overlapping CIDRs can make VPC peering, VPN and hybrid connectivity difficult or impossible without redesign.

References:
- [VPC CIDR blocks](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-cidr-blocks.html)
- [Subnet sizing](https://docs.aws.amazon.com/vpc/latest/userguide/subnet-sizing.html)

---

# 4. What is a Subnet?

A subnet is a range of IP addresses inside a VPC. You launch resources such as EC2 instances into subnets.

A subnet belongs to one Availability Zone. A VPC can span multiple AZs, but a subnet cannot span AZs.

## 4.1 Public subnet

A subnet is considered public when its associated route table has a route to an Internet Gateway for internet-routable traffic.

Example:

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |
| `0.0.0.0/0` | Internet Gateway |

A resource also needs suitable public addressing and security configuration for direct internet communication.

Typical resources:
- Internet-facing Application Load Balancer.
- NAT Gateway.
- Bastion host, where still required.

## 4.2 Private subnet

A private subnet does not have a direct route to an Internet Gateway. It may have a route to a NAT Gateway for outbound IPv4 internet access.

Example:

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |
| `0.0.0.0/0` | NAT Gateway |

Typical resources:
- Application servers.
- Backend services.
- Internal APIs.
- Worker nodes.

## 4.3 Isolated subnet

An isolated subnet has no route to an Internet Gateway or NAT Gateway.

Example:

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |

Typical use:
- Database tiers.
- Sensitive internal services.

An isolated subnet may still use specifically configured private endpoints or other private network connections.

| Feature | Public | Private | Isolated |
|---|---|---|---|
| Route to Internet Gateway | Yes | No | No |
| Route to NAT Gateway | Optional | Common for outbound IPv4 internet | Normally no |
| Direct internet inbound | Possible with public addressing and rules | No direct IGW path | No direct IGW path |
| Typical workload | Internet-facing load balancer | Application servers | Database tier |

**Important:** A subnet is not public merely because you name it `public-subnet`. Its route table and connectivity determine its behavior.

---

# 5. What is a Route Table?

A route table is a collection of rules that determines where network traffic is sent. Think of it as a navigation map.

Each route contains:
- Destination: the IP range traffic is intended for.
- Target: where AWS should send the traffic.

## 5.1 Local route

Every VPC route table includes a local route for communication within the VPC CIDR ranges.

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |

This allows resources in the VPC to communicate using local routing, subject to security controls.

## 5.2 Public route table

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |
| `0.0.0.0/0` | Internet Gateway |

This is commonly associated with public subnets.

## 5.3 Private route table

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |
| `0.0.0.0/0` | NAT Gateway |

This lets private IPv4 resources initiate outbound internet connections through a NAT Gateway.

## 5.4 What does `0.0.0.0/0` mean?

`0.0.0.0/0` represents all IPv4 destinations. It is commonly used as a default route.

AWS uses longest-prefix matching: the most specific matching route takes precedence.

**Remember:** Creating a route table is not enough. Associate the intended subnets with it.

Reference: [Configure route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)

---

# 6. Internet Gateway (IGW)

An Internet Gateway is a horizontally scaled, redundant VPC component that enables communication between a VPC and the internet.

It is attached to a VPC.

## 6.1 Why do we need an Internet Gateway?

Suppose you launch a web server and want users to access it from the internet. The VPC needs an Internet Gateway and an appropriate route.

For direct public IPv4 connectivity, the resource also needs a public IPv4 address or Elastic IP, along with security rules allowing the required traffic.

## 6.2 How it works

```text
                 INTERNET USER
                       |
                       v
                PUBLIC IPv4 ADDRESS
                       |
                       v
              +-------------------+
              | INTERNET GATEWAY  |
              +-------------------+
                       |
                       v
              +-------------------+
              | PUBLIC SUBNET     |
              | EC2 / Load        |
              | Balancer          |
              +-------------------+
                       |
                       v
                      VPC
```

### Steps to enable internet connectivity

1. Create a VPC.
2. Create a subnet.
3. Create an Internet Gateway.
4. Attach the Internet Gateway to the VPC.
5. Create or update a route table.
6. Add `0.0.0.0/0` pointing to the Internet Gateway.
7. Associate the route table with the intended public subnet.
8. Assign suitable public addressing to the resource.
9. Configure Security Groups and other controls.

**Important:** Attaching an Internet Gateway alone does not make every EC2 instance publicly accessible.

Reference: [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)

---

# 7. NAT Gateway

NAT stands for **Network Address Translation**.

A NAT Gateway allows resources in a private subnet to initiate connections to external networks, such as the internet, without allowing unsolicited internet-initiated connections to those private resources.

### Real-world example

A private application server may need to:
- Download operating system updates.
- Download application dependencies.
- Contact an external API.
- Retrieve packages from a public repository.

But you do not want to assign it a public IPv4 address. A NAT Gateway can provide outbound IPv4 internet connectivity.

## 7.1 NAT Gateway architecture

```text
                  INTERNET
                     |
                     v
             +---------------+
             | INTERNET      |
             | GATEWAY       |
             +---------------+
                     |
                     v
             +---------------+
             | NAT GATEWAY   |
             | Public Subnet |
             +---------------+
                     |
                     v
             +---------------+
             | PRIVATE EC2   |
             | Private Subnet|
             +---------------+
```

## 7.2 How NAT Gateway works

Suppose a private EC2 instance has private IP `10.0.11.25` and wants to access an external package repository.

1. EC2 sends traffic to its configured default route.
2. The private subnet route table directs `0.0.0.0/0` to the NAT Gateway.
3. The NAT Gateway translates the source address for outbound internet traffic.
4. Traffic leaves through the Internet Gateway.
5. The response returns through the NAT Gateway to the originating instance.

## 7.3 NAT Gateway requirements

For a public NAT Gateway:
- Create it in a public subnet.
- Allocate or associate an Elastic IP.
- Ensure the VPC has an attached Internet Gateway.
- The public subnet route table must route internet traffic to the Internet Gateway.
- Private subnet route tables must route intended outbound IPv4 traffic to the NAT Gateway.

## 7.4 NAT Gateway vs Internet Gateway

| Internet Gateway | NAT Gateway |
|---|---|
| Enables VPC internet connectivity | Provides translated outbound IPv4 connectivity for private resources |
| Attached to VPC | Created in a subnet |
| Used by public-subnet routing | Commonly used by private-subnet routing |
| Supports internet communication for suitably addressed resources | Does not allow unsolicited internet-initiated connections to private instances |

## 7.5 High availability and cost

A NAT Gateway is created in a specific Availability Zone. For resilient multi-AZ designs, a common pattern is one NAT Gateway per active AZ, with private subnets routed to the NAT Gateway in their own AZ.

NAT Gateways have hourly and data-processing charges. Data transfer charges may also apply. They can become a significant cost in a small learning environment.

For student labs:
- Check current pricing before creating one.
- Understand the architecture first.
- Delete it after the practical.
- Do not leave it running unnecessarily.

Reference: [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)

---

# 8. Security Groups

A Security Group is a **stateful virtual firewall** associated with supported resources or network interfaces. It controls inbound and outbound traffic.

Example inbound rules for a web server:

| Type | Protocol | Port | Source |
|---|---|---:|---|
| HTTP | TCP | 80 | `0.0.0.0/0` |
| HTTPS | TCP | 443 | `0.0.0.0/0` |
| SSH | TCP | 22 | Your trusted public IP `/32` |

Do not allow SSH from `0.0.0.0/0` as a normal production practice.

### Stateful behavior

If an inbound connection is allowed, response traffic for that connection is automatically allowed, even if there is no matching outbound rule specifically for that response.

Security Groups:
- Allow rules only; there are no explicit deny rules.
- Can reference other Security Groups in supported configurations.
- Are evaluated together when multiple groups are attached to a network interface.

---

# 9. Network ACL (NACL)

A Network Access Control List is a **stateless network-level firewall** associated with a subnet. It controls inbound and outbound traffic at the subnet boundary.

Example inbound rules:

| Rule number | Type | Port | Source | Action |
|---:|---|---:|---|---|
| 100 | HTTP | 80 | `0.0.0.0/0` | ALLOW |
| 110 | HTTPS | 443 | `0.0.0.0/0` | ALLOW |
| * | All traffic | All | All | DENY |

NACLs support allow and deny rules. Rules are evaluated in ascending rule-number order, and the first matching rule is applied.

### Stateless behavior

If inbound traffic is allowed, return traffic must also be allowed by outbound rules. For client connections, this commonly means allowing the required ephemeral port range for return traffic.

## 9.1 Security Group vs NACL

| Feature | Security Group | Network ACL |
|---|---|---|
| Level | Resource / network interface | Subnet |
| Stateful | Yes | No |
| Rules | Allow only | Allow and deny |
| Rule evaluation | All applicable rules | Ordered, first matching rule |
| Default behavior | Default SG allows inbound from itself and all outbound traffic | Default NACL allows all inbound and outbound traffic |
| Common use | Primary workload firewall | Additional subnet-level control |

**Best practice:** Use Security Groups as the primary access-control mechanism. Use NACLs where subnet-level stateless filtering provides a specific benefit.

Reference: [VPC security](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html)

---

# 10. Elastic IP Address (EIP)

An Elastic IP address is a static public IPv4 address allocated to your AWS account. It can be associated with supported resources such as an EC2 instance or a public NAT Gateway.

A public IPv4 address automatically assigned to an EC2 instance can change after certain lifecycle events, such as stop and start. An Elastic IP provides a stable public IPv4 address while it remains allocated and appropriately associated.

Example:

```text
EC2 Instance
Private IP: 10.0.1.10
Elastic IP:  203.0.113.25 (documentation example address)
```

**Cost warning:** Public IPv4 addresses, including Elastic IPs, are subject to AWS pricing. Check current pricing and release unused addresses.

---

# 11. Elastic Network Interface (ENI)

An Elastic Network Interface is a virtual network interface that can be attached to an EC2 instance.

It can have:
- Private IPv4 addresses.
- Public IPv4 association.
- IPv6 addresses.
- MAC address.
- Security Groups.
- Subnet association.

Example:

```text
              EC2 INSTANCE
                    |
          +---------+---------+
          |                   |
          v                   v
     +---------+         +---------+
     | ENI-1   |         | ENI-2   |
     | Primary |         | Extra   |
     +---------+         +---------+
          |                   |
          v                   v
     App Network         Management Network
```

Additional ENIs can support designs such as separating management and application traffic. Exact capabilities depend on instance type and networking configuration.

---

# 12. VPC Endpoints

A VPC Endpoint enables private connectivity between resources in a VPC and supported AWS services or endpoint services without requiring an Internet Gateway or NAT Gateway for that service path.

Example: an EC2 instance in a private subnet needs to access an S3 bucket. An S3 Gateway Endpoint can provide a private service path without sending S3 traffic through a NAT Gateway or Internet Gateway.

## 12.1 Types of VPC Endpoints

### Gateway Endpoint

Available for:
- Amazon S3
- Amazon DynamoDB

Gateway Endpoints are configured through route tables.

### Interface Endpoint

Interface Endpoints use AWS PrivateLink and create endpoint network interfaces in selected subnets. They provide private connectivity to supported AWS services and endpoint services.

| Gateway Endpoint | Interface Endpoint |
|---|---|
| S3 and DynamoDB | Many supported AWS services |
| Route-table based | Uses endpoint network interfaces |
| No hourly charge for the gateway endpoint itself | Hourly and data-processing charges generally apply |
| Not an interface endpoint | Uses AWS PrivateLink |

Reference: [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html)

---

# 13. VPC Peering

VPC Peering connects two VPCs so resources can communicate using private IP addresses, subject to routing and security configuration.

Example:
- VPC-A contains an application.
- VPC-B contains a shared service.
- The application needs to access the shared service privately.

```text
+----------------------+          +----------------------+
| VPC-A                |          | VPC-B                |
| 10.0.0.0/16          |          | 10.1.0.0/16          |
|                      |          |                      |
| Application EC2      |          | Shared Service EC2   |
+----------+-----------+          +-----------+----------+
           |                                  |
           +---------- VPC PEERING -----------+
```

Important points:
- VPC CIDR ranges must not overlap.
- The peering connection must be accepted where required.
- Routes must be configured on both sides.
- Security Groups and NACLs must allow traffic.
- VPC Peering is not transitive.

If VPC-A peers with VPC-B, and VPC-B peers with VPC-C, VPC-A cannot automatically communicate with VPC-C through VPC-B.

For larger networks, consider AWS Transit Gateway.

---

# 14. AWS Transit Gateway

AWS Transit Gateway acts as a central network hub for connecting multiple VPCs and supported external networks.

```text
                   +------------------+
                   | TRANSIT GATEWAY  |
                   +--------+---------+
                            |
          +-----------------+-----------------+
          |                 |                 |
          v                 v                 v
      +-------+         +-------+         +-------+
      | VPC-A |         | VPC-B |         | VPC-C |
      +-------+         +-------+         +-------+
```

Common use cases:
- Connecting many VPCs.
- Centralized network connectivity.
- Connecting VPCs to on-premises networks.
- Centralized inspection and routing architectures.

Transit Gateway has its own route tables. Attaching a VPC does not automatically make every other attached VPC reachable; route configuration and security still matter.

---

# 15. VPN and Direct Connect

Organizations often need to connect AWS to an office or on-premises data center.

## 15.1 AWS Site-to-Site VPN

A Site-to-Site VPN creates encrypted IPsec tunnels between your network and an AWS VPC through a supported gateway design.

Use cases:
- Connecting an office network to AWS.
- Hybrid cloud environments.
- Secure connectivity over an underlying network, commonly the internet.

## 15.2 AWS Direct Connect

AWS Direct Connect provides a dedicated network connection from your premises or colocation facility to AWS.

Use cases:
- Consistent network performance requirements.
- Large data-transfer workloads.
- Hybrid enterprise connectivity.

| Site-to-Site VPN | Direct Connect |
|---|---|
| Uses IPsec tunnels over an underlying network | Uses dedicated network connectivity |
| Can be deployed relatively quickly | Requires circuit and provider planning |
| Encrypted VPN traffic | Direct Connect itself is not encryption; add encryption where required |
| Useful for hybrid connectivity and backup | Useful for dedicated, predictable connectivity |

---

# 16. DNS in a VPC

DNS stands for Domain Name System. DNS translates domain names into IP addresses.

```text
www.example.com
        |
        v
       DNS
        |
        v
    IP Address
```

Amazon VPC provides DNS-related settings and an Amazon-provided DNS resolver.

Two important VPC DNS attributes:
- `enableDnsSupport`: enables DNS resolution through the Amazon-provided DNS server.
- `enableDnsHostnames`: controls whether instances with public IP addresses receive public DNS hostnames in the VPC, subject to AWS requirements.

DNS settings matter when using private hosted zones, service discovery and internal application names.

---

# 17. DHCP Options Set

DHCP stands for Dynamic Host Configuration Protocol.

A DHCP options set provides network configuration information to instances in a VPC. It can specify:
- Domain name.
- DNS server information.
- NTP server information.
- NetBIOS-related settings where applicable.

A VPC has a default DHCP options set. You can associate a custom one when your environment requires specific network configuration.

---

# 18. VPC Flow Logs

VPC Flow Logs capture information about IP traffic to and from network interfaces, subnets, or VPCs.

Useful for:
- Network troubleshooting.
- Security investigations.
- Traffic analysis.
- Identifying accepted and rejected traffic.
- Auditing communication patterns.

Flow Logs can publish to supported destinations such as CloudWatch Logs, Amazon S3 or Kinesis Data Firehose, depending on configuration.

**Important:** Flow Logs are not packet captures. They record metadata about network traffic, not the full contents of packets.

Example fields:
- Source IP.
- Destination IP.
- Source port.
- Destination port.
- Protocol.
- Packet and byte counts.
- ACCEPT or REJECT action.

---

# 19. IPv6 in AWS VPC

IPv6 is an IP addressing system that uses 128-bit addresses.

Example:

```text
2001:db8:1234:1::10
```

AWS VPC supports IPv4-only, IPv6-only in supported configurations, and dual-stack designs.

| IPv4 | IPv6 |
|---|---|
| 32-bit address | 128-bit address |
| Example: `10.0.1.10` | Example: `2001:db8::10` |
| Private ranges commonly use RFC 1918 | Uses IPv6 address types and prefixes |
| Default route: `0.0.0.0/0` | Default route: `::/0` |

### Egress-only Internet Gateway

For IPv6, an Egress-only Internet Gateway allows instances to initiate outbound internet communication while preventing the internet from initiating connections to those instances through that gateway.

Do not confuse it with an IPv4 NAT Gateway.

---

# 20. Default VPC vs Custom VPC

AWS accounts commonly have a default VPC in each Region, subject to account and Region configuration.

### Default VPC

A default VPC is preconfigured to make it easy to launch resources. It commonly includes:
- A default subnet in each Availability Zone.
- An Internet Gateway.
- A main route table with an internet route.
- Default Security Group and Network ACL.
- DNS configuration.

### Custom VPC

A custom VPC is designed according to workload requirements. You control:
- CIDR ranges.
- Subnets.
- Route tables.
- Gateways.
- Security Groups.
- NACLs.
- Connectivity and network architecture.

| Default VPC | Custom VPC |
|---|---|
| Preconfigured | Designed by you |
| Convenient for quick testing | Suitable for controlled architecture |
| Less initial setup | Requires network planning |
| Uses default networking choices | Supports workload-specific segmentation |

For production, teams generally plan their network design rather than relying blindly on defaults.

---

# 21. Complete Real-World VPC Architecture

Imagine a company deploying a highly available e-commerce application in one AWS Region.

### Requirements

- Customers access the application over HTTPS.
- Application servers do not have direct public IP access.
- The database is not publicly accessible.
- Application servers need outbound access for updates.
- The system operates across two Availability Zones.

### Proposed CIDR plan

VPC: `10.0.0.0/16`

| Subnet | CIDR | AZ | Purpose |
|---|---|---|---|
| Public-A | `10.0.1.0/24` | AZ-A | Load balancer and NAT |
| Public-B | `10.0.2.0/24` | AZ-B | Load balancer and NAT |
| App-Private-A | `10.0.11.0/24` | AZ-A | Application servers |
| App-Private-B | `10.0.12.0/24` | AZ-B | Application servers |
| DB-Private-A | `10.0.21.0/24` | AZ-A | Database subnet group |
| DB-Private-B | `10.0.22.0/24` | AZ-B | Database subnet group |

### Architecture

```text
                         INTERNET
                            |
                            v
                   +------------------+
                   | Internet Gateway|
                   +--------+---------+
                            |
             +--------------+--------------+
             |                             |
             v                             v
       PUBLIC SUBNET A               PUBLIC SUBNET B
       Availability Zone A           Availability Zone B
       +---------------+             +---------------+
       | Load Balancer |             | Load Balancer |
       | NAT Gateway A |             | NAT Gateway B |
       +-------+-------+             +-------+-------+
               |                             |
               v                             v
       PRIVATE APP SUBNET A          PRIVATE APP SUBNET B
       +---------------+             +---------------+
       | Application   |             | Application   |
       | EC2 / Service |             | EC2 / Service |
       +-------+-------+             +-------+-------+
               |                             |
               +--------------+--------------+
                              |
                              v
                     PRIVATE DB SUBNETS
                     +------------------+
                     | Managed Database |
                     | RDS / Multi-AZ   |
                     +------------------+

                   VPC CIDR: 10.0.0.0/16
```

### Traffic flow: customer to application

1. Customer sends an HTTPS request.
2. Request reaches the internet-facing load balancer.
3. Load balancer forwards the request to healthy application targets.
4. Application processes the request.
5. Application communicates with the database through private network paths.
6. Response returns to the customer.

### Traffic flow: application server to internet

1. Application server sends outbound traffic.
2. Private route table sends traffic to its NAT Gateway.
3. NAT Gateway sends it through the Internet Gateway.
4. Response returns through the NAT Gateway to the application server.

**Important:** NAT is for outbound connectivity; it is not the public entry point for customer requests.

---

# 22. Practical Lab – Create Your First Custom VPC

This lab creates one public subnet and one private subnet. It teaches core networking without requiring a NAT Gateway.

## Lab objective

Create:
- One VPC.
- One public subnet.
- One private subnet.
- One Internet Gateway.
- Two route tables.
- Correct route table associations.

### Lab design

| Resource | Configuration |
|---|---|
| VPC | `10.0.0.0/16` |
| Public subnet | `10.0.1.0/24` |
| Private subnet | `10.0.2.0/24` |
| Internet Gateway | Attach to VPC |
| Public route table | Local + `0.0.0.0/0` to IGW |
| Private route table | Local route only |

## Step 1: Open AWS Console

1. Sign in to your AWS account.
2. Search for VPC.
3. Open Amazon VPC.
4. Confirm the intended Region.

## Step 2: Create VPC

1. Select Your VPCs.
2. Choose Create VPC.
3. Select VPC only.
4. Name: `DevOps-Lab-VPC`.
5. IPv4 CIDR: `10.0.0.0/16`.
6. Choose Default tenancy for a normal learning lab.
7. Create VPC.

## Step 3: Create Public Subnet

1. Select Subnets.
2. Choose Create subnet.
3. Select `DevOps-Lab-VPC`.
4. Name: `Public-Subnet-A`.
5. Select an Availability Zone.
6. CIDR: `10.0.1.0/24`.
7. Create subnet.

## Step 4: Create Private Subnet

1. Select Create subnet.
2. Select the same VPC.
3. Name: `Private-Subnet-A`.
4. Select an Availability Zone.
5. CIDR: `10.0.2.0/24`.
6. Create subnet.

For this simple lab both subnets can be in one AZ. A real highly available design should use multiple AZs.

## Step 5: Create Internet Gateway

1. Select Internet Gateways.
2. Choose Create internet gateway.
3. Name: `DevOps-Lab-IGW`.
4. Create it.
5. Select the gateway.
6. Choose Actions → Attach to VPC.
7. Select `DevOps-Lab-VPC`.
8. Attach.

## Step 6: Create Public Route Table

1. Select Route Tables.
2. Choose Create route table.
3. Name: `Public-RT`.
4. Select `DevOps-Lab-VPC`.
5. Create it.
6. Open Routes and choose Edit routes.
7. Add:

```text
Destination: 0.0.0.0/0
Target: Internet Gateway
```

8. Save changes.

## Step 7: Associate Public Subnet

1. Open `Public-RT`.
2. Select Subnet associations.
3. Choose Edit subnet associations.
4. Select `Public-Subnet-A`.
5. Save.

## Step 8: Create Private Route Table

1. Create another route table.
2. Name it `Private-RT`.
3. Select `DevOps-Lab-VPC`.
4. Create it.
5. Associate `Private-Subnet-A`.

Keep only the local VPC route for this lab.

## Step 9: Enable Public IPv4 Assignment

For the public subnet:

1. Open Subnets.
2. Select `Public-Subnet-A`.
3. Edit subnet settings.
4. Enable automatic assignment of public IPv4 addresses if you want newly launched instances to receive one automatically.
5. Save.

This setting affects automatic public IPv4 assignment at launch; it does not make the subnet public by itself.

## Step 10: Verify

Check:
- VPC CIDR is correct.
- Both subnets belong to the intended VPC.
- Internet Gateway is attached.
- Public route table has the default route to the IGW.
- Public route table is associated with the public subnet.
- Private route table is associated with the private subnet.
- Private subnet has no direct internet route.

AWS Console provides a VPC Resource Map to visualize resources and relationships.

Reference: [Visualize resources in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/view-vpc-resource-map.html)

---

# 23. Practical Extension – Launch and Test EC2

Only perform this extension if you understand the security implications and possible charges.

### Public EC2 test

1. Launch an eligible EC2 instance in the public subnet.
2. Assign a public IPv4 address.
3. Attach a Security Group that permits SSH from your own trusted public IP on port 22.
4. Use a suitable key pair or approved management method.
5. Connect to the instance.
6. Test outbound connectivity if required.

Do not open SSH to everyone.

### Private EC2 test

1. Launch another EC2 instance in the private subnet.
2. Do not assign a public IPv4 address.
3. Use Systems Manager Session Manager if the instance has the required IAM role, SSM Agent and network connectivity to Systems Manager endpoints.
4. Without a NAT Gateway or appropriate VPC endpoints, do not expect the private instance to reach public internet services.

**Teaching point:** A private subnet can communicate within the VPC, but internet connectivity is a separate routing design.

---

# 24. Troubleshooting VPC Connectivity

Do not randomly change settings. Trace the network path in order.

## Problem 1: Public EC2 cannot access the internet

Check:
1. Is the Internet Gateway attached to the correct VPC?
2. Does the subnet route table contain `0.0.0.0/0 -> IGW`?
3. Is the route table associated with the correct subnet?
4. Does the instance have a public IPv4 address or Elastic IP?
5. Does the Security Group allow the required traffic?
6. Does the NACL permit traffic and return traffic?
7. Is the operating-system firewall allowing the connection?
8. Is the application listening on the expected port?

## Problem 2: Private EC2 cannot download packages

Check:
1. Is a NAT Gateway available if internet access is required?
2. Is the NAT Gateway in a public subnet?
3. Does the public subnet route to the Internet Gateway?
4. Does the private route table send traffic to the NAT Gateway?
5. Is the NAT Gateway available and correctly configured?
6. Are Security Groups and NACLs allowing traffic?
7. Is DNS resolution working?

## Problem 3: Two EC2 instances cannot communicate

Check:
1. Are both instances in the intended VPC or connected networks?
2. Do route tables provide a path?
3. Do Security Groups permit the traffic?
4. Do NACLs permit traffic in both directions?
5. Is the application listening on the expected private IP and port?
6. Does the operating-system firewall permit it?

## Problem 4: SSH connection fails

Check:
1. Does the instance have suitable public connectivity, or are you using a private management method?
2. Does the Security Group allow TCP port 22 from your trusted source?
3. Does the NACL permit inbound and return traffic?
4. Is the instance running?
5. Are you using the correct key and username?
6. Is the SSH service running?

---

# 25. VPC Security Best Practices

1. Use private subnets for application and database workloads that do not need direct internet access.
2. Do not expose databases directly to the public internet.
3. Allow only required ports and source ranges in Security Groups.
4. Restrict SSH and RDP to trusted management paths.
5. Use Systems Manager Session Manager where appropriate to reduce direct management exposure.
6. Use multiple Availability Zones for resilient application designs.
7. Use VPC Flow Logs for network visibility and troubleshooting.
8. Plan CIDR ranges to avoid overlap with corporate networks and other VPCs.
9. Use VPC endpoints when they provide a suitable private path to AWS services.
10. Review NAT Gateway and public IPv4 costs.
11. Apply least privilege to IAM roles and policies.
12. Keep infrastructure configuration under version control, but never commit secrets.

---

# 26. Important Interview Questions and Answers

**Q1. What is AWS VPC?**  
Amazon VPC is a logically isolated virtual network in AWS where you can launch resources and control IP addressing, routing and connectivity.

**Q2. What is the difference between a Region and an Availability Zone?**  
A Region is a geographic AWS area. An Availability Zone is an isolated location within a Region.

**Q3. Can a VPC span multiple Availability Zones?**  
Yes. A VPC is regional and can contain subnets in multiple AZs.

**Q4. Can one subnet span multiple Availability Zones?**  
No. A subnet belongs to one AZ.

**Q5. What is CIDR?**  
CIDR is a notation used to represent IP address ranges using a network address and prefix length.

**Q6. What is the difference between a public and private subnet?**  
A public subnet has a route to an Internet Gateway. A private subnet does not have a direct route to an Internet Gateway.

**Q7. What is an Internet Gateway?**  
An Internet Gateway is a VPC component that enables internet communication for appropriately addressed and configured resources.

**Q8. What is a NAT Gateway?**  
A NAT Gateway provides translated outbound IPv4 connectivity for private resources without allowing unsolicited inbound internet connections to those resources.

**Q9. What is the difference between Security Groups and NACLs?**  
Security Groups are stateful and operate at the resource or network-interface level. NACLs are stateless and operate at the subnet level.

**Q10. What is a route table?**  
A route table contains rules that determine where network traffic is directed.

**Q11. What is an Elastic IP?**  
An Elastic IP is a static public IPv4 address allocated to an AWS account.

**Q12. What is a VPC Endpoint?**  
A VPC Endpoint provides private connectivity between a VPC and supported AWS services or endpoint services.

**Q13. What is VPC Peering?**  
VPC Peering is a connection between two VPCs that enables private IP communication, subject to routing and security rules.

**Q14. What is Transit Gateway?**  
Transit Gateway is a central network hub used to connect multiple VPCs and supported external networks.

**Q15. What is VPC Flow Logs?**  
VPC Flow Logs capture metadata about IP traffic to and from VPC network interfaces, subnets or VPCs.

**Q16. Why do we use multiple Availability Zones?**  
To improve fault isolation and availability when application resources are deployed and configured across zones.

**Q17. Does attaching an Internet Gateway automatically make an EC2 instance public?**  
No. Routing, public addressing, Security Groups, NACLs and other configuration are also required.

**Q18. Why should a database be placed in private subnets?**  
To avoid direct internet exposure and restrict database access to authorized application or management paths.

**Q19. What is the difference between VPC Peering and Transit Gateway?**  
VPC Peering connects two VPCs directly and is non-transitive. Transit Gateway provides a central hub for connecting multiple networks.

**Q20. What is the purpose of a private subnet's NAT route?**  
It allows private IPv4 resources to initiate outbound internet connections through a NAT Gateway without assigning them public IPv4 addresses.

---

# 27. Student Assignment

## Theory questions

1. Define AWS VPC.
2. Explain Region and Availability Zone.
3. What is CIDR? Calculate total IPv4 addresses in a `/24`.
4. Explain public, private and isolated subnets.
5. What is a route table?
6. Explain Internet Gateway and NAT Gateway.
7. Differentiate Security Groups and NACLs.
8. Explain VPC Endpoints.
9. Differentiate VPC Peering and Transit Gateway.
10. Explain the three-tier VPC architecture.

## Practical tasks

- [ ] Create a custom VPC.
- [ ] Create one public subnet.
- [ ] Create one private subnet.
- [ ] Create and attach an Internet Gateway.
- [ ] Create a public route table.
- [ ] Add a default route to the Internet Gateway.
- [ ] Associate the public subnet with the public route table.
- [ ] Create a separate private route table.
- [ ] Verify subnet associations.
- [ ] Inspect the VPC Resource Map.
- [ ] Write down the complete CIDR plan.
- [ ] Delete lab resources after testing.

---

# 28. AWS VPC Quick Revision Sheet

| Component | Main responsibility |
|---|---|
| VPC | Isolated virtual network |
| Region | Geographic AWS area |
| Availability Zone | Isolated location within a Region |
| CIDR | IP address range notation |
| Subnet | IP range within a VPC and one AZ |
| Route Table | Directs network traffic |
| Internet Gateway | Enables internet connectivity |
| NAT Gateway | Outbound IPv4 internet access for private resources |
| Security Group | Stateful resource-level firewall |
| Network ACL | Stateless subnet-level firewall |
| Elastic IP | Static public IPv4 address |
| ENI | Virtual network interface |
| VPC Endpoint | Private service connectivity |
| VPC Peering | Private connectivity between two VPCs |
| Transit Gateway | Central network hub |
| Site-to-Site VPN | Encrypted network connection |
| Direct Connect | Dedicated network connection |
| Flow Logs | Network traffic metadata |
| DNS | Name resolution |
| DHCP Options | Network configuration information |

---

# 29. Official AWS Learning Resources

Use official AWS documentation for the latest service behavior, limits, console steps and pricing.

1. [Amazon VPC – Official User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)
2. [How Amazon VPC Works](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html)
3. [VPC Basics](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnet-basics.html)
4. [VPC CIDR Blocks](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-cidr-blocks.html)
5. [Subnet Sizing](https://docs.aws.amazon.com/vpc/latest/userguide/subnet-sizing.html)
6. [Route Tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
7. [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
8. [VPC Security](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html)
9. [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/create-vpc.html)
10. [VPC Resource Map](https://docs.aws.amazon.com/vpc/latest/userguide/view-vpc-resource-map.html)

---

# 30. Final Trainer Explanation

Students, remember this:

An EC2 instance is a compute resource. But a compute resource cannot work in isolation. It needs an IP address, a network, routing and security controls.

AWS VPC provides the networking foundation for deploying cloud infrastructure.

When a company builds a real application, it does not simply launch servers and expose everything to the internet. It designs a network, separates workloads, controls traffic, plans availability and monitors connectivity.

**The most important concept to master is the traffic path:**

- Where does the packet start?
- Which subnet is the resource in?
- Which route table is associated with that subnet?
- Which target does the route select?
- Do Security Groups and NACLs permit the traffic?
- Can the return traffic reach the source?

If you can answer these questions, you are beginning to think like a Cloud and DevOps Engineer.

**Next topic:** VPC Hands-on Lab – Build a Multi-AZ Network with Public and Private Subnets, Internet Gateway, NAT Gateway, EC2 and Security Groups.

---

**AWS & DevOps Training Notes**  
**Prepared by: Sahil Shaikh**  
**From Zero to Production**
