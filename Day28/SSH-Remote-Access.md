# 🔐 Day 28 — SSH & Remote Access

## 🎯 Learning Objectives

By the end of Day 28, you will understand:

* What SSH is
* How remote Linux access works
* How to connect using `ssh`
* SSH usernames, hostnames, and ports
* SSH keys
* `ssh-keygen`
* Public key and private key
* `authorized_keys`
* `known_hosts`
* `scp`
* `sftp`
* File transfer between systems
* SSH security basics
* SSH troubleshooting
* How Java developers use SSH
* How to deploy/manage a Java application remotely

---

# 1. What is SSH?

SSH stands for:

```text
Secure Shell
```

SSH is a protocol used to securely connect to another computer over a network.

For example:

```text
Your Computer
     |
     | SSH
     ↓
Linux Server
```

After connecting, you can run commands on the remote Linux machine.

---

# 2. Why Do We Use SSH?

SSH is commonly used for:

* Remote server administration
* Application deployment
* File transfer
* Running commands remotely
* Checking logs
* Managing services
* Managing Java applications
* Database/server administration
* DevOps operations

For example, a Java application may run on:

```text
Linux Server
     ↓
Spring Boot Application
     ↓
Port 8080
```

You can connect to that server using SSH.

---

# 3. Basic SSH Syntax

The basic syntax is:

```bash
ssh username@hostname
```

Example:

```bash
ssh user@192.168.1.100
```

Here:

```text
ssh       → SSH command
user      → remote username
192.168.1.100 → remote server
```

---

# 4. SSH with Hostname

Instead of an IP address, you can use a hostname.

```bash
ssh user@server.example.com
```

The server name must resolve to the correct IP address.

---

# 5. SSH with a Custom Port

SSH normally uses:

```text
Port 22
```

If the server uses another port:

```bash
ssh -p 2222 user@192.168.1.100
```

Here:

```text
-p 2222
```

means connect using port `2222`.

---

# 6. First SSH Connection

When connecting to a server for the first time, you may see a message similar to:

```text
The authenticity of host 'server' can't be established.
Are you sure you want to continue connecting?
```

You should verify that the server is actually the server you intended to connect to.

If you trust the server and accept it, its host key is stored locally.

This helps SSH detect unexpected changes later.

---

# 7. What is known_hosts?

SSH stores known server identities in:

```text
~/.ssh/known_hosts
```

Example:

```bash
ls -la ~/.ssh
```

You may see:

```text
known_hosts
```

The file contains information about servers you have previously connected to.

---

# 8. SSH Authentication

SSH can authenticate users using:

### Password authentication

```text
Username
+
Password
```

or:

### Key-based authentication

```text
Private Key
+
Public Key
```

Key-based authentication is commonly preferred for server access.

---

# 9. SSH Keys

SSH key authentication uses a pair:

```text
Private Key
+
Public Key
```

Think of it as:

```text
Private Key = Secret
Public Key  = Can be shared
```

Never share your private key.

---

# 10. Private Key

The private key stays on your computer.

Example:

```text
~/.ssh/id_ed25519
```

Treat it like a password.

Do NOT upload it to GitHub.

Do NOT send it to other people.

---

# 11. Public Key

The public key can be placed on the server.

Example:

```text
~/.ssh/id_ed25519.pub
```

The public key is normally added to:

```text
~/.ssh/authorized_keys
```

on the remote server.

---

# 12. Generate an SSH Key

Modern SSH commonly supports Ed25519 keys.

Run:

```bash
ssh-keygen -t ed25519
```

You may see:

```text
Enter file in which to save the key:
```

Press:

```text
Enter
```

to accept the default location.

You may then be asked for a passphrase.

A passphrase is recommended because it adds protection to the private key.

---

# 13. SSH Key Files

After creating the key:

```bash
ls -la ~/.ssh
```

You may see:

```text
id_ed25519
id_ed25519.pub
```

Remember:

```text
id_ed25519
    ↓
Private key

id_ed25519.pub
    ↓
Public key
```

---

# 14. View Your Public Key

To display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

It may look like:

```text
ssh-ed25519 AAAAC3... user@computer
```

You can copy the public key when setting up a server.

Never copy the private key.

---

# 15. Copy Public Key to a Server

If available on your Linux environment:

```bash
ssh-copy-id user@192.168.1.100
```

This adds your public key to the remote user's:

```text
~/.ssh/authorized_keys
```

After that, you can usually connect using:

```bash
ssh user@192.168.1.100
```

without entering the account password each time, depending on your SSH configuration and key passphrase.

---

# 16. authorized_keys

On the remote server:

```text
~/.ssh/authorized_keys
```

contains public keys that are allowed to authenticate.

Example structure:

```text
Remote Server
│
└── ~/.ssh/
    └── authorized_keys
```

Conceptually:

```text
Your Public Key
       ↓
authorized_keys
       ↓
SSH Server
       ↓
Authentication
```

---

# 17. SSH Key Authentication Flow

The process looks like:

```text
Your Computer
     |
     | Private Key
     ↓
SSH Server
     |
     | checks public key
     ↓
authorized_keys
     |
     ↓
Authentication
     |
     ↓
Remote Shell
```

The private key itself is not simply uploaded to the server.

---

# 18. SSH Directory Permissions

SSH is sensitive to permissions.

On a Linux server:

```bash
chmod 700 ~/.ssh
```

For the authorized keys file:

```bash
chmod 600 ~/.ssh/authorized_keys
```

Check:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

Typical permissions:

```text
~/.ssh
700

authorized_keys
600
```

---

# 19. SSH Config File

SSH can use a configuration file:

```text
~/.ssh/config
```

This allows you to create shortcuts.

Example:

```text
Host myserver
    HostName 192.168.1.100
    User srushti
    Port 22
```

Then instead of:

```bash
ssh srushti@192.168.1.100
```

you can use:

```bash
ssh myserver
```

---

# 20. SSH Config Permissions

Check:

```bash
ls -l ~/.ssh/config
```

You can use:

```bash
chmod 600 ~/.ssh/config
```

---

# 21. SSH to a Local Linux Machine

If you have an SSH server installed locally, you may connect to:

```bash
ssh localhost
```

or:

```bash
ssh username@localhost
```

You can check whether SSH is listening:

```bash
ss -ltn | grep :22
```

---

# 22. Check SSH Service

On Ubuntu/Debian systems:

```bash
systemctl status ssh
```

Some systems use:

```bash
systemctl status sshd
```

If the service is installed but stopped:

```bash
sudo systemctl start ssh
```

or:

```bash
sudo systemctl start sshd
```

The exact service name can vary by Linux distribution.

---

# 23. SSH Server vs SSH Client

This is important.

### SSH Client

The machine that connects.

```text
Your PC
   ↓
ssh server
```

### SSH Server

The machine accepting SSH connections.

```text
Linux Server
   ↑
SSH connection
```

The server normally runs an SSH daemon such as:

```text
sshd
```

---

# 24. Check SSH Client

Run:

```bash
ssh -V
```

Example:

```text
OpenSSH_...
```

This confirms that the SSH client is available.

---

# 25. Verbose SSH Mode

When SSH doesn't work, use:

```bash
ssh -v user@server
```

For more detailed output:

```bash
ssh -vv user@server
```

Maximum debugging:

```bash
ssh -vvv user@server
```

This is very useful for troubleshooting.

---

# 26. Common SSH Connection Problems

If you get:

```text
Connection refused
```

Possible reasons:

* SSH server is not running
* Wrong port
* Firewall is blocking the port
* SSH is listening on another port

Check:

```bash
systemctl status ssh
```

and:

```bash
ss -ltn
```

---

# 27. Connection Timeout

If you get:

```text
Connection timed out
```

Possible reasons:

* Server is unreachable
* Network problem
* Firewall
* Incorrect IP address
* Incorrect port

Test connectivity:

```bash
ping 192.168.1.100
```

Then check the SSH port:

```bash
nc -zv 192.168.1.100 22
```

---

# 28. Permission Denied

You may see:

```text
Permission denied
```

Possible reasons:

* Wrong username
* Incorrect password
* Public key not installed
* Wrong private key
* Incorrect SSH permissions
* SSH server configuration

Use:

```bash
ssh -v user@server
```

to investigate.

---

# 29. Specify a Private Key

If you have multiple SSH keys:

```bash
ssh -i ~/.ssh/myserver_key user@192.168.1.100
```

Here:

```text
-i
```

specifies the private key.

---

# 30. SSH Agent

An SSH agent can remember unlocked private keys during a session.

Check:

```bash
ssh-add -l
```

Add a key:

```bash
ssh-add ~/.ssh/id_ed25519
```

This can prevent repeatedly entering a key passphrase during a session.

---

# 31. Secure Copy — scp

`scp` means:

```text
Secure Copy
```

It transfers files using SSH.

Basic syntax:

```bash
scp file.txt user@server:/home/user/
```

Example:

```bash
scp app.jar user@192.168.1.100:/home/user/java-app/
```

This is useful for Java deployment.

---

# 32. Copy a File from Server to Local Machine

Syntax:

```bash
scp user@server:/remote/path/file.txt .
```

Example:

```bash
scp user@192.168.1.100:/home/user/app.log .
```

The `.` means:

```text
Current directory
```

---

# 33. Copy a Directory

Use:

```bash
scp -r myproject user@192.168.1.100:/home/user/
```

Here:

```text
-r
```

means recursive.

---

# 34. Copy with a Custom SSH Port

If SSH uses port `2222`:

```bash
scp -P 2222 app.jar user@192.168.1.100:/home/user/
```

Notice:

```text
ssh  → -p
scp  → -P
```

---

# 35. SFTP

SFTP means:

```text
SSH File Transfer Protocol
```

Start an SFTP session:

```bash
sftp user@192.168.1.100
```

You may see:

```text
sftp>
```

Now you can work with remote files.

---

# 36. Basic SFTP Commands

List remote files:

```text
ls
```

Change remote directory:

```text
cd /home/user
```

Show local directory:

```text
lpwd
```

Change local directory:

```text
lcd Downloads
```

Download:

```text
get app.jar
```

Upload:

```text
put app.jar
```

Exit:

```text
exit
```

---

# 37. SFTP Example

Connect:

```bash
sftp user@192.168.1.100
```

Then:

```text
sftp> cd /home/user/java-app
sftp> put app.jar
sftp> ls
sftp> exit
```

---

# 38. SSH vs SCP vs SFTP

| Tool   | Purpose                   |
| ------ | ------------------------- |
| `ssh`  | Remote shell/commands     |
| `scp`  | Copy files                |
| `sftp` | Interactive file transfer |

Simple memory trick:

```text
ssh  → work on server
scp  → copy files
sftp → manage file transfers
```

---

# 39. Remote Command Execution

You don't always need an interactive shell.

You can execute a command directly:

```bash
ssh user@server "hostname"
```

Another example:

```bash
ssh user@server "uptime"
```

You can run:

```bash
ssh user@server "df -h"
```

This is useful for automation.

---

# 40. Remote Java Application Management

Suppose your Java application runs on:

```text
192.168.1.100
```

You can connect:

```bash
ssh user@192.168.1.100
```

Check Java:

```bash
java -version
```

Check application:

```bash
ps aux | grep java
```

Check service:

```bash
systemctl status my-java-app
```

Check logs:

```bash
journalctl -u my-java-app -n 50
```

This is a realistic server workflow.

---

# 41. Java Deployment Workflow

A simple deployment workflow can look like:

```text
Local Computer
      |
      | Build
      ↓
app.jar
      |
      | scp
      ↓
Linux Server
      |
      ↓
/home/user/java-app/
      |
      ↓
systemd
      |
      ↓
Java Application
```

For example:

```bash
scp target/app.jar user@server:/home/user/java-app/
```

Then:

```bash
ssh user@server
```

Restart:

```bash
sudo systemctl restart my-java-app
```

Check:

```bash
systemctl status my-java-app
```

Logs:

```bash
journalctl -u my-java-app -n 50
```

---

# 42. SSH Security

SSH provides secure communication, but it should still be configured carefully.

Important practices:

* Use strong authentication
* Prefer SSH keys for server administration
* Protect private keys
* Use a passphrase for private keys
* Keep SSH software updated
* Avoid sharing private keys
* Use firewall rules
* Disable unnecessary services
* Use least privilege
* Don't use `root` login unnecessarily

---

# 43. Private Key Security

Never do this:

```text
Upload private key to GitHub
```

Never commit:

```text
id_ed25519
```

to a Git repository.

Your `.gitignore` can help prevent accidental commits.

Example:

```text
.ssh/
*.pem
*_key
```

Be careful with wildcard patterns and make sure they don't hide files you actually intend to track.

---

# 44. SSH Key Permissions

Your private key should be protected.

Example:

```bash
chmod 600 ~/.ssh/id_ed25519
```

Check:

```bash
ls -l ~/.ssh/id_ed25519
```

The key should not be readable by everyone.

---

# 45. SSH Host Key Warning

Suppose you previously connected to:

```text
192.168.1.100
```

and later receive:

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

Do not blindly ignore this warning.

Possible reasons include:

* Server was rebuilt
* Host key changed
* IP address now points to another machine
* Security issue such as a man-in-the-middle attempt

First verify that the server really changed.

---

# 46. known_hosts Management

View known hosts:

```bash
cat ~/.ssh/known_hosts
```

SSH also provides:

```bash
ssh-keygen -F 192.168.1.100
```

to search for a host entry.

If a known server was legitimately rebuilt, an old entry may need to be removed.

One command commonly used is:

```bash
ssh-keygen -R 192.168.1.100
```

Only do this after confirming the host change is legitimate.

---

# 47. SSH Practice Lab

Use a Linux machine, WSL environment, VM, or another system you are authorized to access.

### Step 1 — Check SSH

```bash
ssh -V
```

### Step 2 — Check your `.ssh` directory

```bash
ls -la ~/.ssh
```

### Step 3 — Generate a practice key if needed

```bash
ssh-keygen -t ed25519
```

### Step 4 — View public key

```bash
cat ~/.ssh/id_ed25519.pub
```

### Step 5 — Check SSH service on a Linux server

```bash
systemctl status ssh
```

or:

```bash
systemctl status sshd
```

### Step 6 — Check port 22

```bash
ss -ltn | grep :22
```

### Step 7 — Test a connection

```bash
ssh username@server-ip
```

Only use a server you own or are explicitly authorized to access.

---

# 48. Mini Project — Remote Java Deployment

## 🎯 Goal

Practice a basic Java deployment workflow using SSH.

### Local machine

Create:

```text
app.jar
```

### Remote server

Create:

```text
/home/user/java-app/
```

### Step 1 — Transfer

```bash
scp app.jar user@server:/home/user/java-app/
```

### Step 2 — Connect

```bash
ssh user@server
```

### Step 3 — Check file

```bash
ls -l /home/user/java-app/
```

### Step 4 — Check Java

```bash
java -version
```

### Step 5 — Run manually

```bash
cd /home/user/java-app
java -jar app.jar
```

### Step 6 — Stop the application

If running in the foreground:

```text
Ctrl + C
```

### Step 7 — Production-style approach

Instead of manually running the JAR, use the Day 27 concept:

```text
systemd
   ↓
my-java-app.service
   ↓
app.jar
```

Then:

```bash
sudo systemctl restart my-java-app
```

---

# 49. Troubleshooting Checklist

When SSH doesn't work:

```text
1. Is the server reachable?
2. Is the IP correct?
3. Is the SSH service running?
4. Is port 22 open?
5. Is the username correct?
6. Is the SSH key correct?
7. Are SSH permissions correct?
8. Is the firewall blocking SSH?
9. Is the SSH server using a custom port?
10. What does ssh -vvv show?
```

Useful commands:

```bash
ping SERVER_IP
```

```bash
nc -zv SERVER_IP 22
```

```bash
systemctl status ssh
```

```bash
ss -ltn
```

```bash
ssh -vvv user@SERVER_IP
```

---

# 50. Important Commands

## SSH

```bash
ssh user@server
ssh -p 2222 user@server
ssh -i ~/.ssh/id_ed25519 user@server
ssh -v user@server
```

## SSH Keys

```bash
ssh-keygen -t ed25519
ssh-add ~/.ssh/id_ed25519
ssh-keygen -F SERVER
ssh-keygen -R SERVER
```

## SCP

```bash
scp file.txt user@server:/path/
scp user@server:/path/file.txt .
scp -r folder user@server:/path/
```

## SFTP

```bash
sftp user@server
```

Inside SFTP:

```text
ls
cd
pwd
lpwd
lcd
get
put
exit
```

---

# 🧠 Day 28 Cheat Sheet

```text
SSH
 ↓
Secure Remote Access
```

### Connect

```bash
ssh user@server
```

### Custom port

```bash
ssh -p 2222 user@server
```

### Generate key

```bash
ssh-keygen -t ed25519
```

### Public key

```bash
cat ~/.ssh/id_ed25519.pub
```

### Copy files

```bash
scp app.jar user@server:/path/
```

### Download

```bash
scp user@server:/path/app.jar .
```

### SFTP

```bash
sftp user@server
```

### Debug

```bash
ssh -vvv user@server
```

### SSH service

```bash
systemctl status ssh
```

### Java deployment

```text
Build
 ↓
app.jar
 ↓
scp
 ↓
Linux Server
 ↓
systemd
 ↓
Java Application
```

---

# 🎤 Interview Questions

### Q1. What is SSH?

SSH stands for Secure Shell. It is a protocol used to securely access and manage remote systems.

---

### Q2. What is the default SSH port?

The standard SSH port is:

```text
22
```

It can be configured differently.

---

### Q3. What is the difference between SSH and SCP?

```text
SSH → remote command/shell access
SCP → secure file copying
```

---

### Q4. What is SFTP?

SFTP is SSH File Transfer Protocol. It provides secure interactive file transfer over SSH.

---

### Q5. What is the difference between a public key and private key?

The public key can be placed on the server, while the private key stays secret on the client.

---

### Q6. Where is the public key usually stored on the server?

Usually in:

```text
~/.ssh/authorized_keys
```

---

### Q7. What is `known_hosts`?

`known_hosts` stores identities/host keys for servers that the SSH client has previously connected to.

---

### Q8. How do you generate an SSH key?

```bash
ssh-keygen -t ed25519
```

---

### Q9. How do you copy a Java JAR to a remote server?

```bash
scp app.jar user@server:/home/user/java-app/
```

---

### Q10. How would you restart a Java application remotely?

First connect:

```bash
ssh user@server
```

Then, if the application is managed by systemd:

```bash
sudo systemctl restart my-java-app
```

---

### Q11. How do you troubleshoot an SSH connection?

I would check the server IP, network connectivity, SSH service, port, username, authentication method, firewall, and finally use verbose mode:

```bash
ssh -vvv user@server
```

---

