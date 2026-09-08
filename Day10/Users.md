# 🐧 DAY 10 — LINUX USERS

> **Java Meets Linux — 30 Day Linux Roadmap**

## 🎯 Today's Goal

By the end of Day 10, you should understand:

* What Linux users are
* Root user
* Normal users
* User IDs (UID)
* Primary and secondary groups
* `whoami`
* `id`
* `who`
* `w`
* `users`
* `useradd`
* `adduser`
* `passwd`
* `usermod`
* `userdel`
* `/etc/passwd`
* `/etc/shadow`
* `/etc/group`
* `su`
* `sudo`
* User management
* User troubleshooting
* Java + Linux user management

---

# 1. 👤 What Is a Linux User?

A Linux user is an account that can interact with the operating system.

Linux uses users to control:

```text
Who can access a system?
Who owns a file?
Who can execute a program?
Who can modify a file?
Who can access a directory?
```

Every process and file is associated with a user.

---

# 2. 🔥 Types of Users

Linux commonly has:

```text
1. Root user
2. Normal users
3. System/service users
```

---

# 3. 👑 Root User

The root user is the superuser.

Root has extremely high privileges.

Root can:

```text
Create users
Delete users
Change ownership
Change permissions
Install software
Modify system files
Start/stop services
Configure the system
```

Root UID is normally:

```text
0
```

Check:

```bash
id root
```

Example:

```text
uid=0(root) gid=0(root) groups=0(root)
```

---

# 4. ⚠️ Why Root Is Dangerous

Root can modify critical system files.

For example:

```bash
rm -rf
```

combined with elevated privileges can cause serious damage.

Therefore:

> Use the minimum privileges necessary.

Prefer:

```bash
sudo command
```

instead of staying logged in as root.

---

# 5. 👤 Normal User

A normal user has limited privileges.

Example:

```text
srushti
rahul
developer
```

Check your current user:

```bash
whoami
```

Example:

```text
srushti
```

---

# 6. 🆔 UID

UID means:

> User ID

Every Linux user has a numeric UID.

Run:

```bash
id
```

Example:

```text
uid=1000(srushti) gid=1000(srushti) groups=1000(srushti)
```

Here:

```text
UID = 1000
```

Root:

```text
UID = 0
```

---

# 7. 🔍 `id`

The `id` command displays information about a user.

Run:

```bash
id
```

Example:

```text
uid=1000(srushti) gid=1000(srushti) groups=1000(srushti),27(sudo)
```

This shows:

```text
UID
GID
Groups
```

---

# 8. 🔎 Check Another User

Syntax:

```bash
id username
```

Example:

```bash
id root
```

or:

```bash
id srushti
```

---

# 9. 👀 `whoami`

Use:

```bash
whoami
```

It tells you:

> Which user am I currently using?

Example:

```text
srushti
```

Very useful when working with:

```text
sudo
su
SSH
servers
```

---

# 10. 👥 `who`

The `who` command shows logged-in users.

```bash
who
```

Example:

```text
srushti  pts/0  2026-09-08 20:10
rahul    pts/1  2026-09-08 20:20
```

---

# 11. 🖥️ `w`

Another useful command:

```bash
w
```

It provides information about:

```text
Logged-in users
Login time
Idle time
Current activity
```

Example:

```text
USER     TTY      FROM       LOGIN@
srushti  pts/0    -          20:10
```

---

# 12. 👤 `users`

Run:

```bash
users
```

It displays usernames of currently logged-in users.

Example:

```text
srushti rahul
```

---

# 13. 📁 `/etc/passwd`

One of the most important Linux user files is:

```text
/etc/passwd
```

View it:

```bash
cat /etc/passwd
```

You may see:

```text
root:x:0:0:root:/root:/bin/bash
```

Another user might look like:

```text
srushti:x:1000:1000:Srushti:/home/srushti:/bin/bash
```

---

# 14. 🧩 `/etc/passwd` Structure

A line contains seven fields:

```text
username:password:UID:GID:GECOS:home:shell
```

Example:

```text
srushti:x:1000:1000:Srushti:/home/srushti:/bin/bash
```

Breakdown:

```text
username = srushti
password = x
UID      = 1000
GID      = 1000
GECOS    = Srushti
home     = /home/srushti
shell    = /bin/bash
```

---

# 15. 🔐 Why Is Password Shown as `x`?

Modern Linux systems generally don't store the actual password in `/etc/passwd`.

Instead:

```text
/etc/passwd
      ↓
User account information

/etc/shadow
      ↓
Password hashes and related password information
```

---

# 16. 🔒 `/etc/shadow`

View:

```bash
sudo cat /etc/shadow
```

⚠️ Access to this file is restricted.

Never share its contents publicly.

It contains sensitive authentication information.

---

# 17. 👥 `/etc/group`

Linux group information is stored in:

```text
/etc/group
```

View:

```bash
cat /etc/group
```

Example:

```text
developers:x:1001:srushti,rahul
```

Meaning:

```text
Group = developers

Members:
srushti
rahul
```

---

# 18. 🆕 Creating a User with `useradd`

Basic syntax:

```bash
sudo useradd username
```

Example:

```bash
sudo useradd rahul
```

This creates a user account.

---

# 19. 🏠 Creating a User with Home Directory

Use:

```bash
sudo useradd -m rahul
```

`-m` means:

> Create the user's home directory.

Typically:

```text
/home/rahul
```

---

# 20. 🐚 Specify Login Shell

You can specify a shell:

```bash
sudo useradd -m -s /bin/bash rahul
```

This creates:

```text
User = rahul
Home = /home/rahul
Shell = /bin/bash
```

---

# 21. 🔑 Set Password

After creating the user:

```bash
sudo passwd rahul
```

Linux asks you to enter a password.

Example:

```text
New password:
Retype new password:
```

---

# 22. 🔥 Complete User Creation

A common workflow:

```bash
sudo useradd -m -s /bin/bash developer
```

Then:

```bash
sudo passwd developer
```

Verify:

```bash
id developer
```

---

# 23. 🧑‍💻 `adduser`

Some Linux distributions provide:

```bash
adduser
```

Example:

```bash
sudo adduser developer
```

It provides a more interactive process.

You may be asked for:

```text
Password
Full name
Room number
Phone
Other information
```

For beginners, `adduser` can be easier to understand.

---

# 24. `useradd` vs `adduser`

| Command   | Description                                          |
| --------- | ---------------------------------------------------- |
| `useradd` | Lower-level user creation utility                    |
| `adduser` | More interactive helper on many Debian-based systems |

For scripting and automation, `useradd` is commonly useful.

---

# 25. 🔧 Modify a User with `usermod`

`usermod` modifies an existing user.

Example:

```bash
sudo usermod -s /bin/bash developer
```

This changes the user's login shell.

---

# 26. 🏠 Change Home Directory

Example:

```bash
sudo usermod -d /home/newhome developer
```

To move existing home contents too:

```bash
sudo usermod -d /home/newhome -m developer
```

Use such commands carefully.

---

# 27. 👥 Add User to a Group

Syntax:

```bash
sudo usermod -aG group username
```

Example:

```bash
sudo usermod -aG developers srushti
```

Important:

```text
-a = append
-G = supplementary groups
```

### ⚠️ Important

Do not accidentally use:

```bash
usermod -G developers srushti
```

without understanding the consequences.

`-G` can replace the user's supplementary group list.

The safer common form is:

```bash
sudo usermod -aG developers srushti
```

---

# 28. 🔍 Verify Group Membership

Run:

```bash
groups srushti
```

or:

```bash
id srushti
```

---

# 29. ❌ Delete a User

Basic command:

```bash
sudo userdel developer
```

This removes the user account.

---

# 30. 🏠 Delete User + Home Directory

```bash
sudo userdel -r developer
```

`-r` removes the user's home directory and related mail spool where applicable.

⚠️ This is destructive.

Always verify the username before running it.

---

# 31. 🔐 `su`

`su` means:

> Switch User

Example:

```bash
su rahul
```

You may be asked for the user's password.

To switch to a login shell:

```bash
su - rahul
```

The `-` loads the target user's login environment.

---

# 32. 🔥 `sudo`

`sudo` means:

> Execute a command with elevated privileges.

Example:

```bash
sudo useradd developer
```

Instead of switching to root, you can execute only the required command with elevated privileges.

---

# 33. `su` vs `sudo`

| `su`                                                   | `sudo`                                            |
| ------------------------------------------------------ | ------------------------------------------------- |
| Switches user                                          | Executes a command with elevated privileges       |
| Can open another user's shell                          | Usually runs one command                          |
| Requires target user's password in many configurations | Usually uses invoking user's authentication       |
| Changes identity for the session                       | Privilege elevation is typically command-specific |

---

# 34. 🧪 Practice Environment

> Run these commands inside Linux/WSL/VM. Don't run Linux user-management commands in normal Windows PowerShell.

Create a test user:

```bash
sudo useradd -m linuxstudent
```

Set password:

```bash
sudo passwd linuxstudent
```

Check:

```bash
id linuxstudent
```

Check:

```bash
grep linuxstudent /etc/passwd
```

---

# 35. 🔎 Find a User in `/etc/passwd`

Instead of reading the entire file:

```bash
grep linuxstudent /etc/passwd
```

Example:

```text
linuxstudent:x:1001:1001::/home/linuxstudent:/bin/bash
```

---

# 36. 🏠 Check Home Directory

```bash
ls -ld /home/linuxstudent
```

You should see ownership similar to:

```text
drwx------ linuxstudent linuxstudent /home/linuxstudent
```

Exact permissions may vary depending on distribution/configuration.

---

# 37. 👥 Create a Group

Groups will be covered deeply on **Day 11**, but you can preview:

```bash
sudo groupadd developers
```

Then:

```bash
sudo usermod -aG developers linuxstudent
```

Verify:

```bash
groups linuxstudent
```

---

# 38. 🔐 User + Group + Permissions

Remember the relationship:

```text
User
 ↓
Groups
 ↓
File Ownership
 ↓
Permissions
 ↓
Access
```

Example:

```text
File:
application.log

Owner:
javaapp

Group:
developers

Permissions:
-rw-r-----
```

This determines who can access the file.

---

# 39. ☕ Java Developer Connection

On a Linux server, you may have:

```text
javaapp
deploy
developer
```

For example:

```text
javaapp
   ↓
Runs Java application

deploy
   ↓
Deploys application

developer
   ↓
Maintains source/configuration
```

Separating users can improve security.

---

# 40. 🏢 Real-World Java Server Example

Imagine:

```text
/opt/student-app/
├── app.jar
├── config/
├── logs/
└── scripts/
```

The Java application could run under a dedicated account:

```text
studentapp
```

Instead of running the application as:

```text
root
```

This follows the principle:

> Don't run applications with unnecessary privileges.

---

# 41. 🔍 Find Which User Runs a Java Process

Later, when you learn process management, you can inspect processes.

For example:

```bash
ps aux | grep java
```

You might see:

```text
studentapp  1234  ... java -jar app.jar
```

Here:

```text
studentapp
```

is the user running the Java process.

This becomes very useful when troubleshooting:

```text
Permission denied
Cannot write log
Cannot read config
Cannot access directory
```

---

# 42. 🚨 Common Mistakes

### Mistake 1

Creating a user without a home directory:

```bash
sudo useradd developer
```

Better when you need a normal login account:

```bash
sudo useradd -m developer
```

---

### Mistake 2

Forgetting to set a password:

```bash
sudo useradd -m developer
```

Then:

```bash
sudo passwd developer
```

---

### Mistake 3

Incorrect group modification:

```bash
usermod -G developers developer
```

Prefer:

```bash
sudo usermod -aG developers developer
```

when adding a supplementary group.

---

### Mistake 4

Deleting the wrong user:

```bash
sudo userdel -r username
```

Always verify first:

```bash
id username
```

---

### Mistake 5

Running everything as root.

Avoid unnecessary root access.

---

# 43. 🧪 Mini Project — Linux Developer Accounts

Create two test users:

```bash
sudo useradd -m developer1
sudo useradd -m developer2
```

Set passwords:

```bash
sudo passwd developer1
sudo passwd developer2
```

Verify:

```bash
id developer1
id developer2
```

Check:

```bash
grep developer /etc/passwd
```

Check home directories:

```bash
ls -ld /home/developer1
ls -ld /home/developer2
```

---

# 44. 🎯 Practice Challenge

Complete the following:

### Task 1

Create:

```text
linuxdev
```

### Task 2

Give it a home directory.

### Task 3

Set a password.

### Task 4

Verify its UID.

### Task 5

Find its `/etc/passwd` entry.

### Task 6

Check its home directory ownership.

### Task 7

Create:

```text
developers
```

group.

### Task 8

Add `linuxdev` to the group.

### Task 9

Verify:

```bash
groups linuxdev
```

### Task 10

Delete the test user and its home directory after practice.

---

# 45. 🧠 Important Commands

| Command           | Purpose                          |
| ----------------- | -------------------------------- |
| `whoami`          | Current username                 |
| `id`              | UID, GID and groups              |
| `who`             | Logged-in users                  |
| `w`               | Logged-in users + activity       |
| `users`           | Current logged-in usernames      |
| `useradd`         | Create user                      |
| `adduser`         | Interactive user creation        |
| `passwd`          | Set/change password              |
| `usermod`         | Modify user                      |
| `userdel`         | Delete user                      |
| `su`              | Switch user                      |
| `sudo`            | Execute with elevated privileges |
| `cat /etc/passwd` | User account information         |
| `cat /etc/group`  | Group information                |

---

# 46. ⚡ Cheat Sheet

```bash
# Current user
whoami

# Current user details
id

# Check another user
id username

# Logged-in users
who

# User activity
w

# Create user
sudo useradd -m username

# Set password
sudo passwd username

# Modify shell
sudo usermod -s /bin/bash username

# Add to group
sudo usermod -aG group username

# Delete user
sudo userdel username

# Delete user + home
sudo userdel -r username

# Switch user
su - username

# Elevated command
sudo command

# Check passwd entry
grep username /etc/passwd

# Check groups
groups username
```

---

# 47. 🎤 Interview Questions

### Q1. What is a Linux user?

A Linux user is an account used to identify and control access to system resources.

### Q2. What is root?

Root is the Linux superuser with UID `0` and extensive system privileges.

### Q3. What is UID?

UID stands for User ID and uniquely identifies a user.

### Q4. Where is Linux user information stored?

Primarily in:

```text
/etc/passwd
```

### Q5. Where are password hashes stored?

Typically:

```text
/etc/shadow
```

### Q6. What does `useradd -m` do?

It creates the user's home directory.

### Q7. What does `usermod -aG` do?

It adds a user to a supplementary group without replacing their existing supplementary group memberships.

### Q8. Difference between `su` and `sudo`?

`su` switches to another user, while `sudo` generally executes a command with elevated privileges.

### Q9. What is UID 0?

UID `0` normally represents the root user.

### Q10. Why should applications not normally run as root?

Because a compromised application would have unnecessarily broad privileges, increasing the potential impact of a security issue.

---
