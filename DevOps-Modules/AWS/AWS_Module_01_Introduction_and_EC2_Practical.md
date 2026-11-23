# AWS Application Support Training – Module 01
## Introduction to AWS, Cloud Computing & EC2 Static Website Hosting

**Training level:** Beginner to Practical  
**Module:** 01  
**Batch:** Application Support  
**Practical:** Host a static website on Amazon EC2 using Nginx

---

## Learning Objectives

By the end of this module, students should be able to:

- Understand cloud computing and AWS fundamentals.
- Explain why organizations use AWS.
- Identify common AWS services and their purposes.
- Understand AWS Regions, Availability Zones and Edge Locations.
- Understand AWS billing, payment management and cost control.
- Explain Amazon EC2 and its important components.
- Launch a Linux EC2 instance.
- Connect to EC2 using SSH.
- Install and configure Nginx.
- Host a static website on an EC2 instance.
- Perform basic application and server troubleshooting.
- Clean up AWS resources to avoid unnecessary charges.

# Part 1 – Introduction to Cloud Computing

## 1. What is Cloud Computing?

Before understanding AWS, we must understand cloud computing.

Imagine you want to start a software company. You have developed a website, and now you want to make it available to customers worldwide.

For that, you need:

- A computer or server.
- Storage to store website files.
- A database to store customer information.
- Networking to connect users to your application.
- Security to protect your data.
- Backup and monitoring systems.

Traditionally, companies purchased physical servers, installed them in data centers, configured networking and maintained everything themselves.

This approach required a lot of money, hardware, maintenance and technical resources.

Cloud computing solves this problem.

**Cloud computing is the delivery of computing resources such as servers, storage, databases, networking and software over the internet, usually on a pay-as-you-go basis.**

In simple language:

Instead of purchasing your own physical server, you rent computing resources from a cloud provider whenever you need them.

### Real-life example

Suppose you want to host a website.

**Traditional approach:**

1. Purchase a physical server.
2. Install an operating system.
3. Configure networking.
4. Arrange electricity and cooling.
5. Install web-server software.
6. Maintain hardware and software.

**Cloud approach:**

1. Create a cloud account.
2. Select a server configuration.
3. Launch a virtual server.
4. Deploy your application.
5. Pay according to the resources and pricing model you use.

> **Important:** Cloud does not mean there are no physical servers. Cloud providers operate large physical data centers. They provide computing resources to customers through cloud platforms.

## 2. Traditional Infrastructure vs Cloud Computing

![Traditional infrastructure versus cloud computing](https://images.unsplash.com/photo-1558494949-ef010cbdcc31?w=1200)

Let's understand with a real-life example.

Suppose a company wants to host its application.

### Traditional infrastructure

1. Purchase a physical server.
2. Purchase RAM, storage and networking equipment.
3. Set up a server room.
4. Arrange electricity and cooling.
5. Install an operating system.
6. Configure networking and security.
7. Hire people to maintain the infrastructure.

Even if the application has only 100 users, the company has to invest in the infrastructure.

### Cloud infrastructure

1. Create an AWS account.
2. Select a server configuration.
3. Launch a virtual server.
4. Deploy your application.
5. Pay for the resources you consume.

No need to purchase physical hardware.

| Traditional infrastructure | Cloud infrastructure |
|---|---|
| Buy physical servers | Rent virtual servers |
| High initial investment | Lower initial investment |
| Hardware maintenance | Provider manages physical hardware |
| Manual capacity expansion | Scale resources when needed |
| Physical data center required | Access through the internet |
| Hardware replacement required | Launch replacement resources |

## 3. What is AWS?

![Amazon Web Services](https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg)

**AWS stands for Amazon Web Services.**

AWS is a cloud computing platform provided by Amazon. It provides services such as:

- Computing power.
- Storage.
- Databases.
- Networking.
- Security.
- Application deployment.
- Monitoring.
- Artificial intelligence and machine learning.

AWS allows individuals, startups, enterprises and government organizations to build, deploy and manage applications without purchasing their own physical infrastructure.

### Real-world example

Imagine an e-commerce application. It needs infrastructure to handle:

- Customers browsing products.
- Large amounts of data.
- Orders and payments.
- Continuous application availability.
- Traffic spikes.
- Security and monitoring.

AWS provides services that help organizations build and operate this infrastructure.

Official website: https://aws.amazon.com/

## 4. Why Do Organizations Prefer AWS?

Organizations choose AWS for several reasons.

### 4.1 Scalability

Increase or decrease computing resources according to application requirements.

**Example:** An e-commerce website can handle additional traffic during a festival sale.

### 4.2 High Availability

AWS provides infrastructure across multiple Availability Zones and Regions. Applications can be designed to remain available even if infrastructure fails.

### 4.3 Pay-as-you-go Pricing

Pay for many services based on usage instead of purchasing all infrastructure upfront.

### 4.4 Security

AWS provides services for identity management, encryption, network security and threat detection.

### 4.5 Global Reach

AWS has infrastructure in multiple geographic Regions, allowing companies to deploy applications closer to customers.

### 4.6 Monitoring and Automation

AWS provides services for monitoring resources, collecting logs, creating alarms and automating infrastructure management.

## 5. Where is AWS Used?

| Industry | AWS usage |
|---|---|
| Banking | Application hosting, databases and security |
| Healthcare | Healthcare applications and data storage |
| E-commerce | Product catalogs, orders and payments |
| Education | Online learning platforms |
| Entertainment | Video streaming and content delivery |
| IT companies | Application hosting, CI/CD and infrastructure |

## 6. Common AWS Services

![AWS cloud services](https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=1200)

| Category | AWS service | Purpose |
|---|---|---|
| Compute | EC2 | Virtual servers |
| Storage | S3 | Object storage |
| Database | RDS | Managed relational database |
| Networking | VPC | Private cloud networking |
| Networking | Elastic Load Balancing | Load balancing |
| Security | IAM | Access and permission management |
| Monitoring | CloudWatch | Metrics, logs and alarms |
| Scaling | Auto Scaling | Automatically adjust capacity |
| DNS | Route 53 | Domain name management |
| Content delivery | CloudFront | Content delivery network |

AWS is not a single service. It is a collection of many cloud services that work together.

# Part 2 – AWS Global Infrastructure

## 7. What is AWS Global Infrastructure?

AWS operates infrastructure across different geographical locations around the world.

There are three important concepts.

### 7.1 Region

A Region is a separate geographical area where AWS operates its infrastructure.

Examples:

- Mumbai – `ap-south-1`
- Hyderabad – `ap-south-2`
- Singapore – `ap-southeast-1`
- US East (N. Virginia) – `us-east-1`

### 7.2 Availability Zone (AZ)

An Availability Zone is an isolated location within an AWS Region, consisting of one or more data centers with independent infrastructure.

A Region contains multiple Availability Zones.

### 7.3 Edge Location

Edge Locations are sites used by AWS services such as CloudFront to deliver content closer to end users.

![AWS global infrastructure](https://images.unsplash.com/photo-1516321318423-f06f85e504b3?w=1200)

### Real-world example

Suppose your company has an application hosted in Mumbai.

If you want to improve availability, you can deploy application servers across multiple Availability Zones in the Mumbai Region.

If one Availability Zone has an issue, the application can continue operating through another healthy Availability Zone, provided the application has been designed for this.

# Part 3 – AWS Account and Payment Management

## 8. How Does AWS Payment Work?

This is an important topic for Application Support Engineers.

AWS follows a usage-based billing model for many services. You are charged based on the resources you consume and the pricing model of each service.

**Important:** AWS is not completely free. Free Tier eligibility and allowances depend on the account creation date, service and current offer.

### 8.1 Common AWS Billing Components

1. **Compute charges:** EC2 instance usage.
2. **Storage charges:** S3 storage and EBS volumes.
3. **Data transfer charges:** Applicable network data transfer.
4. **Database charges:** RDS instance and storage usage.
5. **Additional service charges:** Depending on services enabled.

### 8.2 Example Monthly Bill

The following figures are illustrative only and are not actual AWS prices.

| Resource | Example monthly cost |
|---|---:|
| EC2 instance | ₹1,500 |
| EBS storage | ₹300 |
| S3 storage | ₹100 |
| Data transfer | ₹200 |
| **Total** | **₹2,100** |

Real prices depend on Region, configuration, usage, taxes and pricing options.

## 9. Important AWS Billing Services

| Service | Purpose |
|---|---|
| AWS Billing and Cost Management | Manage bills and payments |
| AWS Cost Explorer | Analyze spending |
| AWS Budgets | Set cost and usage budgets |
| AWS Pricing Calculator | Estimate future costs |
| Cost Anomaly Detection | Detect unusual spending patterns |

### 9.1 AWS Billing and Cost Management

This dashboard helps account owners:

- View current and previous bills.
- Review service-wise charges.
- Manage available payment methods.
- View invoices and tax information.
- Monitor account spending.

### 9.2 AWS Cost Explorer

Cost Explorer helps analyze AWS spending over time.

For example, you can identify:

- Which service costs the most.
- Which Region is generating charges.
- How costs change month by month.
- Which usage category needs investigation.

### 9.3 AWS Budgets

AWS Budgets allows you to define cost or usage thresholds and receive notifications.

Example:

Suppose your training budget is ₹500.

You can configure an alert when actual or forecasted spending reaches a defined percentage of the budget.

**Important:** A budget alert does not automatically stop resources. It sends notifications based on your configuration.

### 9.4 AWS Pricing Calculator

Use the AWS Pricing Calculator before deploying resources.

It helps estimate costs based on:

- Region.
- Instance type.
- Operating system.
- Running hours.
- Storage.
- Data transfer.
- Other service configuration.

Official calculator: https://calculator.aws/

## 10. How to Manage AWS Costs

1. Choose the correct EC2 instance size.
2. Stop unused EC2 instances.
3. Delete unused EBS volumes and snapshots when no longer needed.
4. Monitor S3 storage.
5. Configure AWS Budgets.
6. Review monthly bills.
7. Use Cost Explorer to identify expensive services.
8. Review public IPv4 and data-transfer charges where applicable.
9. Remove unused resources after verifying that they are not required.

> **Important:** Stopping an EC2 instance does not necessarily stop all charges. EBS storage, Elastic IPs in certain configurations, snapshots and other associated resources can still incur charges.

## 11. AWS Free Tier – What Should Students Know?

AWS offers different Free Tier benefits depending on account eligibility and signup date.

Students should understand:

- Always check the Free Tier page for their own account.
- Some benefits have usage limits.
- Some services are not covered.
- Exceeding allowances can result in charges.
- Setting a budget alert does not automatically stop resources.

Useful links:

- AWS Free Tier: https://aws.amazon.com/free/
- AWS Pricing: https://aws.amazon.com/pricing/
- AWS Pricing Calculator: https://calculator.aws/
- AWS Cost Management Documentation: https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html

# Part 4 – Understanding Amazon EC2

## 12. What is EC2?

![Amazon EC2 service](https://upload.wikimedia.org/wikipedia/commons/1/1d/AmazonWebservices_Console.svg)

**EC2 stands for Elastic Compute Cloud.**

Amazon EC2 is a service that allows users to create and manage virtual servers in the AWS Cloud.

In simple language:

EC2 provides a virtual computer on the internet. You can install an operating system, configure software, run applications, host websites and manage services on it.

### Why do we use EC2?

Suppose you have developed a website using HTML, CSS and JavaScript.

You want people to access your website from anywhere in the world.

You need a server that:

- Runs continuously.
- Stores your website files.
- Accepts user requests.
- Sends website content to browsers.
- Can be monitored and maintained.

EC2 provides the virtual server where you can perform these operations.

## 13. How Does EC2 Work?

![Web server architecture](https://images.unsplash.com/photo-1558494949-ef010cbdcc31?w=1200)

Basic request flow:

1. A user opens a website in a browser.
2. The browser sends an HTTP request.
3. The request travels through the internet.
4. The request reaches the EC2 public IP address.
5. The Nginx web server receives the request.
6. Nginx searches for the requested HTML file.
7. Nginx returns the website content to the browser.
8. The user sees the website.

This is the basic architecture we will implement today.

## 14. Important EC2 Terminology

| Term | Explanation |
|---|---|
| EC2 Instance | A virtual server |
| AMI | Amazon Machine Image; a template used to launch an instance |
| Instance Type | Defines CPU, memory and networking capacity |
| Key Pair | Credentials used for secure SSH access |
| Security Group | Virtual firewall controlling inbound and outbound traffic |
| EBS | Persistent block storage for EC2 |
| Public IP | IP address used to reach the instance from the internet |
| Private IP | Internal IP address used within a private network |
| Elastic IP | Static public IPv4 address that can be allocated to an account |
| User Data | Scripts that can run during instance initialization |

## 15. Understanding EC2 Configuration

When creating an EC2 instance, you configure the following.

### 15.1 Name

A friendly name to identify the server.

Example:

`application-support-web-server`

### 15.2 AMI

AMI stands for Amazon Machine Image.

It is a template that contains the information required to launch an instance, including an operating system and potentially preconfigured software.

Examples:

- Amazon Linux 2023
- Ubuntu Server
- Red Hat Enterprise Linux
- Windows Server

### 15.3 Instance Type

Instance type determines the hardware resources available to the virtual server.

| Instance family | Typical purpose |
|---|---|
| T3 | Burstable general-purpose workloads |
| T4g | Arm-based burstable workloads |
| M | General-purpose workloads |
| C | Compute-intensive workloads |
| R | Memory-intensive workloads |

### 15.4 Key Pair

A key pair is used to authenticate a connection to your Linux EC2 instance through SSH.

### 15.5 Security Group

A security group controls which network connections can reach your server.

### 15.6 Storage

EC2 generally uses EBS volumes for persistent block storage.

# Part 5 – Practical: Host a Static Website on AWS EC2

## 16. Practical Objective

In this practical, students will:

1. Create a Linux EC2 instance.
2. Configure its network access.
3. Connect to the instance using SSH.
4. Install Nginx.
5. Create a static website.
6. Host the website using the EC2 public IP address.
7. Test and troubleshoot the website.

### Practical Architecture

**User Browser → Internet → EC2 Security Group → EC2 Linux Instance → Nginx → Website Files**

### Practical Configuration

| Component | Configuration |
|---|---|
| Cloud provider | AWS |
| Service | EC2 |
| Operating system | Amazon Linux 2023 |
| Instance type | `t3.micro` (subject to regional availability and pricing) |
| Web server | Nginx |
| Website | HTML, CSS and JavaScript |
| Access | Public IPv4 address |
| HTTP port | 80 |
| SSH port | 22 |

> **Cost warning:** Check current Free Tier eligibility and estimated charges before launching. Do not assume this instance is free.

## 17. Step 1 – Login to AWS Management Console

1. Open https://console.aws.amazon.com/
2. Sign in to your AWS account.
3. Search for `EC2` in the search bar.
4. Open the EC2 dashboard.
5. Select the Region in which you want to create your server.

For this practical, select Mumbai (`ap-south-1`) if available and suitable.

## 18. Step 2 – Launch an EC2 Instance

1. Open the EC2 Dashboard.
2. Click **Instances**.
3. Click **Launch instances**.
4. Enter the instance name:

   `application-support-web-server`

5. Under Application and OS Images (AMI), select:

   **Amazon Linux 2023 AMI**

6. Under Instance Type, select:

   `t3.micro`

7. Under Key Pair, click **Create new key pair**.

### Create Key Pair

Enter:

- Key pair name: `aws-training-key`
- Key pair type: RSA
- Private key file format: `.pem`

Click **Create key pair**.

Your browser will download the private key file.

> **Important security rules**
>
> - Never share your `.pem` file.
> - Never upload it to GitHub.
> - Never send it through WhatsApp or email.
> - Keep a secure backup.
> - Do not lose it, because you may need it to access your instance.

## 19. Step 3 – Configure Network Settings

Under Network Settings, configure the Security Group.

Create a new security group named:

`web-server-security-group`

Add the following inbound rules:

| Type | Port | Source | Purpose |
|---|---:|---|---|
| SSH | 22 | My IP | Remote server administration |
| HTTP | 80 | `0.0.0.0/0` | Public website access |

### Why do we configure these rules?

- Port 22 allows SSH connections to the Linux server.
- Port 80 allows users to access the website through HTTP.

For SSH, select **My IP** rather than allowing access from everywhere.

For a real production website, use HTTPS on port 443 with a properly configured TLS certificate. Restrict SSH further through a VPN, bastion host or AWS Systems Manager where appropriate.

## 20. Step 4 – Configure Storage

For this training exercise:

- Volume type: General Purpose SSD (`gp3`) if available.
- Size: 8 GiB.
- Keep the volume encrypted.

EBS is the storage device attached to the EC2 instance.

It stores:

- Operating system files.
- Application files.
- Configuration files.
- Website files.

Remember that EBS storage can incur charges even when the EC2 instance is stopped.

## 21. Step 5 – Launch Instance

Review the configuration.

Click **Launch Instance**.

Wait until the instance is running and its status checks pass.

You should see:

- Instance ID.
- Instance state.
- Public IPv4 address.
- Private IPv4 address.
- Instance type.

**Practical checkpoint:** Students should write down their instance ID and public IPv4 address.

# Part 6 – Connect to EC2 Using SSH

## 22. What is SSH?

SSH stands for Secure Shell.

SSH is a network protocol that allows you to securely connect to and manage remote servers.

For example, your laptop is in Pune and your EC2 server is hosted in an AWS data center in Mumbai. You can connect to that remote server using SSH.

### Step 1 – Open EC2 Instance Details

1. Go to EC2.
2. Select Instances.
3. Select your running instance.
4. Copy the Public IPv4 address.

### Step 2 – Open an SSH Client

On Windows, students can use:

- Windows PowerShell.
- Windows Terminal.
- Git Bash.

Make sure OpenSSH Client is installed.

### Step 3 – Connect Using SSH

Assume your private key is in the Downloads folder.

In PowerShell:

```powershell
ssh -i "$HOME\Downloads\aws-training-key.pem" ec2-user@YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with your actual EC2 public IPv4 address.

For Amazon Linux 2023, the default username is:

`ec2-user`

If the connection is successful, you will see a Linux terminal prompt.

### Understand the command

```bash
ssh -i aws-training-key.pem ec2-user@PUBLIC_IP
```

| Part | Meaning |
|---|---|
| `ssh` | Connect using SSH |
| `-i` | Specify private key |
| `aws-training-key.pem` | Private key file |
| `ec2-user` | Linux login username |
| `PUBLIC_IP` | EC2 server address |

# Part 7 – Install Nginx Web Server

## 23. What is Nginx?

Nginx is a web server that can serve static website files, act as a reverse proxy, handle HTTP requests and perform load balancing.

In today's practical, Nginx will serve our HTML, CSS and JavaScript files.

### Step 1 – Update Linux Packages

Run:

```bash
sudo dnf update -y
```

Explanation:

- `sudo` – Run a command with elevated privileges.
- `dnf` – Package manager in Amazon Linux 2023.
- `update` – Update installed packages.
- `-y` – Automatically confirm prompts.

### Step 2 – Install Nginx

```bash
sudo dnf install nginx -y
```

### Step 3 – Start Nginx

```bash
sudo systemctl start nginx
```

### Step 4 – Enable Nginx on Boot

```bash
sudo systemctl enable nginx
```

This ensures that Nginx starts automatically when the server boots.

### Step 5 – Check Nginx Status

```bash
sudo systemctl status nginx
```

Expected status:

```text
Active: active (running)
```

### Step 6 – Test Nginx Locally

Run:

```bash
curl http://localhost
```

If Nginx is working, you should receive its default HTML page.

Now open your browser and enter:

`http://YOUR_PUBLIC_IP`

You should see the Nginx welcome page.

# Part 8 – Create and Host Your Own Static Website

## 24. Understand the Website Directory

For our Amazon Linux 2023 Nginx installation, the default document root is:

```bash
/usr/share/nginx/html/
```

Let's explore it.

```bash
cd /usr/share/nginx/html/
```

List files:

```bash
ls -la
```

You will find the default website file, usually `index.html`.

### What is index.html?

`index.html` is the default entry page that Nginx serves when someone visits your website without specifying a particular file.

## 25. Create Your Website

### Step 1 – Create index.html

Run:

```bash
sudo nano /usr/share/nginx/html/index.html
```

Paste the following complete code.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AWS EC2 Website</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: #101827;
            color: white;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 20px;
        }

        .container {
            max-width: 700px;
            padding: 50px 30px;
            background: #1e293b;
            border-radius: 15px;
            box-shadow: 0 10px 35px #0005;
        }

        h1 {
            color: #ff9900;
            font-size: 38px;
            margin-bottom: 20px;
        }

        p {
            color: #cbd5e1;
            line-height: 1.8;
            margin-bottom: 15px;
        }

        .status {
            display: inline-block;
            background: #14532d;
            color: #86efac;
            padding: 12px 25px;
            border-radius: 25px;
            margin: 20px 0;
        }

        footer {
            margin-top: 25px;
            font-size: 13px;
            color: #94a3b8;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>Welcome to AWS EC2!</h1>

        <p>
            Congratulations! Your static website is
            successfully hosted on an AWS EC2 instance.
        </p>

        <div class="status">
            ● Application Status: Running
        </div>

        <p>
            Web Server: Nginx<br>
            Operating System: Amazon Linux 2023<br>
            Cloud Provider: Amazon Web Services
        </p>

        <footer>
            AWS Application Support Training - Practical Lab 01
        </footer>
    </div>
</body>
</html>
```

### Step 2 – Save the File

If you are using Nano:

1. Press `CTRL + O`.
2. Press Enter.
3. Press `CTRL + X`.

### Step 3 – Set Appropriate File Permissions

```bash
sudo chmod 644 /usr/share/nginx/html/index.html
```

This allows the web server to read the file.

### Step 4 – Restart Nginx

```bash
sudo systemctl restart nginx
```

### Step 5 – Verify Website

Run:

```bash
curl http://localhost
```

You should see the HTML source of your website.

Now open your browser:

`http://YOUR_PUBLIC_IP`

**Practical completed!** Students have successfully:

- Created an AWS EC2 instance.
- Configured a Linux server.
- Connected using SSH.
- Installed Nginx.
- Created a static website.
- Hosted the website publicly.

# Part 9 – Application Support Troubleshooting

## 26. Common Problems and Solutions

Imagine a customer reports:

"My website is not opening."

As an Application Support Engineer, you should investigate the issue systematically.

| Problem | Possible reason | Troubleshooting |
|---|---|---|
| Website not opening | EC2 stopped | Check instance state |
| Connection timed out | Port 80 blocked | Check Security Group |
| SSH connection failed | Port 22 blocked | Check SSH inbound rule |
| Permission denied | Incorrect key or username | Verify PEM and username |
| Nginx not working | Service stopped | Start Nginx |
| 403 Forbidden | Incorrect permissions | Check directory and file permissions |
| 404 Not Found | File missing or incorrect URL | Check document root |
| Default page showing | Wrong website file | Check `index.html` |

## 27. Important Troubleshooting Commands

Students should practice these commands.

### 1. Check instance operating system

```bash
cat /etc/os-release
```

### 2. Check Nginx status

```bash
sudo systemctl status nginx
```

### 3. Restart Nginx

```bash
sudo systemctl restart nginx
```

### 4. Check listening ports

```bash
sudo ss -tulnp
```

### 5. Check Nginx access logs

```bash
sudo tail -f /var/log/nginx/access.log
```

### 6. Check Nginx error logs

```bash
sudo tail -f /var/log/nginx/error.log
```

### 7. Check disk space

```bash
df -h
```

### 8. Check memory

```bash
free -h
```

### 9. Check CPU and running processes

```bash
top
```

### 10. Test website locally

```bash
curl -I http://localhost
```

## 28. Real-life Troubleshooting Exercise

Ask students to intentionally stop Nginx.

```bash
sudo systemctl stop nginx
```

Now open the public IP address in a browser.

The website will not load.

Ask students to identify the problem.

They should:

1. Connect to EC2 using SSH.
2. Check Nginx status.
3. Identify that the service is inactive.
4. Start the service.
5. Verify the website.

```bash
sudo systemctl start nginx
sudo systemctl status nginx
```

This is a basic example of an application incident and its resolution.

# Part 10 – AWS Cost and Security Cleanup

## 29. Do Not Skip Resource Cleanup

Once the practical is completed, students must understand resource cleanup.

An EC2 instance left running can continue generating charges.

### How to terminate your EC2 instance

1. Open AWS EC2 Dashboard.
2. Select Instances.
3. Select your training instance.
4. Select Instance State.
5. Click Terminate Instance.
6. Confirm termination.

Also check:

- Whether an EBS volume remains after termination.
- Whether any snapshots were created.
- Whether an Elastic IP has been allocated.
- Whether any other paid resources were created.

Delete unused resources only after confirming they are not needed.

> **Important:** Termination is generally irreversible for the instance's local state. Ensure you have backed up anything you want to retain. An EBS volume configured to delete on termination is normally deleted automatically, but separately retained volumes may remain chargeable.

For a production server, do not terminate resources without the proper change approval.

# Part 11 – Student Assignment

## 30. Practical Assignment 01: AWS EC2 Website Hosting

### Objective

Create and host a static website on an AWS EC2 Linux instance using Nginx.

### Tasks

- [ ] Create an AWS account and explore the AWS Console.
- [ ] Understand AWS Regions and Availability Zones.
- [ ] Launch an Amazon Linux 2023 EC2 instance.
- [ ] Create and download a PEM key pair.
- [ ] Configure SSH and HTTP Security Group rules.
- [ ] Connect to EC2 using SSH.
- [ ] Update Linux packages.
- [ ] Install and start Nginx.
- [ ] Create a custom HTML website.
- [ ] Host the website on the EC2 public IP.
- [ ] Stop Nginx and troubleshoot the website failure.
- [ ] Terminate the EC2 instance and verify resource cleanup.

### Submission Requirements

Students should submit:

1. Screenshot of the running EC2 instance.
2. Screenshot of successful SSH connection.
3. Screenshot of Nginx running.
4. Screenshot of the hosted website.
5. Screenshot or output of the Nginx status command.
6. A short explanation of the troubleshooting exercise.
7. Confirmation that unused resources were cleaned up.

# Part 12 – Interview Questions

## 31. AWS and Cloud Computing Questions

1. What is cloud computing?
2. What is AWS?
3. Why do organizations prefer AWS?
4. What is the difference between traditional infrastructure and cloud infrastructure?
5. What are the advantages of cloud computing?
6. What is the difference between scalability and elasticity?
7. What are AWS Regions and Availability Zones?
8. What is an Edge Location?
9. What is the AWS pay-as-you-go model?
10. What is AWS Free Tier?

## 32. EC2 Questions

1. What is Amazon EC2?
2. What does EC2 stand for?
3. What is an AMI?
4. What is an EC2 instance type?
5. What is a Security Group?
6. What is the difference between public IP and private IP?
7. What is an Elastic IP?
8. What is EBS?
9. What is a key pair?
10. What is SSH?
11. What is the difference between stopping and terminating an EC2 instance?
12. What is Nginx?
13. Which port is used by HTTP?
14. Which port is used by SSH?
15. What would you check if a website hosted on EC2 is not opening?

# Part 13 – Knowledge Check

## 33. Multiple Choice Questions

**1. What does EC2 stand for?**

- A. Elastic Compute Cloud
- B. Enterprise Cloud Computing
- C. Elastic Container Cluster

**Answer:** A. Elastic Compute Cloud

**2. Which port is used by HTTP?**

- A. 22
- B. 80
- C. 443

**Answer:** B. 80

**3. Which command checks Nginx status?**

- A. `sudo nginx check`
- B. `sudo systemctl status nginx`
- C. `sudo dnf status`

**Answer:** B. `sudo systemctl status nginx`

**4. Which AWS service is used for object storage?**

- A. EC2
- B. RDS
- C. S3

**Answer:** C. S3

**5. What happens when an EC2 instance is terminated?**

- A. It is permanently shut down and retired.
- B. It only restarts.
- C. It becomes an S3 bucket.

**Answer:** A. It is permanently shut down and retired. Associated resources may have different deletion settings.

# Part 14 – Trainer's Session Plan

## 34. Suggested Training Schedule

Recommended duration: approximately 3.5–4 hours.

| Session | Duration |
|---|---:|
| Cloud Computing and AWS introduction | 40 minutes |
| AWS benefits, services and infrastructure | 35 minutes |
| Billing and payment management | 25 minutes |
| EC2 concepts | 30 minutes |
| EC2 website hosting practical | 70 minutes |
| Troubleshooting and interview questions | 30 minutes |

## 35. Trainer's Closing Notes

Today's class should leave students with a clear understanding of how cloud infrastructure works and how to deploy a website on a virtual server.

**Important industry concept:**

We used EC2 for learning server administration. In production, if the requirement is only to host a static website, Amazon S3 with CloudFront is often a simpler and more suitable architecture.

EC2 is valuable for Application Support training because students must understand:

- Operating systems.
- Linux services.
- Network ports.
- Application logs.
- Server monitoring.
- Troubleshooting.
- Infrastructure security.

The goal is to teach students not only how to deploy an application, but also how to investigate and resolve issues when that application stops working.

---

## Next Module

**Module 02 – AWS IAM (Identity and Access Management)**

Topics:

- What is IAM?
- Why do we need IAM?
- IAM users and groups.
- IAM roles.
- IAM policies.
- Permissions and access control.
- Root user security.
- Multi-Factor Authentication (MFA).
- Least privilege principle.
- Practical IAM user and policy configuration.

---

## Official Learning References

1. AWS Official Website: https://aws.amazon.com/
2. AWS Free Tier: https://aws.amazon.com/free/
3. AWS Pricing: https://aws.amazon.com/pricing/
4. AWS Pricing Calculator: https://calculator.aws/
5. Amazon EC2 Documentation: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html
6. Amazon Linux 2023 User Guide: https://docs.aws.amazon.com/linux/al2023/ug/
7. Nginx Documentation: https://nginx.org/en/docs/
8. AWS Cost Management Documentation: https://docs.aws.amazon.com/cost-management/latest/userguide/what-is-costmanagement.html

---

**End of Module 01 – Introduction to AWS and EC2 Static Website Hosting**

*Prepared for Application Support Training.*
