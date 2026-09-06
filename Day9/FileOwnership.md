# 🐧 DAY 09 — FILE OWNERSHIP

> **Java Meets Linux — 30 Day Linux Roadmap**

## 🎯 Today's Goal

By the end of Day 9, you should understand:

* What file ownership means
* Users and groups in Linux
* How to check file ownership
* `chown`
* `chgrp`
* Changing owner
* Changing group
* Changing owner + group together
* Recursive ownership
* Ownership vs permissions
* `sudo`
* Troubleshooting ownership problems
* Real-world ownership scenarios
* Interview questions

---

# 1. 🔐 What Is File Ownership?

Every file and directory in Linux has an **owner**.

Linux mainly tracks:

```text
Owner
Group
Others
```

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 srushti developers 1200 notes.txt
```

Breakdown:

```text
-rw-r--r-- 1 srushti developers 1200 notes.txt
          │       │
          │       └── Group
          └────────── Owner
```

So:

```text
Owner  = srushti
Group  = developers
```

---

# 2. 👤 User

A **user** is an account on a Linux system.

Example:

```text
root
srushti
developer
admin
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

# 3. 👥 Group

A group is a collection of users.

Example:

```text
developers
```

could contain:

```text
srushti
rahul
amit
```

Groups make permission management easier.

Instead of giving access to 10 users individually, you can give access to a group.

---

# 4. 🔎 Check File Ownership

Use:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 srushti developers 2500 app.log
```

Here:

```text
Owner = srushti
Group = developers
```

---

# 5. 🧠 Understanding `ls -l`

Example:

```text
-rwxr-xr-- 1 srushti developers 4096 script.sh
```

Break it down:

```text
-rwxr-xr-- 
 │││ │││ │││
 │││ │││ ││└── Others
 │││ │││ └──── Others
 │││ ││└────── Group
 │││ │└─────── Group
 │││ └──────── Group
 ││└────────── Owner
 │└─────────── Owner
 └──────────── File type
```

More simply:

```text
-rwxr-xr--
 │   │   │
 │   │   └── Others
 │   └────── Group
 └────────── Owner
```

Meaning:

```text
Owner  = rwx
Group  = r-x
Others = r--
```

---

# 6. 🛠️ `chown`

`chown` means:

> **change owner**

Syntax:

```bash
chown USER FILE
```

Example:

```bash
chown rahul notes.txt
```

Now:

```text
Owner = rahul
```

---

# 7. 🔥 Example

Create a file:

```bash
touch app.log
```

Check:

```bash
ls -l app.log
```

Example:

```text
-rw-r--r-- 1 srushti developers 0 app.log
```

Change owner:

```bash
sudo chown rahul app.log
```

Check again:

```bash
ls -l app.log
```

Now:

```text
-rw-r--r-- 1 rahul developers 0 app.log
```

---

# 8. ⚠️ Why `sudo`?

Changing ownership of files you don't own generally requires elevated privileges.

Example:

```bash
sudo chown rahul app.log
```

`sudo` means:

> Execute this command with elevated privileges.

You may be asked for your Linux password.

---

# 9. 👥 Changing Group

Use:

```bash
chgrp
```

Syntax:

```bash
chgrp GROUP FILE
```

Example:

```bash
chgrp developers app.log
```

Check:

```bash
ls -l app.log
```

---

# 10. 🔥 Change Owner AND Group

You can use `chown` for both.

Syntax:

```bash
chown USER:GROUP FILE
```

Example:

```bash
sudo chown srushti:developers app.log
```

Now:

```text
Owner = srushti
Group = developers
```

---

# 11. 📁 Ownership of Directories

Ownership applies to directories too.

Create:

```bash
mkdir project
```

Check:

```bash
ls -ld project
```

Example:

```text
drwxr-xr-x 2 srushti developers 4096 project
```

Notice:

```text
d
```

means directory.

---

# 12. 📌 `ls -ld`

Use:

```bash
ls -ld project
```

This shows information about the directory itself.

Compare:

```bash
ls -l project
```

and:

```bash
ls -ld project
```

### `ls -l project`

Shows contents.

### `ls -ld project`

Shows the directory itself.

---

# 13. 🔄 Recursive Ownership

Suppose you have:

```text
project/
├── src/
├── logs/
├── config/
└── README.md
```

You want to change ownership of everything.

Use:

```bash
sudo chown -R rahul:developers project/
```

`-R` means:

```text
Recursive
```

It applies the change to:

```text
project/
src/
logs/
config/
README.md
```

⚠️ Be careful with recursive ownership commands.

Never casually run:

```bash
sudo chown -R ...
```

on system directories.

---

# 14. 🧪 Create a Practice Environment

Create:

```bash
mkdir Day9-Ownership
cd Day9-Ownership
```

Create files:

```bash
touch app.log
touch config.txt
touch data.txt
```

Create directories:

```bash
mkdir application
mkdir logs
```

Check:

```bash
ls -la
```

---

# 15. 🔍 Check Ownership

Run:

```bash
ls -l
```

Then:

```bash
ls -ld application
```

You should see your username and group.

---

# 16. 📊 Using `stat`

Another useful command is:

```bash
stat app.log
```

Example:

```text
File: app.log
Size: 0
Uid: 1000
Gid: 1000
Access: (0644/-rw-r--r--)
```

Important information includes:

```text
Uid
Gid
Access
```

---

# 17. 🧠 UID

UID means:

> User ID

Every Linux user has a numerical ID.

Example:

```text
Uid: 1000
```

You can check your UID with:

```bash
id
```

Example:

```text
uid=1000(srushti) gid=1000(srushti)
```

---

# 18. 🧠 GID

GID means:

> Group ID

Example:

```text
gid=1000(srushti)
```

So Linux internally works with numerical IDs.

Conceptually:

```text
User
 ↓
UID

Group
 ↓
GID
```

---

# 19. 🔎 Check User Information

Run:

```bash
id
```

Example:

```text
uid=1000(srushti) gid=1000(srushti) groups=1000(srushti),27(sudo)
```

This tells you:

* User ID
* Primary group ID
* Groups the user belongs to

---

# 20. 👥 Check Groups

Run:

```bash
groups
```

Example:

```text
srushti sudo developers
```

This tells you which groups your current user belongs to.

---

# 21. 🔐 Ownership vs Permissions

This is extremely important.

Ownership answers:

> **Who owns the file?**

Permissions answer:

> **What can they do?**

Example:

```text
-rwxr-xr--
```

with:

```text
Owner = srushti
Group = developers
```

Means:

```text
srushti
   ↓
rwx

developers
   ↓
r-x

others
   ↓
r--
```

---

# 22. 🧩 Example

Suppose:

```text
-rw-r----- 1 srushti developers app.log
```

Meaning:

```text
Owner:
rw-

Group:
r--

Others:
---
```

So:

```text
srushti → read + write

developers → read

others → no access
```

---

# 23. 🔥 Ownership + Permissions Together

Consider:

```text
-rwxr-x--- 1 srushti developers deploy.sh
```

Owner:

```text
srushti
```

Permissions:

```text
rwx
```

Group:

```text
developers
```

Group permissions:

```text
r-x
```

Others:

```text
---
```

Therefore:

```text
Owner    → read + write + execute
Group    → read + execute
Others   → no permission
```

---

# 24. 🔧 Change Owner

Syntax:

```bash
sudo chown username filename
```

Example:

```bash
sudo chown rahul app.log
```

---

# 25. 🔧 Change Group

Syntax:

```bash
sudo chgrp groupname filename
```

Example:

```bash
sudo chgrp developers app.log
```

---

# 26. 🔧 Change Owner + Group

Syntax:

```bash
sudo chown username:groupname filename
```

Example:

```bash
sudo chown rahul:developers app.log
```

---

# 27. 📁 Change Directory Ownership

```bash
sudo chown rahul:developers project/
```

---

# 28. 📁 Recursive Ownership

```bash
sudo chown -R rahul:developers project/
```

Use this carefully.

---

# 29. 🔍 Verify Changes

After changing ownership:

```bash
ls -l
```

or:

```bash
stat app.log
```

Never assume the command worked.

Always verify.

---

# 30. 🚨 Common Ownership Problem

Suppose you see:

```text
Permission denied
```

Don't immediately use:

```bash
sudo
```

First inspect:

```bash
ls -l filename
```

Then:

```bash
id
```

Ask:

```text
Who owns the file?
What group owns it?
What permissions exist?
Which user am I?
```

---

# 31. 🧠 Troubleshooting Workflow

When access fails:

```text
Permission denied
       ↓
ls -l file
       ↓
Check owner
       ↓
Check group
       ↓
Check permissions
       ↓
id
       ↓
Check current user/groups
       ↓
Fix ownership or permissions if appropriate
```

---

# 32. 🏢 Real-World Example

Imagine a Java application:

```text
/opt/myapp/
├── app.jar
├── config/
├── logs/
└── scripts/
```

A deployment user owns the application:

```text
deploy:developers
```

You might have:

```text
app.jar
deploy:developers

config/
deploy:developers

logs/
appuser:developers
```

Different ownership can provide controlled access.

---

# 33. ☕ Java Developer Connection

Suppose your Java application writes:

```text
logs/application.log
```

If the Java process doesn't have permission to write the log file, the application may fail to create or update logs.

You may investigate:

```bash
ls -l logs/
```

Then:

```bash
ps
```

and:

```bash
id
```

This is a practical Linux skill for Java developers working with servers.

---

# 34. 🛠️ Mini Project — Application Ownership

Create:

```bash
mkdir JavaApp
cd JavaApp
```

Create:

```bash
touch app.jar
touch application.log
touch application.properties
```

Create directories:

```bash
mkdir config
mkdir logs
mkdir scripts
```

Check:

```bash
ls -la
```

Check directory ownership:

```bash
ls -ld config logs scripts
```

Check file ownership:

```bash
ls -l
```

Inspect:

```bash
stat app.jar
```

---

# 35. 🎯 Practice Tasks

Complete these tasks:

### Task 1

Create:

```text
project/
```

### Task 2

Inside it create:

```text
src/
logs/
config/
```

### Task 3

Create:

```text
src/Main.java
logs/app.log
config/application.conf
```

### Task 4

Check ownership:

```bash
ls -l
```

### Task 5

Check directory ownership:

```bash
ls -ld src logs config
```

### Task 6

Check user information:

```bash
id
```

### Task 7

Check groups:

```bash
groups
```

### Task 8

Inspect a file:

```bash
stat logs/app.log
```

### Task 9

If you have a test user and group, practice:

```bash
sudo chown testuser logs/app.log
```

Then:

```bash
ls -l logs/app.log
```

---

# 36. 🧠 Important Commands

| Command  | Purpose                      |
| -------- | ---------------------------- |
| `ls -l`  | View owner/group             |
| `ls -ld` | View directory itself        |
| `id`     | User and group IDs           |
| `groups` | User's groups                |
| `stat`   | Detailed file information    |
| `chown`  | Change owner                 |
| `chgrp`  | Change group                 |
| `sudo`   | Run with elevated privileges |

---

# 37. ⚡ Quick Cheat Sheet

```bash
# Current user
whoami

# User information
id

# Groups
groups

# View ownership
ls -l

# Directory ownership
ls -ld directory

# Detailed information
stat file

# Change owner
sudo chown user file

# Change group
sudo chgrp group file

# Change owner + group
sudo chown user:group file

# Recursive ownership
sudo chown -R user:group directory/
```

---

# 38. 🎤 Interview Questions

### Q1. What is file ownership in Linux?

Every Linux file and directory has an associated owner and group that determine how permissions are applied.

---

### Q2. How do you check file ownership?

```bash
ls -l filename
```

or:

```bash
stat filename
```

---

### Q3. What is `chown`?

`chown` changes the owner of a file or directory.

Example:

```bash
sudo chown rahul file.txt
```

---

### Q4. What is `chgrp`?

`chgrp` changes the group associated with a file or directory.

```bash
sudo chgrp developers file.txt
```

---

### Q5. How do you change owner and group together?

```bash
sudo chown rahul:developers file.txt
```

---

### Q6. What does `-R` mean?

`-R` means recursive.

Example:

```bash
sudo chown -R rahul:developers project/
```

It applies the ownership change to the directory and its contents.

---

### Q7. What is UID?

UID stands for **User ID**.

It uniquely identifies a Linux user.

---

### Q8. What is GID?

GID stands for **Group ID**.

It identifies a Linux group.

---

### Q9. Difference between ownership and permissions?

**Ownership** identifies who owns the file.

**Permissions** define what the owner, group, and others can do.

---

### Q10. Why should `sudo` be used carefully?

Because it provides elevated privileges. An incorrect command can modify or damage important system files.

---

# 39. 🧠 Day 9 Challenge

Without looking at the notes, explain:

```text
-rwxr-x---
```

Then answer:

```text
Who owns this file?
Which group owns it?
How can you check ownership?
How can you change owner?
How can you change group?
How can you change both?
What does -R mean?
What is UID?
What is GID?
```

If you can answer all of these, **Day 9 is complete.** 💪

---

# 40. 🔥 Java + Linux Connection

As a Java developer, you'll commonly encounter:

```text
Java Application
       ↓
Linux Server
       ↓
Application User
       ↓
Files / Logs / Config
       ↓
Ownership
       ↓
Permissions
```

Understanding ownership helps you troubleshoot:

* Java log files
* deployment problems
* configuration files
* application directories
* server access
* CI/CD deployments
* production permission errors

---




