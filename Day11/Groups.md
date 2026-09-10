# 🐧 DAY 11 — LINUX GROUPS

> **Java Meets Linux — 30 Day Linux Roadmap**

## 🎯 Today's Goal

By the end of Day 11, you should understand:

* What Linux groups are
* Why groups are needed
* Primary groups
* Secondary/supplementary groups
* `groupadd`
* `groupdel`
* `groupmod`
* `groups`
* `id`
* `/etc/group`
* `/etc/gshadow`
* Adding users to groups
* Removing users from groups
* Group ownership
* Group permissions
* Users + Groups + Ownership + Permissions
* Real-world Java server examples
* Group-management troubleshooting

---

# 1. 👥 What Is a Linux Group?

A Linux group is a collection of users.

Groups are mainly used to manage access to files, directories, and system resources.

Instead of giving permissions to users one by one:

```text
srushti
rahul
amit
neha
```

you can create:

```text
developers
```

and put everyone into that group.

Then give permissions to:

```text
developers
```

---

# 2. 💡 Why Are Groups Useful?

Imagine a Java development team:

```text
srushti
rahul
amit
```

All developers need access to:

```text
/opt/javaapp/
```

Instead of configuring each user separately:

```text
srushti → access
rahul   → access
amit    → access
```

Create:

```text
developers
```

Then:

```text
developers → access
```

Now anyone added to the group can receive the group permissions.

---

# 3. 🧠 Users + Groups

Think of Linux access like this:

```text
                Linux System
                     |
          +----------+----------+
          |                     |
        Users                 Groups
          |                     |
     srushti                 developers
     rahul                   testers
     amit                    admins
```

A user can belong to multiple groups.

---

# 4. 👤 Check Your Groups

Run:

```bash
groups
```

Example:

```text
srushti sudo developers
```

This means the current user belongs to:

```text
srushti
sudo
developers
```

---

# 5. 🔍 Use `id`

Run:

```bash
id
```

Example:

```text
uid=1000(srushti) gid=1000(srushti) groups=1000(srushti),27(sudo),1001(developers)
```

This shows:

```text
UID
Primary GID
Supplementary Groups
```

---

# 6. 🆔 Primary Group

Every Linux user normally has a primary group.

Example:

```text
User:
srushti

Primary group:
srushti
```

Check:

```bash
id srushti
```

Example:

```text
uid=1000(srushti)
gid=1000(srushti)
```

Here:

```text
Primary group = srushti
```

---

# 7. 👥 Secondary / Supplementary Groups

A user can also belong to additional groups.

Example:

```text
srushti
   |
   +── srushti
   +── sudo
   +── developers
   +── docker
```

These additional groups are commonly called:

> Supplementary groups

They provide additional access.

---

# 8. 🆕 Create a Group

Use:

```bash
sudo groupadd developers
```

This creates:

```text
developers
```

---

# 9. 🔎 Verify the Group

Use:

```bash
grep developers /etc/group
```

Example:

```text
developers:x:1001:
```

You can also use:

```bash
getent group developers
```

Example:

```text
developers:x:1001:srushti,rahul
```

---

# 10. 📁 `/etc/group`

Linux group information is commonly stored in:

```text
/etc/group
```

View it:

```bash
cat /etc/group
```

Example:

```text
developers:x:1001:srushti,rahul
```

---

# 11. 🧩 `/etc/group` Structure

The general format is:

```text
group_name:password:GID:members
```

Example:

```text
developers:x:1001:srushti,rahul
```

Breakdown:

```text
Group name = developers
Password field = x
GID = 1001
Members = srushti, rahul
```

---

# 12. 🔐 `/etc/gshadow`

Linux also has:

```text
/etc/gshadow
```

It contains sensitive group authentication information.

View only when necessary:

```bash
sudo cat /etc/gshadow
```

⚠️ Never share its contents publicly.

---

# 13. ➕ Add User to Group

Use:

```bash
sudo usermod -aG developers srushti
```

Meaning:

```text
-a → append
-G → supplementary groups
```

So:

```text
srushti
   ↓
developers
```

---

# 14. 🔎 Verify Group Membership

Run:

```bash
groups srushti
```

or:

```bash
id srushti
```

You should see:

```text
developers
```

---

# 15. ⚠️ Important `usermod` Rule

Prefer:

```bash
sudo usermod -aG developers srushti
```

instead of blindly using:

```bash
sudo usermod -G developers srushti
```

Why?

Because `-G` specifies the supplementary group list, while `-aG` adds the group to the existing supplementary memberships.

---

# 16. 🔄 Multiple Groups

Suppose:

```text
srushti
```

belongs to:

```text
developers
testers
docker
```

You can add another:

```bash
sudo usermod -aG admins srushti
```

Now:

```text
developers
testers
docker
admins
```

remain available as supplementary memberships.

---

# 17. ❌ Remove User From Group

On many Linux systems, a convenient command is:

```bash
sudo gpasswd -d srushti developers
```

This removes:

```text
srushti
```

from:

```text
developers
```

Verify:

```bash
groups srushti
```

---

# 18. 🗑️ Delete a Group

Use:

```bash
sudo groupdel developers
```

⚠️ Make sure you actually want to remove the group.

Deleting a group does not automatically mean that files associated with that numeric GID are deleted.

---

# 19. ✏️ Rename a Group

Use:

```bash
sudo groupmod -n programmers developers
```

This changes:

```text
developers
```

to:

```text
programmers
```

---

# 20. 🆔 Change Group ID

A group has a GID.

You can change it with:

```bash
sudo groupmod -g 1050 developers
```

⚠️ Changing GIDs can have consequences for existing files that reference the old numeric GID.

Don't do this casually on a production system.

---

# 21. 🔍 Check Group Details

Use:

```bash
getent group developers
```

This is often preferable to directly reading `/etc/group` because it asks the system's configured account databases.

Example:

```text
developers:x:1001:srushti,rahul
```

---

# 22. 📂 Group Ownership of Files

Example:

```bash
ls -l app.log
```

Output:

```text
-rw-r----- 1 srushti developers 1200 app.log
```

Here:

```text
Owner = srushti
Group = developers
```

The group:

```text
developers
```

gets the permissions in the middle permission block:

```text
r--
```

---

# 23. 🔐 Group Permissions

Consider:

```text
-rw-r-----
```

Breakdown:

```text
Owner:
rw-

Group:
r--

Others:
---
```

If:

```text
Owner = srushti
Group = developers
```

then:

```text
srushti     → read + write
developers  → read
others      → no access
```

---

# 24. 🧠 Groups + Permissions

The complete model:

```text
User
 ↓
Group membership
 ↓
File ownership
 ↓
Permission bits
 ↓
Access decision
```

Example:

```text
User:
rahul

Group:
developers

File:
app.log

Owner:
srushti

Group:
developers

Permissions:
-rw-r-----
```

Rahul can read the file because he belongs to:

```text
developers
```

and the group has:

```text
r--
```

---

# 25. 📁 Directory Groups

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

The directory belongs to:

```text
srushti:developers
```

---

# 26. 🔧 Change Group Ownership

Use:

```bash
sudo chgrp developers project
```

Or:

```bash
sudo chown :developers project
```

Check:

```bash
ls -ld project
```

---

# 27. 🔄 Recursive Group Ownership

For a directory:

```bash
sudo chgrp -R developers project/
```

This changes the group ownership of:

```text
project/
├── src/
├── logs/
├── config/
└── files
```

⚠️ Use recursive ownership commands carefully.

---

# 28. 🧪 Practice Environment

Create:

```bash
mkdir Day11-Groups
cd Day11-Groups
```

Create files:

```bash
touch app.log
touch config.txt
touch developers.txt
```

Create directories:

```bash
mkdir project
mkdir logs
mkdir shared
```

Check:

```bash
ls -la
```

---

# 29. 👥 Create Test Groups

Create:

```bash
sudo groupadd developers
sudo groupadd testers
```

Verify:

```bash
getent group developers
getent group testers
```

---

# 30. 👤 Create Test Users

If you don't already have test users:

```bash
sudo useradd -m developer1
sudo useradd -m developer2
```

Set passwords:

```bash
sudo passwd developer1
sudo passwd developer2
```

---

# 31. ➕ Add Users to Developers

Run:

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
```

Verify:

```bash
groups developer1
groups developer2
```

---

# 32. 🧪 Add One User to Testers

```bash
sudo usermod -aG testers developer2
```

Now:

```text
developer1
   ↓
developers

developer2
   ↓
developers
testers
```

Check:

```bash
id developer2
```

---

# 33. 📁 Shared Project

Create:

```bash
mkdir shared-project
```

Change group:

```bash
sudo chgrp developers shared-project
```

Check:

```bash
ls -ld shared-project
```

Example:

```text
drwxr-xr-x 2 srushti developers 4096 shared-project
```

---

# 34. 🔐 Give Group Access

Suppose:

```text
Owner:
rwx

Group:
rwx

Others:
---
```

Permission:

```text
770
```

Set:

```bash
sudo chmod 770 shared-project
```

Now:

```text
Owner:
rwx

Group:
rwx

Others:
---
```

So members of:

```text
developers
```

can access the directory according to those permissions.

---

# 35. 🧠 Why `770`?

Remember:

```text
7 = rwx
7 = rwx
0 = ---
```

Therefore:

```text
770

Owner  → rwx
Group  → rwx
Others → ---
```

---

# 36. ☕ Java Team Example

Imagine a Java project:

```text
/opt/javaapp/
├── app.jar
├── config/
├── logs/
└── scripts/
```

Create group:

```text
javaapp
```

Developers:

```text
srushti
rahul
amit
```

are members of:

```text
javaapp
```

Then:

```text
/opt/javaapp/
Owner = javaappuser
Group = javaapp
```

Group permissions can allow developers to manage required files without giving everyone root access.

---

# 37. 🏢 Production Example

A server may have:

```text
javaapp
deploy
developers
admins
```

Possible responsibilities:

```text
javaapp
   ↓
Runs application

deploy
   ↓
Deployment account

developers
   ↓
Application maintenance

admins
   ↓
System administration
```

This separation improves security and organization.

---

# 38. 🔥 Group + Java Logs

Suppose Java creates:

```text
/var/log/javaapp/application.log
```

You might configure ownership like:

```text
javaapp:developers
```

Then permissions can allow:

```text
javaapp → write
developers → read
others → no access
```

Conceptually:

```text
-rw-r----- javaapp developers application.log
```

This is a common type of Linux access-control scenario.

---

# 39. ⚠️ Group Membership Changes

After adding a user to a group:

```bash
sudo usermod -aG developers srushti
```

an already-running login session may not immediately reflect the new group membership.

You can start a new login session, or use:

```bash
newgrp developers
```

for a shell using the specified group as its effective group.

Check:

```bash
id
```

---

# 40. 🔄 `newgrp`

Example:

```bash
newgrp developers
```

Then:

```bash
id
```

This can start a new shell with:

```text
developers
```

as the effective group.

Exit the new shell with:

```bash
exit
```

---

# 41. 🔍 Troubleshooting Group Access

Suppose:

```text
Permission denied
```

Check:

```bash
ls -l file
```

Then:

```bash
id
```

Then:

```bash
groups
```

Ask:

```text
Who owns the file?
Which group owns it?
Am I a member of that group?
What are the group permissions?
```

---

# 42. 🧠 Troubleshooting Workflow

```text
Permission denied
       ↓
ls -l file
       ↓
Check owner + group
       ↓
id
       ↓
Check group membership
       ↓
Check group permissions
       ↓
Fix group membership/ownership/permissions
       ↓
Verify again
```

---

# 43. 🧪 Mini Project — Shared Java Workspace

Create:

```bash
sudo groupadd java-team
```

Create users:

```bash
sudo useradd -m javauser1
sudo useradd -m javauser2
```

Set passwords:

```bash
sudo passwd javauser1
sudo passwd javauser2
```

Add users:

```bash
sudo usermod -aG java-team javauser1
sudo usermod -aG java-team javauser2
```

Create project directory:

```bash
sudo mkdir -p /opt/java-team
```

Change group:

```bash
sudo chgrp java-team /opt/java-team
```

Set permissions:

```bash
sudo chmod 770 /opt/java-team
```

Verify:

```bash
ls -ld /opt/java-team
```

Check users:

```bash
groups javauser1
groups javauser2
```

---

# 44. 🎯 Practice Challenge

Complete this without looking at the previous sections.

### Task 1

Create:

```text
developers
```

group.

### Task 2

Create:

```text
testers
```

group.

### Task 3

Create:

```text
dev1
dev2
```

users.

### Task 4

Add both users to:

```text
developers
```

### Task 5

Add `dev2` to:

```text
testers
```

### Task 6

Create:

```text
project/
```

### Task 7

Make:

```text
developers
```

the group owner.

### Task 8

Give owner and group full access:

```text
770
```

### Task 9

Verify:

```bash
ls -ld project
```

### Task 10

Verify:

```bash
id dev1
id dev2
```

---

# 45. 🧠 Important Commands

| Command           | Purpose                                    |
| ----------------- | ------------------------------------------ |
| `groups`          | Show current user's groups                 |
| `groups username` | Show a user's groups                       |
| `id`              | UID, GID and group membership              |
| `groupadd`        | Create a group                             |
| `groupdel`        | Delete a group                             |
| `groupmod`        | Modify a group                             |
| `getent group`    | Query group information                    |
| `usermod -aG`     | Add user to supplementary group            |
| `gpasswd -d`      | Remove user from group                     |
| `chgrp`           | Change group ownership                     |
| `newgrp`          | Start shell with specified effective group |
| `chmod`           | Change permissions                         |

---

# 46. ⚡ Cheat Sheet

```bash
# Show groups
groups

# User information
id

# Create group
sudo groupadd developers

# Delete group
sudo groupdel developers

# Rename group
sudo groupmod -n programmers developers

# Check group
getent group developers

# Add user to group
sudo usermod -aG developers username

# Remove user from group
sudo gpasswd -d username developers

# Change group ownership
sudo chgrp developers file.txt

# Recursive group ownership
sudo chgrp -R developers project/

# Change owner + group
sudo chown username:developers file.txt

# Give owner + group full access
sudo chmod 770 project/

# Use group as effective group
newgrp developers
```

---

# 47. 🎤 Interview Questions

### Q1. What is a Linux group?

A group is a collection of users used to simplify permission and access management.

### Q2. Why are groups useful?

They allow permissions to be assigned to multiple users through a single group.

### Q3. What is a primary group?

The primary group is the main group associated with a user's account and is commonly used as the default group for newly created files.

### Q4. What are supplementary groups?

Additional groups that provide a user with extra access.

### Q5. How do you create a group?

```bash
sudo groupadd developers
```

### Q6. How do you add a user to a group?

```bash
sudo usermod -aG developers username
```

### Q7. What does `-aG` mean?

`-G` specifies supplementary groups and `-a` appends the specified group(s) instead of replacing the existing supplementary group list.

### Q8. How do you check group membership?

```bash
groups username
```

or:

```bash
id username
```

### Q9. How do you change a file's group ownership?

```bash
sudo chgrp developers file.txt
```

### Q10. What is `/etc/group`?

It is a local system database file containing group account information.

### Q11. What is GID?

GID stands for Group ID and identifies a Linux group numerically.

### Q12. What is the difference between `chgrp` and `groupmod`?

`chgrp` changes the group ownership of files/directories.

`groupmod` changes properties of the group itself.

---

# 48. 🧠 Day 11 Final Test

Explain this:

```text
-rwxrwx--- 1 srushti developers 4096 project
```

Answer:

```text
1. What is the owner?
2. What is the group?
3. What permissions does the owner have?
4. What permissions does the group have?
5. What permissions do others have?
6. What command changes the group?
7. What command adds a user to developers?
8. What command checks group membership?
9. What does GID mean?
10. What is the difference between primary and supplementary groups?
```

If you can answer all 10, **Day 11 is complete.** 💪

---

# 🚀 DAY 11 COMPLETE

You learned:

```text
✔ Linux Groups
✔ Primary Groups
✔ Supplementary Groups
✔ GID
✔ groupadd
✔ groupdel
✔ groupmod
✔ groups
✔ id
✔ getent
✔ /etc/group
✔ /etc/gshadow
✔ usermod -aG
✔ gpasswd
✔ chgrp
✔ Group Ownership
✔ Group Permissions
✔ Java Team Access
✔ Shared Java Workspace
✔ Group Troubleshooting
```




