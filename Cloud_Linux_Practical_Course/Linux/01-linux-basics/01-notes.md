# Linux Basics — Notes

## 1. What is Linux?

Linux is an open-source operating system kernel. Linux distributions combine the kernel with utilities, package managers, libraries and other software.

Common distributions:
- Ubuntu
- Debian
- Rocky Linux
- AlmaLinux
- Fedora
- Amazon Linux

## 2. Terminal

The terminal allows us to interact with Linux using commands.

A command generally has:

```text
command + options + arguments
```

Example:

```bash
ls -lah /var/log
```

## 3. Important Commands

```bash
pwd
ls
cd
whoami
id
hostname
uname -a
date
history
clear
man ls
```

## 4. Absolute vs Relative Paths

Absolute:

```text
/etc/nginx/nginx.conf
```

Relative:

```text
./config/app.conf
```

## 5. Root User

`root` has extensive privileges.

Check the current user:

```bash
whoami
```

Use `sudo` when administrative privileges are needed.

## 6. Package Management

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install nginx
sudo apt remove nginx
```

Do not run random installation commands from the internet without understanding what they do.

## 7. Services

Systemd systems use:

```bash
systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

## 8. Environment Variables

View:

```bash
printenv
```

Example:

```bash
echo $HOME
echo $PATH
```

## 9. Redirection and Pipes

```bash
ls > files.txt
ls >> files.txt
cat files.txt | grep ".conf"
```

`>` replaces a file. `>>` appends to it.

## 10. Good Habits

- Read commands before executing.
- Use `man` and `--help`.
- Understand permissions.
- Keep production and lab environments separate.
- Verify destructive commands before running them.
