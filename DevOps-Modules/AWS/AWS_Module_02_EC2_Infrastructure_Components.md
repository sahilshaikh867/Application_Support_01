# AWS Application Support Training – Module 02
# EC2 Infrastructure Components: AMI, Launch Templates, Security Groups, EBS, Snapshots, Elastic IP, Placement Groups and ENIs

**Level:** Beginner to Intermediate  
**Module:** 02  
**Batch:** Application Support  
**Suggested duration:** 4–5 hours including practicals

## Learning Objectives

By the end of this module, students should be able to:

- Explain AMIs and create custom machine images.
- Understand and use EC2 Launch Templates.
- Configure Security Groups safely.
- Create, attach, format and mount EBS volumes.
- Create and restore EBS snapshots.
- Allocate, associate and release Elastic IP addresses.
- Explain Cluster, Spread and Partition Placement Groups.
- Understand Elastic Network Interfaces (ENIs).
- Troubleshoot common EC2 storage and network incidents.

## Prerequisites

- Basic AWS Console and EC2 knowledge.
- Ability to connect to Linux EC2 using SSH.
- Basic Linux commands: `ls`, `df`, `lsblk`, `mount`, `umount`, `systemctl`, `ip`.
- Basic understanding of IP addresses, ports and firewalls.

> **Cost and safety warning:** Use a training account and non-production resources. EC2 instances, EBS volumes, snapshots, public IPv4 addresses and other resources may incur charges. Check current regional pricing and Free Tier eligibility. Never experiment on production resources without approval, a change plan and a recovery strategy.

---

# Part 1 – AMI (Amazon Machine Image)

## 1. What is an AMI?

AMI stands for **Amazon Machine Image**.

An AMI is a template used to launch EC2 instances. It provides the information needed to start an instance, including the operating system image and root storage configuration.

In simple language, an AMI is a prepared server blueprint. Instead of installing and configuring an operating system from the beginning every time, you can launch an EC2 instance using an existing AMI.

### Real-life example

Imagine a company has configured a Linux server with:

- Amazon Linux operating system.
- Required security updates.
- Nginx web server.
- Monitoring agent.
- Standard application directories.
- Approved configuration files.

The company needs five more servers with the same baseline. Instead of repeating the setup manually, the team can create an AMI from the prepared instance and launch additional instances from that AMI.

### Basic workflow

1. Launch an EC2 instance.
2. Install and configure required software.
3. Validate the server.
4. Create an AMI.
5. Launch new EC2 instances using that AMI.
6. Validate the new instances and their application configuration.

## 2. Why do we use AMIs?

1. **Server standardization:** Launch servers with a consistent OS and baseline configuration.
2. **Faster deployment:** Reduce repeated installation and setup work.
3. **Recovery:** Use an AMI as one component of a recovery strategy.
4. **Scaling:** Launch additional instances from a prepared image.
5. **Environment replication:** Build similar development, testing and production servers.
6. **Application deployment:** Prepare an image containing application dependencies.

An AMI alone does not make an application highly available. Availability also depends on architecture, networking, databases, load balancing, backups and operations.

## 3. AMI sources and types

| Source | Explanation | Example |
|---|---|---|
| AWS-provided AMI | Published by AWS | Amazon Linux 2023 |
| AWS Marketplace AMI | Published by a Marketplace seller | Vendor software image |
| Community AMI | Shared publicly by another publisher | Community-created image |
| Custom AMI | Created by an organization from its configured instance | Company web-server baseline |

**Security note:** Prefer trusted publishers. Review image age, security updates, Marketplace charges, licensing and publisher details.

## 4. What information does an AMI contain?

An AMI contains or references information such as:

- Root volume image.
- Block device mappings for EBS or instance-store devices.
- Launch permissions.
- Architecture, such as `x86_64` or `arm64`.
- Virtualization and boot information.

For an EBS-backed AMI, the image data is backed by EBS snapshots.

## 5. AMI vs EC2 Instance vs EBS Snapshot

| Item | Meaning | Main purpose |
|---|---|---|
| AMI | EC2 launch image definition | Launch an EC2 instance |
| EC2 instance | Virtual server | Run OS and applications |
| EBS snapshot | Point-in-time volume backup | Restore or copy volume data |

An EBS-backed AMI normally references snapshots for its backing volume data. Creating an AMI can therefore result in snapshot storage charges.

## 6. Practical 1 – Create an AMI from an EC2 instance

**Objective:** Create a reusable image from an existing training EC2 instance.

### Before starting

- Use a non-production instance.
- Confirm that it contains no sensitive information.
- Record the instance name and ID.
- Check whether it has additional EBS volumes.
- Understand application-consistency requirements before imaging.

### Console steps

1. Sign in to the AWS Management Console.
2. Open **EC2 → Instances**.
3. Select your training instance.
4. Choose **Actions → Image and templates → Create image**.
5. Enter the image name:

   `app-support-nginx-golden-image`

6. Add a description:

   `Training AMI containing Amazon Linux 2023 and Nginx configuration`

7. Review the volume settings.
8. Choose whether to reboot the instance according to consistency requirements.
9. Select **Create image**.
10. Open **EC2 → AMIs**.
11. Monitor the AMI state until it becomes `Available`.

### Why does the reboot option matter?

If applications are actively writing data while an image is created, the image may not be application-consistent. Rebooting can help achieve a cleaner filesystem state, but causes downtime.

For databases and critical applications, use application-aware backup procedures and vendor guidance. Do not assume a normal AMI creation is automatically a fully application-consistent backup.

### Verification

Check:

- AMI name and ID.
- AMI state.
- Source instance.
- Block device mappings.
- Snapshot IDs associated with the image.

## 7. Practical 2 – Launch an instance from a custom AMI

1. Open **EC2 → AMIs**.
2. Select your custom AMI.
3. Choose **Launch instance from AMI**.
4. Enter a new name:

   `app-support-nginx-copy`

5. Select an appropriate instance type.
6. Select or create a suitable key pair.
7. Configure VPC, subnet and Security Group.
8. Review storage settings.
9. Launch the instance.
10. Wait for the instance and status checks.
11. Connect to the new instance.
12. Verify that the expected OS and software are present.

Linux verification:

```bash
cat /etc/os-release
```

Check Nginx:

```bash
sudo systemctl status nginx
```

Test the local web server:

```bash
curl -I http://localhost
```

### Things to check after launching from an AMI

- Hostname and machine identity.
- SSH host keys and access configuration.
- Application configuration.
- Environment-specific values.
- Monitoring and log-agent registration.
- Secrets and credentials.
- Static IP assumptions.
- Domain names and certificates.
- Dependencies on the original server.

Do not bake long-lived secrets or unique machine credentials into a reusable AMI.

---

# Part 2 – EC2 Launch Templates

## 8. What is an EC2 Launch Template?

A **Launch Template** is a reusable configuration that defines how EC2 instances should be launched.

It can specify:

- AMI ID.
- Instance type.
- Key pair.
- Security Groups.
- Network interface settings.
- EBS volume mappings.
- User data.
- IAM instance profile.
- Tags and other launch settings.

In simple language:

An AMI is the server image. A Launch Template is the reusable set of instructions describing how to launch an instance using that image and other configuration.

### Real-life example

A company needs ten identical web servers. Without a Launch Template, an engineer might manually select the AMI, instance type, Security Group and storage settings for every server. This can introduce differences.

With a Launch Template, the team defines the standard configuration once and reuses it.

## 9. AMI vs Launch Template

| AMI | Launch Template |
|---|---|
| Provides the machine image | Provides launch configuration |
| Contains or references OS and image data | References an AMI and other settings |
| Used to create instances | Used to create instances consistently |
| Does not define the entire launch process | Can define networking, storage and instance type |

**Remember:** A Launch Template does not replace an AMI. A Launch Template normally references an AMI.

## 10. Launch Template versions

Launch Templates support versions.

Example:

- Version 1: Amazon Linux 2023 and `t3.micro`.
- Version 2: Updated AMI ID and revised user data.
- Version 3: Updated Security Group configuration.

A team can create a new version and choose which version to use for future launches or an Auto Scaling Group.

Creating a new version does not automatically update already-running instances. Existing instances generally need a separate update or replacement process.

## 11. Practical – Create a Launch Template

**Objective:** Create a reusable EC2 launch configuration for a training web server.

### Console steps

1. Open **EC2**.
2. Select **Launch Templates**.
3. Click **Create launch template**.
4. Enter:

   `application-support-web-template`

5. Add description:

   `Standard training web server launch configuration`

6. Select the Amazon Linux 2023 AMI.
7. Select an appropriate instance type, such as `t3.micro`, after checking availability and pricing.
8. Select a suitable key pair.
9. Configure the Security Group.
10. Configure the EBS root volume.
11. Add tags:
    - Key: `Project`
    - Value: `ApplicationSupportTraining`
12. Review the configuration.
13. Click **Create launch template**.

### Launch an instance from the template

1. Open **Launch Templates**.
2. Select the template.
3. Choose **Actions → Launch instance from template**.
4. Select the required template version.
5. Review the configuration.
6. Launch the instance.
7. Verify the instance configuration.

### Why do companies use Launch Templates?

- Standardized deployments.
- Repeatable infrastructure.
- Auto Scaling integration.
- Version-controlled launch configuration.
- Reduced manual configuration errors.

---

# Part 3 – Security Groups

## 12. What is a Security Group?

A **Security Group** is a virtual firewall associated with resources such as EC2 instances and network interfaces.

It controls allowed inbound and outbound network traffic.

In simple language, a Security Group decides which network connections are allowed to reach your server and which outbound connections are allowed from it.

### Real-life example

Suppose an EC2 instance hosts a public website. You want:

- Customers to access the website using HTTP or HTTPS.
- Administrators to connect using SSH.
- Other unwanted connections to be blocked.

You can create Security Group rules for these requirements.

## 13. Inbound rules

Inbound rules control traffic entering the resource.

Example:

| Type | Protocol | Port | Source | Purpose |
|---|---|---:|---|---|
| SSH | TCP | 22 | Your trusted public IP `/32` | Linux administration |
| HTTP | TCP | 80 | `0.0.0.0/0` | Public website |
| HTTPS | TCP | 443 | `0.0.0.0/0` | Secure public website |

## 14. Outbound rules

Outbound rules control traffic leaving the resource.

A new Security Group commonly starts with an allow-all outbound rule. Organizations may restrict outbound traffic according to application requirements.

Example:

| Type | Protocol | Port | Destination | Purpose |
|---|---|---:|---|---|
| HTTPS | TCP | 443 | Approved destination ranges | External APIs or repositories |
| DNS | UDP/TCP | 53 | Approved DNS resolvers | Name resolution |

Do not blindly remove outbound rules. This can break package installation, API calls, DNS resolution, monitoring and application dependencies.

## 15. Security Group sources and destinations

A rule can use:

- IPv4 CIDR range.
- IPv6 CIDR range.
- Another Security Group as a reference.
- Supported prefix lists.

Example CIDR:

`203.0.113.10/32`

`/32` means one IPv4 address. The address above is a documentation example, not a real administrator IP.

## 16. Stateful behaviour

Security Groups are **stateful**.

If inbound traffic is allowed and the resource sends a response to that connection, the response traffic is automatically allowed, even if there is no separate outbound rule matching the response.

Security Groups do not support explicit deny rules. They allow traffic that matches their rules; other traffic is not allowed.

## 17. Security Group vs Network ACL

| Security Group | Network ACL |
|---|---|
| Applied to an EC2 network interface | Applied at subnet level |
| Stateful | Stateless |
| Supports allow rules only | Supports allow and deny rules |
| Return traffic is automatically allowed for tracked connections | Return traffic rules must be configured separately |
| Rules are evaluated together as permissions | Rules are evaluated in rule-number order |

### Troubleshooting example

A user cannot access an EC2-hosted website. An Application Support Engineer should investigate:

1. Is the EC2 instance running?
2. Is the web server running?
3. Is the application listening on the expected port?
4. Does the Security Group allow the port?
5. Does the Network ACL allow required traffic?
6. Is the route to the internet correct?
7. Does the instance have a suitable public IP or another valid access path?
8. Is the operating-system firewall blocking traffic?

## 18. Practical – Configure a Security Group for a web server

**Objective:** Allow website traffic while restricting SSH access.

### Console steps

1. Open **EC2 → Security Groups**.
2. Click **Create security group**.
3. Name:

   `app-support-web-sg`

4. Description:

   `Security Group for training Nginx web server`

5. Select the correct VPC.
6. Add inbound rules:

| Type | Port | Source |
|---|---:|---|
| SSH | 22 | My IP |
| HTTP | 80 | Anywhere IPv4 (`0.0.0.0/0`) |

7. Keep or configure outbound rules according to the lab requirement.
8. Create the Security Group.
9. Attach it to the training EC2 instance through the instance's network settings.
10. Test SSH and HTTP access.

### Verification

Browser:

`http://YOUR_PUBLIC_IP`

On EC2:

```bash
sudo ss -tulnp
```

```bash
sudo systemctl status nginx
```

### Security Group troubleshooting

If SSH fails:

- Check the SSH inbound rule.
- Confirm the source IP is correct.
- Check the key pair and username.
- Check the instance's route and public IP.
- Check instance state and status checks.

If HTTP fails:

- Check the HTTP inbound rule.
- Check whether Nginx is running.
- Check whether Nginx listens on port 80.
- Check Network ACLs and routing.
- Check the operating-system firewall.

### Best practices

- Do not expose SSH to `0.0.0.0/0` unless specifically approved.
- Use the smallest necessary source CIDR.
- Use Security Group references for communication between application tiers where appropriate.
- Avoid opening all ports.
- Review unused rules and Security Groups.
- Apply meaningful names and tags.
- Document why each rule exists.

---

# Part 4 – EBS (Elastic Block Store)

## 19. What is EBS?

**Amazon Elastic Block Store (EBS)** provides block storage volumes for EC2 instances.

In simple language, EBS is like a virtual hard disk that can be attached to an EC2 instance.

It stores:

- Operating system files.
- Application files.
- Database files.
- Logs.
- Uploaded content.
- Other persistent application data.

### Real-life example

Imagine an EC2 instance is a computer:

- EC2 is the computer.
- EBS is its attached hard disk.
- The operating system uses the disk to store files and applications.

EBS volumes are designed to persist independently of the running life of an EC2 instance, subject to volume deletion settings and lifecycle actions.

## 20. Why do we use EBS?

1. Persistent block storage.
2. Expandable storage capacity.
3. Different performance and cost options.
4. Data backup through snapshots.
5. Ability to detach and attach volumes where supported.
6. Storage for application and database workloads.

## 21. EBS Volume Types

The main EBS volume families include General Purpose SSD, Provisioned IOPS SSD, Throughput Optimized HDD and Cold HDD.

| Volume type | Typical use |
|---|---|
| `gp3` | General-purpose SSD workloads |
| `gp2` | Previous-generation general-purpose SSD |
| `io2` | High-performance, IOPS-intensive workloads |
| `io1` | Previous-generation Provisioned IOPS SSD |
| `st1` | Throughput-intensive HDD workloads |
| `sc1` | Infrequently accessed, throughput-oriented HDD workloads |

Availability, limits and performance depend on Region and EC2 instance.

**Important:** HDD volume types such as `st1` and `sc1` cannot be used as boot volumes.

For a beginner web-server practical, a small `gp3` volume is generally suitable. Always check current pricing and performance requirements.

## 22. Important EBS concepts

### 22.1 Volume size

Storage capacity, measured in GiB. Example: an 8 GiB root volume and a separate 10 GiB data volume.

### 22.2 Root volume

The volume containing the operating system's root filesystem.

### 22.3 Data volume

An additional volume used for application files, logs, uploads or other data.

### 22.4 Attachment

An EBS volume is attached to an EC2 instance in the same Availability Zone.

### 22.5 Formatting and mounting

A new empty Linux data volume usually needs a filesystem before it can be mounted.

Common filesystems include ext4 and XFS.

> **Critical warning:** Formatting a device creates a filesystem and can destroy existing data. Verify the exact device and confirm it is empty before running `mkfs`. Never copy a command blindly from a lab into production.

## 23. Practical – Create and attach an EBS volume

**Objective:** Create an additional EBS data volume, attach it to a Linux EC2 instance, format it, mount it and verify storage.

### Before starting

- Use a non-production Linux EC2 instance.
- Confirm the instance's Availability Zone.
- Create the EBS volume in the same Availability Zone.
- Ensure the volume is new and contains no required data.
- Check the expected device using Linux commands.

### Step 1 – Check existing disks

Connect to EC2:

```bash
lsblk
```

List mounted filesystems:

```bash
df -hT
```

Record the output before attaching the new volume.

### Step 2 – Create an EBS volume

1. Open **EC2 → Volumes**.
2. Click **Create volume**.
3. Select a volume type such as `gp3`.
4. Enter a small training size, for example `10 GiB`.
5. Select the same Availability Zone as the EC2 instance.
6. Enable encryption.
7. Add a tag:
   - Key: `Name`
   - Value: `app-support-data-volume`
8. Create the volume.

### Step 3 – Attach the volume

1. Select the new volume.
2. Choose **Actions → Attach volume**.
3. Select the training EC2 instance.
4. Review the device name.
5. Attach the volume.

### Step 4 – Identify the new device

On EC2:

```bash
lsblk
```

Example output (device names vary):

```text
NAME          MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
nvme0n1       259:0    0   8G  0 disk
└─nvme0n1p1   259:1    0   8G  0 part /
nvme1n1       259:2    0  10G  0 disk
```

On Nitro-based instances, EBS volumes commonly appear as NVMe devices. The Linux device name may differ from the name shown in the EC2 console.

**Do not assume the new device is always `/dev/nvme1n1`.** Compare `lsblk` output before and after attachment, and verify size and mount status.

### Step 5 – Check whether the device contains a filesystem

Replace the example device with the actual verified device:

```bash
sudo file -s /dev/nvme1n1
```

Also inspect:

```bash
sudo lsblk -f
```

If the volume is new and empty, it may show no filesystem.

If it contains a filesystem or data, do not format it. Investigate first.

### Step 6 – Create a filesystem on a confirmed empty volume

**Only for a confirmed empty training volume:**

```bash
sudo mkfs -t xfs /dev/nvme1n1
```

Alternatively, use ext4 if required:

```bash
sudo mkfs -t ext4 /dev/nvme1n1
```

Do not run both commands on the same volume as part of this exercise.

### Step 7 – Create a mount directory

```bash
sudo mkdir -p /data
```

### Step 8 – Mount the volume

```bash
sudo mount /dev/nvme1n1 /data
```

### Step 9 – Verify the mount

```bash
df -hT
```

```bash
lsblk -f
```

The volume should now appear mounted at `/data`.

### Step 10 – Create a test file

```bash
sudo touch /data/training-test.txt
```

Write content:

```bash
echo "EBS volume practical completed" | sudo tee /data/training-test.txt
```

Read the file:

```bash
cat /data/training-test.txt
```

### Step 11 – Configure persistent mounting

A manual mount may not automatically return after a reboot. Configure persistent mounting using the filesystem UUID in `/etc/fstab`.

Get the UUID:

```bash
sudo blkid /dev/nvme1n1
```

Example output:

```text
/dev/nvme1n1: UUID="example-uuid" TYPE="xfs"
```

Back up the current fstab file:

```bash
sudo cp /etc/fstab /etc/fstab.backup
```

Edit the file:

```bash
sudo nano /etc/fstab
```

Add a line using the actual UUID:

```text
UUID=YOUR_ACTUAL_UUID /data xfs defaults,nofail 0 2
```

For ext4, use `ext4` instead of `xfs`.

Validate:

```bash
sudo mount -a
```

Check:

```bash
findmnt /data
```

**Important:** A wrong `/etc/fstab` entry can cause boot or mount problems. Use the correct UUID and filesystem type. The `nofail` option helps avoid making boot depend on this non-root data volume, but does not replace validation.

### Step 12 – Change directory ownership if required

For a simple lab using the default Amazon Linux user:

```bash
sudo chown ec2-user:ec2-user /data
```

Verify:

```bash
ls -ld /data
```

## 24. Detach and reattach an EBS volume

### Safe detach procedure

1. Stop applications writing to the volume.
2. Flush pending writes where appropriate.
3. Unmount the filesystem.
4. Detach the volume from the EC2 console.
5. Confirm the volume state.
6. Attach it to another compatible instance in the same Availability Zone.
7. Inspect the filesystem and mount it.

Unmount:

```bash
sudo umount /data
```

Verify:

```bash
findmnt /data
```

If the volume is busy, identify processes:

```bash
sudo fuser -vm /data
```

Do not force-detach a volume without understanding the risk of filesystem or application corruption.

### Important points

- EBS volumes are Availability Zone-specific.
- A volume can be detached and reattached subject to service and attachment constraints.
- Some volume types support Multi-Attach, but it has specific requirements. It does not make a normal filesystem safe for simultaneous writes from multiple instances.
- Do not detach a root volume from a running instance as a normal troubleshooting step.

---

# Part 5 – EBS Snapshots

## 25. What is an EBS Snapshot?

An EBS snapshot is a point-in-time backup of an EBS volume.

In simple language, a snapshot captures the volume's data at a particular point in time so that you can use it to create a new volume or support recovery.

### Real-life example

Suppose an application stores uploaded documents on an EBS data volume. Before a major application change, the operations team creates a snapshot.

If the volume is damaged or data must be recovered, the team can create a new EBS volume from the snapshot and restore the application data.

### Snapshot uses

1. Backup and recovery.
2. Data migration.
3. Creating new EBS volumes.
4. Creating AMIs.
5. Copying backup data across Regions.
6. Supporting disaster recovery.

## 26. How snapshots work

EBS snapshots are stored and managed by AWS. They are point-in-time backups, and subsequent snapshots can use incremental storage.

An incremental snapshot stores blocks that have changed since the previous snapshot for that volume.

Deleting a snapshot does not mean all data it once referenced is necessarily immediately removed, because other snapshots may depend on the same underlying data blocks.

## 27. Snapshot vs AMI vs EBS Volume

| Feature | EBS Volume | EBS Snapshot | AMI |
|---|---|---|---|
| Purpose | Active block storage | Point-in-time backup | Launch an EC2 instance |
| Can be attached to EC2 | Yes | No, not directly | No, not directly |
| Main use | Store and access data | Backup and restore | Standardized instance launch |

## 28. Practical – Create an EBS Snapshot

**Objective:** Create a snapshot of the training data volume.

### Console steps

1. Open **EC2 → Volumes**.
2. Select the training EBS data volume.
3. Choose **Actions → Create snapshot**.
4. Add description:

   `Application Support Training – Data Volume Backup`

5. Add tag:
   - Key: `Name`
   - Value: `app-support-data-snapshot`
6. Create the snapshot.
7. Open **Snapshots**.
8. Monitor progress until completed.

### Consistency considerations

A snapshot captures a point-in-time view of the volume, but is not automatically application-consistent.

For a simple training data volume, demonstrate with a quiesced or unmounted filesystem.

For production:

- Follow application-specific backup instructions.
- Quiesce writes or stop services where appropriate.
- Coordinate database backup procedures.
- Consider multi-volume snapshot requirements.
- Test recovery regularly.

## 29. Practical – Restore a volume from a snapshot

1. Open **EC2 → Snapshots**.
2. Select the completed snapshot.
3. Choose **Actions → Create volume from snapshot**.
4. Select the Availability Zone where the target instance runs.
5. Select an appropriate volume type.
6. Configure size and encryption as required.
7. Create the volume.
8. Attach the new volume to a suitable EC2 instance.
9. Identify the new device using `lsblk`.
10. Inspect the filesystem before mounting.

If the snapshot came from a formatted filesystem, do not format the restored volume again.

Example:

```bash
lsblk -f
```

Create a mount point:

```bash
sudo mkdir -p /restore
```

Mount the restored filesystem using its verified device:

```bash
sudo mount /dev/nvme2n1 /restore
```

The device name is an example. Use the actual device identified on your instance.

Verify restored files:

```bash
ls -lah /restore
```

If the restored volume contains a filesystem UUID that conflicts with another attached volume, investigate the duplicate UUID before mounting and update configuration safely.

## 30. Snapshot best practices

- Create snapshots before major changes where appropriate.
- Use clear names, descriptions and tags.
- Define a backup retention policy.
- Monitor snapshot costs.
- Test restoration, not just snapshot creation.
- Restrict snapshot sharing permissions.
- Encrypt snapshots containing sensitive data.
- Use AWS Backup or an approved backup process for centralized backup management.
- Copy snapshots to another Region for cross-Region recovery where required.

---

# Part 6 – Elastic IP Address

## 31. What is an Elastic IP?

An **Elastic IP address (EIP)** is a static public IPv4 address that you allocate to your AWS account and can associate with supported resources such as an EC2 instance or network interface.

In simple language, a normal auto-assigned public IPv4 address may change when an instance is stopped and started. An Elastic IP provides a persistent public IPv4 address that can be reassociated with another supported resource.

### Real-life example

Suppose a company has configured a DNS record that points to an EC2 public IP address.

If the instance is stopped and started and its auto-assigned public IP changes, the DNS record may point to the old address.

An Elastic IP can provide a stable IPv4 endpoint for that design.

For production applications, DNS, load balancers and resilient architecture are usually preferable to relying on a single EC2 instance address.

## 32. Why use Elastic IP?

- Stable public IPv4 address.
- Reassociation to another supported resource.
- Useful for some legacy integrations that require an allowlisted IP.
- Useful for specific failover designs.
- Provides a stable address for DNS configuration.

## 33. Elastic IP – Important points

- Elastic IPs are public IPv4 addresses.
- They are allocated in a Region.
- They can be associated with supported resources.
- They are not a replacement for load balancing or high availability.
- Public IPv4 addresses, including Elastic IPs, can incur charges under current AWS pricing.
- An allocated but unused Elastic IP can still incur charges.
- Review current pricing before allocation.

## 34. Practical – Allocate and associate an Elastic IP

**Objective:** Allocate an Elastic IP and associate it with the training EC2 instance.

### Console steps

1. Open **EC2 → Elastic IPs** under Network & Security.
2. Click **Allocate Elastic IP address**.
3. Select the appropriate network border group and settings.
4. Allocate the address.
5. Select the newly allocated Elastic IP.
6. Choose **Actions → Associate Elastic IP address**.
7. Select resource type, such as Instance.
8. Select the training EC2 instance.
9. Select the appropriate private IP if prompted.
10. Associate the address.

### Verification

Record the Elastic IP shown in the console.

Open:

`http://YOUR_ELASTIC_IP`

The Nginx website should load if the Security Group, route, instance and web server are configured correctly.

### Disassociate and release

When the training is complete:

1. Select the Elastic IP.
2. Choose **Actions → Disassociate Elastic IP address** if associated.
3. Choose **Actions → Release Elastic IP addresses**.
4. Confirm release.

**Important:** Releasing an Elastic IP returns it to AWS's pool. You generally cannot request the same address again. Do not release an address used by a production DNS record or allowlist.

---

# Part 7 – EC2 Placement Groups

## 35. What is a Placement Group?

An EC2 Placement Group is a logical grouping used to influence how EC2 instances are placed on the underlying AWS infrastructure.

In simple language, Placement Groups help determine how instances are positioned in relation to one another for specific performance, availability or fault-isolation requirements.

AWS provides three main placement strategies:

1. Cluster.
2. Spread.
3. Partition.

## 36. Cluster Placement Group

A Cluster Placement Group places instances close together in a single Availability Zone to support workloads that need high network performance and low latency between instances.

### Real-life example

A high-performance computing application has multiple EC2 instances that frequently exchange large amounts of data.

The team may use a Cluster Placement Group to improve network performance between the instances.

### Advantages

- Low-latency communication potential.
- High network throughput potential.
- Useful for tightly coupled workloads.

### Considerations

- Instances are placed in one Availability Zone.
- Capacity and instance-type constraints apply.
- It does not provide multi-AZ resilience by itself.
- A failure affecting the placement area can affect multiple instances.

## 37. Spread Placement Group

A Spread Placement Group places a small number of instances across distinct underlying hardware to reduce the chance that one hardware failure affects all instances.

### Real-life example

A company runs three critical application servers. It wants to reduce the chance that all three instances are affected by one underlying hardware failure.

A Spread Placement Group can help isolate instances.

### Advantages

- Reduced correlated hardware failure risk.
- Useful for a small number of critical instances.
- Supports fault isolation.

### Considerations

- There are limits on the number of instances per Availability Zone per placement group.
- Availability Zone placement still matters.
- It does not replace application-level redundancy or backups.

## 38. Partition Placement Group

A Partition Placement Group divides instances into logical partitions. Each partition is placed on separate sets of racks, with partition-level isolation.

### Real-life example

A distributed big-data application runs multiple nodes. The team can distribute nodes across partitions so that a hardware failure affecting one partition is less likely to affect every partition.

### Advantages

- Supports large distributed workloads.
- Provides partition-level fault isolation.
- Useful for distributed systems such as Hadoop and Kafka where supported by workload design.

### Considerations

- Applications should understand partition placement and failure domains.
- It does not automatically distribute application data or replicate it.
- Instance placement and service limits apply.

## 39. Placement Group comparison

| Strategy | Main goal | Example workload |
|---|---|---|
| Cluster | Low latency and high network throughput | HPC and tightly coupled applications |
| Spread | Separate instances across distinct hardware | Critical individual instances |
| Partition | Separate groups of instances into partitions | Distributed big-data systems |

**Interview point:** Placement Groups influence infrastructure placement. They do not replace load balancing, multi-AZ architecture, database replication, backups, disaster recovery or application-level fault tolerance.

## 40. Practical – Explore Placement Groups

For a beginner training session, first explore the console and understand the configuration rather than launching expensive or specialized instances.

1. Open **EC2 → Placement Groups**.
2. Click **Create placement group**.
3. Enter:

   `app-support-training-spread`

4. Select the Spread strategy.
5. Review supported settings.
6. Create the group if you intend to use it.
7. Understand how an instance is launched with the placement group selected.

For an actual placement demonstration:

1. Check supported instance types and placement constraints.
2. Check Availability Zone support and capacity.
3. Select the placement group during instance launch or configure the supported placement setting.
4. Launch only the small number of instances required for the lab.
5. Verify the placement group association in instance details.
6. Terminate unneeded instances and remove unused placement groups when no longer required.

Do not launch several instances merely to demonstrate a placement strategy without checking cost and capacity.

---

# Part 8 – Elastic Network Interface (ENI)

## 41. What is an Elastic Network Interface?

An **Elastic Network Interface (ENI)** is a virtual network interface that can be attached to an EC2 instance in a VPC.

In simple language, an ENI is like a network card in a physical computer. It allows an EC2 instance to communicate with other resources and networks.

### Real-life example

A physical server may have multiple network cards for different networks.

Similarly, an EC2 instance can have one or more network interfaces, depending on its instance type and networking requirements.

An ENI can include:

- A primary private IPv4 address.
- One or more secondary private IPv4 addresses.
- One or more associated Elastic IP addresses, subject to service limits.
- A MAC address.
- One or more Security Groups.
- A subnet association.
- Attachment information.

## 42. Primary and secondary network interfaces

### Primary network interface

Every EC2 instance has a primary network interface, commonly identified as `eth0` inside Linux, although interface names can vary.

The primary interface cannot normally be detached from a running instance.

### Secondary network interface

An additional ENI can be created and attached to an instance where supported.

It can be useful for:

- Separating network traffic.
- Network appliances.
- Multi-homed applications.
- Different Security Group associations.
- Certain failover and recovery designs.

An ENI is associated with a subnet in one Availability Zone. It can be moved between compatible instances in the same Availability Zone, subject to attachment and instance limits.

## 43. Why use ENIs?

1. Multiple network interfaces.
2. Multiple private IP addresses.
3. Different Security Groups on different interfaces.
4. Network separation for supported designs.
5. Moving a secondary interface between instances for certain recovery patterns.
6. Network appliance and specialized architecture designs.

## 44. ENI vs Public IP vs Elastic IP

| Item | Meaning |
|---|---|
| ENI | Virtual network interface attached to an EC2 instance |
| Private IP | Internal address assigned to a network interface |
| Auto-assigned public IP | Public IPv4 address that may change with instance lifecycle |
| Elastic IP | Allocated static public IPv4 address associated with a supported resource |

A public IP address is not itself a network interface.

## 45. Practical – Explore the EC2 Network Interface

### Step 1 – Open Network Interfaces

1. Open EC2 Console.
2. Select **Network Interfaces** under Network & Security.
3. Locate the interface associated with your training instance.
4. Open its details.

### Step 2 – Understand the details

Review:

- Network Interface ID.
- Interface status.
- VPC ID.
- Subnet ID.
- Availability Zone.
- Private IPv4 address.
- Public IPv4 address, if assigned.
- MAC address.
- Security Groups.
- Attachment information.

### Step 3 – Inspect from Linux

Connect to EC2 and run:

```bash
ip addr
```

This displays network interfaces and their addresses.

Check routes:

```bash
ip route
```

Check DNS resolution:

```bash
getent hosts aws.amazon.com
```

Check outbound HTTPS connectivity:

```bash
curl -I https://aws.amazon.com
```

The last command can fail if outbound rules, routing, DNS or other network controls are not configured correctly.

### Step 4 – Create a secondary ENI (optional advanced demonstration)

Only perform this if the instance type and lab design support it.

1. Open **EC2 → Network Interfaces**.
2. Choose **Create network interface**.
3. Select a subnet in the same Availability Zone as the target EC2 instance.
4. Configure a description.
5. Assign the required Security Group or Groups.
6. Create the interface.
7. Select it and choose **Attach**.
8. Select the compatible EC2 instance.
9. Complete the attachment.
10. Verify the interface from the EC2 console and operating system.

**Important:** A secondary ENI does not automatically configure application routing, source/destination behaviour or operating-system network settings. Those require deliberate configuration.

Do not detach the primary network interface as part of this exercise.

---

# Part 9 – How These Services Work Together

## 46. Example: A company's standard web-server environment

The services can work together as follows:

1. **AMI:** Provides the prepared operating-system and software image.
2. **Launch Template:** Defines how the EC2 instance should be launched.
3. **EC2:** Provides the virtual server.
4. **Security Group:** Controls allowed network traffic.
5. **EBS:** Provides persistent block storage.
6. **EBS Snapshot:** Provides a point-in-time backup of the volume.
7. **Elastic IP:** Provides a stable public IPv4 address when required.
8. **Placement Group:** Influences instance placement for a specific performance or fault-isolation goal.
9. **ENI:** Provides the instance's virtual network connectivity.

A simple training web server may use:

- An Amazon Linux AMI.
- A Launch Template for repeatable deployment.
- An EC2 instance.
- A Security Group allowing HTTP and restricted SSH.
- An EBS root volume and a separate data volume.
- An EBS snapshot for the data volume.
- An Elastic IP only if a stable public IPv4 address is required.
- A primary ENI for network connectivity.

A Placement Group is optional and is not normally needed for one small web server.

---

# Part 10 – Application Support Troubleshooting Scenarios

## 47. Scenario 1 – EC2 instance launch fails

**User complaint:** "The EC2 instance is not launching."

Investigate:

1. Is the selected AMI available in the Region?
2. Is the instance type supported in the selected Availability Zone?
3. Is there sufficient capacity?
4. Are account quotas or service quotas reached?
5. Is the subnet and VPC configuration valid?
6. Are permissions sufficient?
7. Is the Launch Template using the correct version?

Record the exact AWS error message and request ID where available.

## 48. Scenario 2 – Website is not accessible

**User complaint:** "The website is not opening."

Investigate in this order:

1. Is the EC2 instance running?
2. Are EC2 status checks passing?
3. Does the instance have a valid network path?
4. Does the Security Group allow TCP port 80 or 443?
5. Do Network ACLs allow the traffic and return traffic?
6. Is Nginx or the application service running?
7. Is the service listening on the expected port?
8. Is the operating-system firewall blocking the connection?
9. Is the correct public IP or DNS name being used?

Commands:

```bash
sudo systemctl status nginx
```

```bash
sudo ss -tulnp
```

```bash
curl -I http://localhost
```

## 49. Scenario 3 – EBS volume is not visible

Investigate:

1. Is the EBS volume in the expected state?
2. Is it attached to the correct instance?
3. Is it in the same Availability Zone?
4. Does the instance support another volume attachment?
5. Run `lsblk`.
6. Run `sudo dmesg | tail -50`.
7. Inspect EC2 console attachment details.

Commands:

```bash
lsblk
```

```bash
sudo lsblk -f
```

```bash
sudo dmesg | tail -50
```

Do not format the volume to make it appear. Formatting does not solve an attachment or device-detection problem and can destroy data.

## 50. Scenario 4 – Disk space is full

**User complaint:** "The application is failing because the server has no disk space."

Check:

```bash
df -h
```

Identify large directories:

```bash
sudo du -xhd1 /var 2>/dev/null | sort -h
```

Check logs:

```bash
sudo du -sh /var/log/*
```

Potential actions:

- Identify files consuming space.
- Check log rotation.
- Check application temporary files.
- Review retention requirements.
- Expand the EBS volume if needed.
- Grow the partition and filesystem where applicable.
- Follow the organization's change process.

Never delete logs or application data blindly. Logs may be needed for incident investigation, audits or compliance.

## 51. Scenario 5 – Elastic IP is not working

Investigate:

1. Is the Elastic IP allocated?
2. Is it associated with the correct resource?
3. Is the instance running?
4. Does the subnet have a route to an Internet Gateway?
5. Does the Security Group allow required traffic?
6. Do Network ACLs allow required traffic?
7. Is the web server running?
8. Is the correct IP being used?

## 52. Scenario 6 – Snapshot restore does not show expected files

Investigate:

1. Was the correct snapshot selected?
2. Was the volume created in the correct Availability Zone?
3. Is the volume attached to the correct instance?
4. Was the correct device identified?
5. Does the filesystem exist?
6. Is the filesystem mounted at the correct directory?
7. Are permissions correct?
8. Was the snapshot application-consistent enough for the workload?

Commands:

```bash
lsblk -f
```

```bash
sudo blkid
```

```bash
df -hT
```

```bash
findmnt
```

---

# Part 11 – Hands-on Lab Assignments

## 53. Lab 01 – Create and launch an AMI

**Objective:** Create a reusable AMI from a configured training server.

- [ ] Launch an Amazon Linux EC2 instance.
- [ ] Install Nginx.
- [ ] Verify that Nginx is running.
- [ ] Create a custom AMI.
- [ ] Wait until the AMI is available.
- [ ] Launch a second EC2 instance from the custom AMI.
- [ ] Verify that Nginx is installed on the second instance.
- [ ] Record the AMI ID and both instance IDs.

## 54. Lab 02 – Create an EC2 Launch Template

**Objective:** Standardize EC2 instance launch settings.

- [ ] Create a Launch Template.
- [ ] Select an AMI.
- [ ] Configure an instance type.
- [ ] Configure a key pair.
- [ ] Configure a Security Group.
- [ ] Configure EBS storage.
- [ ] Add useful tags.
- [ ] Launch an instance from the template.
- [ ] Verify the launched configuration.

## 55. Lab 03 – Security Group configuration

**Objective:** Control access to an EC2 web server.

- [ ] Create a new Security Group.
- [ ] Allow SSH only from the student's trusted public IP.
- [ ] Allow HTTP from the internet for the training website.
- [ ] Verify website access.
- [ ] Temporarily remove the HTTP rule.
- [ ] Test the website and observe the failure.
- [ ] Restore the HTTP rule.
- [ ] Verify website access again.

Do not remove SSH access while relying on that SSH session for administration.

## 56. Lab 04 – EBS volume management

**Objective:** Attach, format and mount an additional EBS volume.

- [ ] Create a 10 GiB `gp3` volume.
- [ ] Select the same Availability Zone as the EC2 instance.
- [ ] Attach the volume.
- [ ] Identify the new device using `lsblk`.
- [ ] Inspect the device using `lsblk -f` and `file -s`.
- [ ] Create an XFS filesystem only if the volume is confirmed empty.
- [ ] Create `/data`.
- [ ] Mount the volume.
- [ ] Create a test file.
- [ ] Configure persistent mounting using the UUID.
- [ ] Validate the `/etc/fstab` entry.

## 57. Lab 05 – Snapshot and restore

**Objective:** Back up and restore an EBS data volume.

- [ ] Create a test file on `/data`.
- [ ] Create an EBS snapshot.
- [ ] Wait for snapshot completion.
- [ ] Create a new EBS volume from the snapshot.
- [ ] Select the correct Availability Zone.
- [ ] Attach the restored volume.
- [ ] Identify its device.
- [ ] Inspect the filesystem.
- [ ] Mount it at `/restore`.
- [ ] Verify the test file is present.

## 58. Lab 06 – Elastic IP

**Objective:** Allocate and associate a static public IPv4 address.

- [ ] Allocate an Elastic IP.
- [ ] Associate it with the training EC2 instance.
- [ ] Open the website using the Elastic IP.
- [ ] Record the address.
- [ ] Disassociate the address.
- [ ] Release it after confirming it is not required.

## 59. Lab 07 – Explore Placement Groups and ENIs

**Objective:** Understand instance placement and network interfaces.

- [ ] Explore Cluster, Spread and Partition strategies.
- [ ] Create a training Spread Placement Group if required.
- [ ] Review placement group restrictions.
- [ ] Open the EC2 Network Interfaces page.
- [ ] Identify the primary ENI of the training instance.
- [ ] Record its ENI ID, subnet, private IP and Security Groups.
- [ ] Run `ip addr` and `ip route` on Linux.
- [ ] Create a secondary ENI only if the instance type and lab design support it.
- [ ] Verify the attachment and clean up resources.

---

# Part 12 – Interview Questions

## 60. AMI and Launch Template questions

1. What is an AMI?
2. Why do we create custom AMIs?
3. What is the difference between an AMI and an EC2 instance?
4. What is the relationship between an AMI and an EBS snapshot?
5. What is an EC2 Launch Template?
6. What information can a Launch Template contain?
7. What is Launch Template versioning?
8. Does creating a new Launch Template version automatically update existing instances?
9. What should be checked after launching an instance from a custom AMI?
10. Why should secrets not be stored inside a reusable AMI?

## 61. Security Group questions

1. What is a Security Group?
2. What is an inbound rule?
3. What is an outbound rule?
4. What is the difference between a Security Group and a Network ACL?
5. Are Security Groups stateful or stateless?
6. Can a Security Group have explicit deny rules?
7. Why should SSH not normally be open to everyone?
8. What does `0.0.0.0/0` mean?
9. What does `/32` mean in an IPv4 CIDR?
10. How would you troubleshoot an SSH connection failure?

## 62. EBS and Snapshot questions

1. What is Amazon EBS?
2. What is an EBS volume?
3. What is the difference between a root volume and a data volume?
4. What is the difference between `gp3` and `io2`?
5. Can an EBS volume be attached to an instance in a different Availability Zone?
6. What is the purpose of mounting a volume?
7. What is an EBS snapshot?
8. What is an incremental snapshot?
9. What is the difference between an EBS snapshot and an AMI?
10. How do you restore an EBS volume from a snapshot?
11. What is the purpose of `/etc/fstab`?
12. Why should you check `lsblk` before formatting a disk?

## 63. Elastic IP, Placement Group and ENI questions

1. What is an Elastic IP?
2. What is the difference between an auto-assigned public IP and an Elastic IP?
3. Can an Elastic IP be reassociated with another supported resource?
4. What is an EC2 Placement Group?
5. What are the three Placement Group strategies?
6. When would you use a Cluster Placement Group?
7. What is the purpose of a Spread Placement Group?
8. What is a Partition Placement Group?
9. What is an Elastic Network Interface?
10. What information is associated with an ENI?
11. What is the difference between an ENI and a public IP?
12. Can a secondary ENI be moved between compatible EC2 instances?

---

# Part 13 – Knowledge Check

## 64. Multiple Choice Questions

### 1. What is an AMI used for?

A. Monitoring CPU utilization  
B. Launching EC2 instances  
C. Managing DNS records  

**Answer:** B. Launching EC2 instances

### 2. Which service provides block storage for EC2?

A. EBS  
B. S3  
C. Route 53  

**Answer:** A. EBS

### 3. Which resource controls allowed inbound and outbound traffic for an EC2 network interface?

A. AMI  
B. Security Group  
C. Snapshot  

**Answer:** B. Security Group

### 4. Which EBS volume type is a common general-purpose SSD choice?

A. `gp3`  
B. `sc1`  
C. `st1`  

**Answer:** A. `gp3`

### 5. What is an EBS snapshot?

A. A running EC2 instance  
B. A point-in-time backup of an EBS volume  
C. A Security Group rule  

**Answer:** B. A point-in-time backup of an EBS volume

### 6. What does an Elastic IP provide?

A. A static public IPv4 address  
B. A private subnet  
C. A Linux filesystem  

**Answer:** A. A static public IPv4 address

### 7. Which Placement Group strategy is designed for low latency and high network throughput between instances?

A. Spread  
B. Cluster  
C. Partition  

**Answer:** B. Cluster

### 8. Which Placement Group strategy provides partition-level placement isolation?

A. Partition  
B. Cluster  
C. Default  

**Answer:** A. Partition

### 9. What is an ENI?

A. A virtual network interface  
B. An EBS backup  
C. A machine image  

**Answer:** A. A virtual network interface

### 10. Which command lists Linux block devices?

A. `ip addr`  
B. `lsblk`  
C. `systemctl`  

**Answer:** B. `lsblk`

---

# Part 14 – Trainer's Session Plan

## 65. Suggested training schedule

Recommended duration: approximately 4–5 hours.

| Session | Duration |
|---|---:|
| AMI concepts and AMI practical | 35 minutes |
| Launch Templates | 25 minutes |
| Security Groups and firewall rules | 40 minutes |
| EBS volumes and Linux mounting practical | 60 minutes |
| EBS snapshots and restore | 40 minutes |
| Elastic IP | 20 minutes |
| Placement Groups | 25 minutes |
| ENI and networking practical | 30 minutes |
| Troubleshooting and interview discussion | 25 minutes |

The trainer can divide the hands-on exercises across multiple sessions if AWS account permissions, capacity or budget are limited.

## 66. Trainer's closing notes

By completing this module, students should understand how an EC2 server is built, secured, connected to storage and prepared for recovery.

The key industry lesson is that an EC2 instance is only one part of the infrastructure.

A reliable application environment also needs:

- A trusted machine image.
- Repeatable launch configuration.
- Least-privilege network access.
- Correct storage configuration.
- Tested backup and restore procedures.
- Reliable network addressing.
- Appropriate placement and fault isolation.
- Monitoring and operational documentation.

For Application Support Engineers, understanding these components makes it easier to investigate incidents such as:

- Server launch failures.
- SSH and website connectivity issues.
- Disk-full alerts.
- Missing or unmounted volumes.
- Failed restores.
- Unexpected public IP changes.
- Incorrect network interface configuration.

**Next recommended module:** Amazon VPC – Virtual Private Cloud, CIDR, subnets, route tables, Internet Gateway, NAT Gateway, Network ACLs and VPC troubleshooting.

---

## Official Learning References

1. Amazon Machine Images: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html
2. EC2 Launch Templates: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-launch-template.html
3. Security Groups: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html
4. Amazon EBS: https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html
5. EBS Volume Types: https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html
6. EBS Snapshots: https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html
7. Elastic IP Addresses: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html
8. EC2 Placement Groups: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html
9. Elastic Network Interfaces: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html
10. AWS Pricing: https://aws.amazon.com/pricing/

---

**End of Module 02 – EC2 Infrastructure Components**

*Prepared for Application Support Training.*
