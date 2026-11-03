# Linux ACL — Practical Lab

## Goal

Grant access to one user without changing the main group ownership model.

## Step 1

Create users:

```bash
sudo useradd -m acluser1
sudo useradd -m acluser2
sudo passwd acluser1
sudo passwd acluser2
```

Create lab:

```bash
mkdir -p ~/linux-lab/acl
cd ~/linux-lab/acl
echo "ACL protected content" > report.txt
chmod 600 report.txt
```

## Step 2 — Inspect

```bash
getfacl report.txt
```

## Step 3 — Grant Read Access

```bash
setfacl -m u:acluser1:r report.txt
```

Verify:

```bash
getfacl report.txt
```

## Step 4 — Test

```bash
sudo -iu acluser1
cat /home/$USER/linux-lab/acl/report.txt
exit
```

If the parent path permissions prevent access, create the lab in a shared test location instead and adjust the test directory permissions deliberately.

## Step 5 — Remove ACL

```bash
setfacl -x u:acluser1 report.txt
getfacl report.txt
```

## Cleanup

```bash
rm -rf ~/linux-lab/acl
sudo userdel -r acluser1
sudo userdel -r acluser2
```
