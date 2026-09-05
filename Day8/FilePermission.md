# 🐧 Day 08 — File Permissions

> **30 Days of Linux — Day 08**

## 🎯 Mission

Today you will learn how Linux controls access to files and directories.

By the end of Day 08, you will understand:

* Linux file permissions
* Users, groups, and others
* Read `r`
* Write `w`
* Execute `x`
* `ls -l`
* Symbolic permissions
* Numeric permissions
* `chmod`
* File vs directory permissions
* `umask`
* `stat`
* `find` with permissions
* Permission troubleshooting
* Security best practices

---

# 1. What Are File Permissions?

Linux is a multi-user operating system.

Different users may need different levels of access to files.

For example:

```text
Developer → Read + Write
Manager   → Read
Others    → No Access
```

Linux uses **permissions** to control this access.

The three basic permissions are:

```text
r → read
w → write
x → execute
```

---

# 2. Users, Groups, and Others

Linux permissions are divided into three categories:

```text
User
Group
Others
```

### User

The owner of the file.

### Group

Users belonging to the file's group.

### Others

Everyone else.

Think:

```text
             FILE
              │
      ┌───────┼───────┐
      ↓       ↓       ↓
    User    Group   Others
     u        g       o
```

---

# 3. Check Permissions

Use:

```bash
ls -l
```

Example:

```text
-rwxr-xr-- 1 srushti developers 1200 Sep 5 app.sh
```

The first part:

```text
-rwxr-xr--
```

contains the permissions.

---

# 4. Permission String Anatomy

Consider:

```text
-rwxr-xr--
```

Break it into:

```text
- rwx r-x r--
│ │   │   │
│ │   │   └── Others
│ │   └────── Group
│ └────────── User
└──────────── File type
```

So:

```text
- rwx r-x r--
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── User
```

---

# 5. First Character — File Type

The first character tells you the type.

Common examples:

```text
- → regular file
d → directory
l → symbolic link
```

Example:

```text
-rw-r--r--
```

Regular file.

Example:

```text
drwxr-xr-x
```

Directory.

---

# 6. Read Permission — `r`

Read permission allows you to view the contents of a file.

For a regular file:

```text
r → read contents
```

Example:

```bash
cat file.txt
```

requires read access to the file.

---

# 7. Write Permission — `w`

Write permission allows modification of a file.

For example:

```bash
echo "Hello" > file.txt
```

requires appropriate write access.

Write permission can allow:

```text
Modify
Overwrite
Append
Change contents
```

---

# 8. Execute Permission — `x`

Execute permission allows a file to be executed as a program or script.

For example:

```bash
./script.sh
```

A script normally needs execute permission.

You can give it using:

```bash
chmod +x script.sh
```

Then:

```bash
./script.sh
```

---

# 9. Important: Directory Permissions

Permissions behave differently for directories.

For a directory:

```text
r → list directory entries
w → create/delete/rename entries
x → enter/traverse directory
```

This is extremely important.

---

# 10. Directory `r`

If you have read permission on a directory, you can generally list its contents:

```bash
ls directory
```

But read permission alone does not necessarily let you access the files inside.

---

# 11. Directory `w`

Write permission on a directory allows changes to its entries.

For example:

```text
Create file
Delete file
Rename file
```

However, directory write permission usually works together with execute permission for normal file management.

---

# 12. Directory `x`

Execute permission on a directory means you can traverse/access it.

For example:

```bash
cd project
```

requires execute/traverse permission on the directory.

Easy memory:

```text
File:
r → read
w → modify
x → execute

Directory:
r → list
w → change entries
x → enter/traverse
```

---

# 13. Permission Positions

Every permission set contains three groups:

```text
rwx rwx rwx
│   │   │
│   │   └── Others
│   └────── Group
└────────── User
```

Example:

```text
rwxr-xr--
```

means:

```text
User   → rwx
Group  → r-x
Others → r--
```

---

# 14. Meaning of `-`

A dash means the permission is not granted.

Example:

```text
r-x
```

means:

```text
r → yes
w → no
x → yes
```

Example:

```text
---
```

means:

```text
read    → no
write   → no
execute → no
```

---

# 15. Numeric Permissions

Linux also represents permissions using numbers.

The values are:

| Permission | Value |
| ---------- | ----: |
| `r`        |     4 |
| `w`        |     2 |
| `x`        |     1 |
| `-`        |     0 |

Remember:

```text
r = 4
w = 2
x = 1
```

---

# 16. Calculate Permission Numbers

For:

```text
rwx
```

calculate:

```text
r = 4
w = 2
x = 1

4 + 2 + 1 = 7
```

Therefore:

```text
rwx = 7
```

---

# 17. More Examples

### `r--`

```text
r = 4
w = 0
x = 0

4
```

Therefore:

```text
r-- = 4
```

### `rw-`

```text
4 + 2 = 6
```

Therefore:

```text
rw- = 6
```

### `r-x`

```text
4 + 1 = 5
```

Therefore:

```text
r-x = 5
```

### `--x`

```text
1
```

Therefore:

```text
--x = 1
```

---

# 18. Numeric Permission Table

| Number | Permission |
| -----: | ---------- |
|      0 | `---`      |
|      1 | `--x`      |
|      2 | `-w-`      |
|      3 | `-wx`      |
|      4 | `r--`      |
|      5 | `r-x`      |
|      6 | `rw-`      |
|      7 | `rwx`      |

This table is extremely important for Linux interviews.

---

# 19. Understanding `755`

Consider:

```text
755
```

Break it:

```text
7 → User
5 → Group
5 → Others
```

Convert:

```text
7 = rwx
5 = r-x
5 = r-x
```

Therefore:

```text
755 = rwxr-xr-x
```

---

# 20. Understanding `644`

Consider:

```text
644
```

Convert:

```text
6 = rw-
4 = r--
4 = r--
```

Therefore:

```text
644 = rw-r--r--
```

This is commonly used for regular files.

---

# 21. Common Permissions

### `600`

```text
rw-------
```

Owner:

```text
read + write
```

Group:

```text
no access
```

Others:

```text
no access
```

Useful for private files.

---

### `700`

```text
rwx------
```

Owner has full access.

Group and others have no access.

---

### `644`

```text
rw-r--r--
```

Owner:

```text
read + write
```

Group:

```text
read
```

Others:

```text
read
```

---

### `755`

```text
rwxr-xr-x
```

Owner:

```text
read + write + execute
```

Group:

```text
read + execute
```

Others:

```text
read + execute
```

Common for executable scripts and directories.

---

# 22. `chmod`

`chmod` means:

> Change mode

It is used to modify file permissions.

Basic syntax:

```bash
chmod permissions filename
```

Example:

```bash
chmod 644 notes.txt
```

---

# 23. Numeric `chmod`

Example:

```bash
chmod 755 script.sh
```

This gives:

```text
User   → rwx
Group  → r-x
Others → r-x
```

Check:

```bash
ls -l script.sh
```

---

# 24. Another Example

```bash
chmod 600 secret.txt
```

Result:

```text
-rw-------
```

Only the owner can read and write the file.

---

# 25. `chmod 700`

Run:

```bash
chmod 700 private.txt
```

Result:

```text
-rwx------
```

For a normal text file, execute permission may not be necessary.

For a private directory:

```bash
chmod 700 private/
```

can be appropriate.

---

# 26. Symbolic `chmod`

Instead of numbers, you can use:

```text
u → user
g → group
o → others
a → all
```

Permissions:

```text
r → read
w → write
x → execute
```

Operators:

```text
+ → add
- → remove
= → set exactly
```

---

# 27. Add Execute Permission

Run:

```bash
chmod +x script.sh
```

This adds execute permission.

Equivalent concept:

```text
User + Group + Others
```

depending on the system's current mode and `umask`/`chmod` behavior.

More explicitly:

```bash
chmod u+x script.sh
```

adds execute permission for the owner.

---

# 28. Add Write Permission

```bash
chmod u+w file.txt
```

Adds write permission for the owner.

---

# 29. Remove Write Permission

```bash
chmod u-w file.txt
```

Removes write permission from the owner.

---

# 30. Group Permissions

Add group write permission:

```bash
chmod g+w project.txt
```

Remove group write permission:

```bash
chmod g-w project.txt
```

---

# 31. Others Permissions

Add read permission:

```bash
chmod o+r file.txt
```

Remove read permission:

```bash
chmod o-r file.txt
```

---

# 32. Set Exact Permissions

You can use `=`.

Example:

```bash
chmod u=rwx file.sh
```

Owner gets exactly:

```text
rwx
```

Another:

```bash
chmod g=rx file.sh
```

Group gets exactly:

```text
r-x
```

---

# 33. Apply Permission to Everyone

Example:

```bash
chmod a+r file.txt
```

Adds read permission for:

```text
User
Group
Others
```

---

# 34. Remove Permission from Everyone

```bash
chmod a-w file.txt
```

Removes write permission from all three categories.

Be careful with broad permission changes.

---

# 35. Check Permissions with `stat`

Use:

```bash
stat file.txt
```

You may see information such as:

```text
Access: (0644/-rw-r--r--)
```

This gives both:

```text
Numeric permissions
Symbolic permissions
```

---

# 36. `ls -ld`

When checking a directory:

```bash
ls -ld project
```

This shows permissions of the directory itself.

Compare:

```bash
ls -l project
```

which normally shows the contents inside the directory.

Remember:

```text
ls -ld directory
→ directory itself
```

---

# 37. Recursive `chmod`

You can modify permissions recursively:

```bash
chmod -R 755 project/
```

`-R` means:

```text
Recursive
```

It applies changes to the directory and everything under it.

### ⚠️ Warning

Do not blindly use:

```bash
chmod -R 777 /
```

or similar commands.

Incorrect recursive permissions can create serious security problems.

---

# 38. Why `777` Is Dangerous

`777` means:

```text
rwxrwxrwx
```

Everyone can:

```text
read
write
execute
```

This is usually too permissive.

Avoid using `777` as a random fix for:

```text
Permission denied
```

Instead, understand:

```text
Who owns the file?
Which group owns it?
What permission is actually required?
```

---

# 39. Permission Denied Troubleshooting

Suppose:

```bash
./script.sh
```

returns:

```text
Permission denied
```

First check:

```bash
ls -l script.sh
```

If you see:

```text
-rw-r--r--
```

there is no execute permission.

You can add it:

```bash
chmod u+x script.sh
```

Then:

```bash
./script.sh
```

---

# 40. Another Permission Problem

If:

```bash
cat secret.txt
```

returns:

```text
Permission denied
```

check:

```bash
ls -l secret.txt
```

You may not have read permission.

Do not immediately use `sudo`.

First understand:

```text
Owner?
Group?
Permissions?
```

---

# 41. `sudo`

`sudo` allows an authorized user to execute commands with elevated privileges.

Example:

```bash
sudo some-command
```

You may need it for administrative tasks.

However:

> Do not use `sudo` simply to hide permission problems.

First understand the permission issue.

---

# 42. `umask`

`umask` controls which permission bits are cleared when new files and directories are created.

Check your current value:

```bash
umask
```

Example:

```text
0022
```

The exact default depends on the environment and configuration.

---

# 43. Why `umask` Matters

Suppose a program requests default permissions for a newly created file.

The system applies the process's `umask` to determine which permission bits are removed.

Conceptually:

```text
Requested permissions
        ↓
      umask
        ↓
Final permissions
```

Typical examples often result in:

```text
Files       → 644
Directories → 755
```

when a common `0022` umask is used.

But actual results depend on the application and system configuration.

---

# 44. Check Current `umask`

Run:

```bash
umask
```

For a more symbolic representation:

```bash
umask -S
```

Example:

```text
u=rwx,g=rx,o=rx
```

The exact output depends on your system.

---

# 45. File vs Directory Permissions

This is a common interview topic.

### File

```text
r → read contents
w → modify contents
x → execute
```

### Directory

```text
r → list entries
w → create/delete/rename entries
x → traverse/enter
```

Remember:

```text
FILE      → contents
DIRECTORY → entries/navigation
```

---

# 46. `find` with Permissions

You can search for files based on permissions.

Example:

```bash
find . -type f -perm 644
```

This searches for files whose permissions match the specified mode.

Another example:

```bash
find . -type f -perm -111
```

This can find files with execute bits set.

Permission matching can be subtle, so always test it in a practice directory before using it on important files.

---

# 47. Practice Environment

Create a practice directory:

```bash
mkdir Day8-Practice
cd Day8-Practice
```

Create files:

```bash
touch public.txt private.txt script.sh
```

Create directories:

```bash
mkdir public private
```

Check:

```bash
ls -la
```

---

# 48. Practice 1 — Check Permissions

Run:

```bash
ls -l
```

Identify:

```text
User permissions
Group permissions
Others permissions
```

for every file.

---

# 49. Practice 2 — Set `644`

Run:

```bash
chmod 644 public.txt
```

Check:

```bash
ls -l public.txt
```

Expected permission:

```text
-rw-r--r--
```

---

# 50. Practice 3 — Set `600`

Run:

```bash
chmod 600 private.txt
```

Check:

```bash
ls -l private.txt
```

Expected:

```text
-rw-------
```

---

# 51. Practice 4 — Make Script Executable

Run:

```bash
chmod +x script.sh
```

Check:

```bash
ls -l script.sh
```

You should see execute permission.

Then:

```bash
./script.sh
```

If the script contains commands, they can execute according to its interpreter and contents.

---

# 52. Practice 5 — Symbolic Permissions

Run:

```bash
chmod u+x script.sh
chmod g+r script.sh
chmod o-r script.sh
```

Then:

```bash
ls -l script.sh
```

Observe the changes.

---

# 53. Practice 6 — Directory Permissions

Check:

```bash
ls -ld public
```

Change:

```bash
chmod 755 public
```

Check again:

```bash
ls -ld public
```

---

# 54. Practice 7 — Private Directory

Run:

```bash
chmod 700 private
```

Check:

```bash
ls -ld private
```

Expected:

```text
drwx------
```

The owner has full access.

---

# 55. Mini Project — Linux Permission Manager

Create:

```bash
mkdir Day8-Permission-Project
cd Day8-Permission-Project
```

Create:

```bash
mkdir public private scripts
touch public/readme.txt
touch private/secret.txt
touch scripts/backup.sh
```

---

## Step 1 — Public File

Run:

```bash
chmod 644 public/readme.txt
```

Check:

```bash
ls -l public/readme.txt
```

Expected:

```text
-rw-r--r--
```

---

## Step 2 — Private File

Run:

```bash
chmod 600 private/secret.txt
```

Check:

```bash
ls -l private/secret.txt
```

Expected:

```text
-rw-------
```

---

## Step 3 — Script

Run:

```bash
chmod 755 scripts/backup.sh
```

Check:

```bash
ls -l scripts/backup.sh
```

Expected:

```text
-rwxr-xr-x
```

---

## Step 4 — Private Directory

Run:

```bash
chmod 700 private
```

Check:

```bash
ls -ld private
```

---

## Step 5 — Inspect Everything

Run:

```bash
find . -maxdepth 2 -ls
```

Study:

```text
Permissions
Owner
Group
File names
```

---

# 56. 🔥 Real-World Java Example

Suppose you have:

```text
deploy.sh
```

used to start a Java application.

Initially:

```text
-rw-r--r--
```

Trying:

```bash
./deploy.sh
```

may produce:

```text
Permission denied
```

Fix:

```bash
chmod +x deploy.sh
```

Then:

```bash
./deploy.sh
```

This is a common Linux task for Java developers and DevOps workflows.

---

# 57. 🔥 Real-World Server Example

Imagine:

```text
application.properties
```

contains sensitive configuration.

You may want only the owner to read and modify it:

```bash
chmod 600 application.properties
```

This reduces unnecessary access.

Always consider the actual application, service account, group ownership, and deployment setup before choosing permissions.

---

# 58. Security Best Practices

Follow the principle:

> Give only the permissions that are actually required.

Avoid:

```bash
chmod 777 file
```

when a narrower permission is sufficient.

Prefer:

```text
600 → private files
644 → ordinary readable files
700 → private directories/scripts
755 → executable files/directories when appropriate
```

These are common patterns, not universal rules.

---

# 59. Common Mistakes

### Mistake 1

Using:

```bash
chmod 777
```

for everything.

❌ Too permissive.

---

### Mistake 2

Forgetting execute permission:

```bash
./script.sh
```

Fix:

```bash
chmod +x script.sh
```

---

### Mistake 3

Changing permissions without checking current state.

Always inspect:

```bash
ls -l
```

---

### Mistake 4

Using recursive `chmod` carelessly.

Be careful with:

```bash
chmod -R
```

---

### Mistake 5

Using `sudo` without understanding why.

First investigate:

```text
owner
group
permissions
directory traversal
```

---

# 60. 🧠 Quick Memory Trick

Remember:

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
```

And:

```text
755 = rwxr-xr-x
644 = rw-r--r--
600 = rw-------
700 = rwx------
```

---

# 61. Permission Formula

Think:

```text
USER | GROUP | OTHERS
----------------------
  7  |   5   |   5
```

For:

```text
755
```

convert each number separately:

```text
7 → rwx
5 → r-x
5 → r-x
```

Result:

```text
rwxr-xr-x
```

---

# 62. Command Cheat Sheet

```bash
# Show permissions
ls -l

# Show directory itself
ls -ld directory

# Detailed metadata
stat file.txt

# Numeric permissions
chmod 644 file.txt
chmod 755 script.sh
chmod 600 secret.txt
chmod 700 private/

# Add execute
chmod +x script.sh

# Owner execute
chmod u+x script.sh

# Group write
chmod g+w file.txt

# Others read
chmod o+r file.txt

# Remove owner write
chmod u-w file.txt

# Set exact owner permissions
chmod u=rwx file.sh

# Recursive
chmod -R 755 directory/

# Check umask
umask

# Symbolic umask
umask -S

# Find files by permission
find . -type f -perm 644
```

---

# 63. 🎯 Day 08 Challenge

Try these without looking at the answers.

### Challenge 1

What does this mean?

```text
-rwxr-xr--
```

Identify:

```text
User
Group
Others
```

---

### Challenge 2

Convert:

```text
755
```

to symbolic permissions.

---

### Challenge 3

Convert:

```text
rw-r--r--
```

to numeric permissions.

---

### Challenge 4

Give the owner read/write permission and everyone else read-only permission.

Hint:

```bash
chmod ...
```

---

### Challenge 5

Make:

```text
backup.sh
```

executable by the owner.

---

### Challenge 6

Make:

```text
secret.txt
```

accessible only to its owner for reading and writing.

---

### Challenge 7

Create a directory where only the owner has full access.

---

### Challenge 8

Check the permissions of a directory itself rather than its contents.

---

### Challenge 9

Check your current `umask`.

---

### Challenge 10

Find regular files with permission mode `644`.

---

# 64. 🧠 Interview Questions

### Q1. What are Linux file permissions?

Linux file permissions control who can read, modify, or execute files and directories.

---

### Q2. What are the three permission categories?

```text
User
Group
Others
```

---

### Q3. What does `r` mean?

Read permission.

---

### Q4. What does `w` mean?

Write permission.

---

### Q5. What does `x` mean?

Execute permission for a file, and traversal/search permission for a directory.

---

### Q6. What does `chmod` do?

`chmod` changes file or directory permissions.

---

### Q7. What does `755` mean?

```text
User   → rwx
Group  → r-x
Others → r-x
```

---

### Q8. What does `644` mean?

```text
User   → rw-
Group  → r--
Others → r--
```

---

### Q9. What does `600` mean?

```text
User   → rw-
Group  → ---
Others → ---
```

---

### Q10. What is the difference between `644` and `755`?

`644` normally gives read/write to the owner and read-only access to group/others.

`755` additionally gives execute permission to all three categories.

---

### Q11. What is `umask`?

`umask` specifies permission bits that are cleared from permissions requested when new files/directories are created.

---

### Q12. How do you make a script executable?

```bash
chmod +x script.sh
```

---

### Q13. How do you check permissions?

```bash
ls -l
```

or:

```bash
stat file
```

---

### Q14. What does `-R` mean in `chmod`?

It means recursive.

Example:

```bash
chmod -R 755 project/
```

changes permissions throughout the directory tree.

---

### Q15. Why should you avoid `chmod 777`?

Because it grants read, write, and execute permissions to everyone and can create unnecessary security risks.

---

# 65. 💼 Real-World Uses

File permissions are important in:

* Linux administration
* Java deployment
* Embedded Linux
* DevOps
* Server security
* Bash scripting
* Web servers
* CI/CD
* Application configuration
* Log management
* SSH environments

For a Java developer working on Linux, understanding:

```text
chmod
ls -l
ownership
groups
permissions
```

is extremely useful.

---

# 66. 🧩 Day 08 Summary

Today you learned:

```text
Users
Groups
Others
r
w
x
chmod
755
644
600
700
umask
stat
ls -l
```

Most important numbers:

```text
r = 4
w = 2
x = 1
```

Most important permission modes:

```text
755 → rwxr-xr-x
644 → rw-r--r--
600 → rw-------
700 → rwx------
```

Most important concept:

```text
FILE:
r → read contents
w → modify contents
x → execute

DIRECTORY:
r → list
w → modify entries
x → traverse
```

---

