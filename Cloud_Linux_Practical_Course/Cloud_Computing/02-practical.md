# Cloud Computing Practical Lab — Beginner to Intermediate

## Lab Goal

Build a small cloud environment and understand how compute, storage, networking and monitoring work together.

## Level 1 — Create an AWS Account Safely

### Tasks

1. Sign in to AWS.
2. Enable MFA on the root account.
3. Create an administrative IAM identity according to your organization's current AWS account practices.
4. Do not use the root account for daily work.
5. Select one AWS Region and use it consistently for the lab.

### Verification

Confirm:
- MFA is enabled.
- You can access the AWS Console using the intended identity.
- You know which Region you are using.

---

# Level 2 — EC2 Web Server

## Objective

Launch a Linux virtual machine and host a simple web page.

### Steps

1. Open EC2.
2. Choose an Ubuntu LTS AMI.
3. Select a small eligible instance type.
4. Create or select a key pair.
5. Configure the security group:
   - SSH: TCP 22 — preferably restricted to your own IP
   - HTTP: TCP 80 — allow only as required for the lab
6. Launch the instance.
7. Connect using SSH.

### Linux commands

```bash
ssh -i your-key.pem ubuntu@PUBLIC_IP
```

Update packages:

```bash
sudo apt update
```

Install Nginx:

```bash
sudo apt install nginx -y
```

Check service:

```bash
sudo systemctl status nginx
```

Create a simple page:

```bash
echo "<h1>My First Cloud Server</h1>" | sudo tee /var/www/html/index.html
```

Open:

```text
http://PUBLIC_IP
```

### Verification

You should see your HTML page.

---

# Level 3 — S3 Storage

## Objective

Understand object storage.

### Tasks

1. Create an S3 bucket with a globally unique name.
2. Keep public access blocked unless the lab specifically requires another configuration.
3. Upload a text file.
4. Upload an image.
5. Review object properties.
6. Enable versioning for the lab bucket.
7. Delete the test objects when finished.

### Questions

- What is an object?
- What is a bucket?
- Why should public access be disabled by default?
- What is versioning useful for?

---

# Level 4 — CloudWatch

## Objective

Observe an EC2 instance.

### Tasks

1. Open CloudWatch.
2. Review available EC2 metrics.
3. Generate CPU activity on the test instance.
4. Observe the metric change.
5. Create a basic CPU alarm according to the current AWS Console options.
6. Record what caused the alarm.

Generate temporary CPU activity:

```bash
yes > /dev/null &
```

Find the process:

```bash
ps aux | grep yes
```

Stop it:

```bash
kill <PID>
```

> Stop test processes after the experiment.

---

# Level 5 — Architecture Exercise

Draw this architecture:

```text
User
  |
Internet
  |
Security Group
  |
EC2 + Nginx
  |
S3
  |
CloudWatch
```

Explain:
- Where the request enters.
- Which component serves the website.
- Where objects are stored.
- Which service monitors resources.

---

# Final Verification Checklist

- [ ] IAM/MFA understood
- [ ] EC2 created
- [ ] SSH connection successful
- [ ] Nginx installed
- [ ] Web page accessible
- [ ] S3 bucket created
- [ ] Object uploaded
- [ ] CloudWatch metrics reviewed
- [ ] CPU test completed
- [ ] All unnecessary resources deleted

## Cleanup

Terminate the EC2 instance, remove unnecessary storage, delete test objects/buckets where appropriate, and remove unused networking resources.

Never leave paid resources running just because the lab is finished.
