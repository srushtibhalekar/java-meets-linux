# 🐧 Day 04 — File Searching

> **30 Days of Linux — Day 04**

## 🎯 Mission

**Don't search manually. Make Linux find it. 🔎**

---

# 1. Why File Searching?

As your Linux system grows, you'll have thousands of files and directories.

Instead of manually checking every folder, Linux provides commands to search efficiently.

Today we will learn:

* `find`
* `locate`
* `which`
* `whereis`
* Search by name
* Search by type
* Search by size
* Search by time
* Search using multiple conditions

---

# 2. `find` — The Main Search Command

Basic syntax:

```bash
find [location] [condition] [action]
```

Example:

```bash
find . -name "notes.txt"
```

Meaning:

```text
.          → Search from current directory
-name      → Search by name
notes.txt  → Filename
```

---

# 3. Search From Current Directory

```bash
find .
```

This displays everything under the current directory.

Example:

```text
.
./Documents
./Documents/notes.txt
./Projects
./Projects/java.txt
```

---

# 4. Search for a Specific File

```bash
find . -name "notes.txt"
```

This searches for files named exactly:

```text
notes.txt
```

---

# 5. Case-Insensitive Search

Use:

```bash
find . -iname "notes.txt"
```

This can match:

```text
notes.txt
Notes.txt
NOTES.TXT
```

Difference:

```text
-name   → case-sensitive
-iname  → case-insensitive
```

---

# 6. Search for `.txt` Files

Use the wildcard:

```bash
find . -name "*.txt"
```

Example output:

```text
./notes.txt
./Documents/info.txt
./Projects/java.txt
```

`*` means multiple characters.

---

# 7. Search for `.java` Files

```bash
find . -name "*.java"
```

Useful when working with Java projects.

---

# 8. Search Only Files

Use:

```bash
find . -type f
```

`f` means:

**regular file**

Search only text files:

```bash
find . -type f -name "*.txt"
```

---

# 9. Search Only Directories

Use:

```bash
find . -type d
```

`d` means:

**directory**

Search for a directory by name:

```bash
find . -type d -name "Documents"
```

---

# 10. Search From a Specific Location

Instead of `.` you can specify a directory.

Example:

```bash
find /home -name "*.txt"
```

Search inside `/tmp`:

```bash
find /tmp -type f
```

Search inside your home directory:

```bash
find ~ -name "*.java"
```

---

# 11. Limit Search Depth

Sometimes you don't want to search deeply into subdirectories.

Use:

```bash
find . -maxdepth 1 -type f
```

Search two levels deep:

```bash
find . -maxdepth 2 -type f
```

Example:

```text
Level 1
./file.txt

Level 2
./Documents/file.txt
```

---

# 12. Search by File Size

Find files larger than 10 MB:

```bash
find . -type f -size +10M
```

Find files smaller than 1 MB:

```bash
find . -type f -size -1M
```

Find files exactly around a specified size:

```bash
find . -type f -size 10M
```

Common units:

| Unit | Meaning   |
| ---- | --------- |
| `c`  | Bytes     |
| `k`  | Kilobytes |
| `M`  | Megabytes |
| `G`  | Gigabytes |

---

# 13. Search by Modification Time

Find files modified within the last day:

```bash
find . -type f -mtime -1
```

Find files modified more than 7 days ago:

```bash
find . -type f -mtime +7
```

### `mtime`

Means:

**Modification time measured in days.**

---

# 14. Search by Minutes

Find files modified within the last 30 minutes:

```bash
find . -type f -mmin -30
```

Find files modified more than 60 minutes ago:

```bash
find . -type f -mmin +60
```

This is useful when troubleshooting recently changed files.

---

# 15. Search Empty Files

Find empty files:

```bash
find . -type f -empty
```

Find empty directories:

```bash
find . -type d -empty
```

---

# 16. Search by Permission

Find files that are executable:

```bash
find . -type f -perm /111
```

Permissions will be studied in detail on **Day 8**.

---

# 17. Search by Owner

You can search for files belonging to a specific user:

```bash
find . -user username
```

Example:

```bash
find /home -user srushti
```

Ownership will be covered later.

---

# 18. Combine Conditions

You can combine search conditions.

Find `.txt` files:

```bash
find . -type f -name "*.txt"
```

Find directories named `backup`:

```bash
find . -type d -name "backup"
```

Find files larger than 10 MB:

```bash
find . -type f -size +10M
```

---

# 19. `find` With `-exec`

`find` can perform an action on the files it finds.

Example:

```bash
find . -name "*.txt" -exec ls -l {} \;
```

Here:

```text
{}  → Current file found by find
\;  → End of the -exec command
```

This is powerful because you can search and then perform an operation.

---

# 20. Find and Delete

⚠️ Be careful with deletion.

Example:

```bash
find . -name "*.tmp" -delete
```

This searches for `.tmp` files and deletes them.

**Always test your search first:**

```bash
find . -name "*.tmp"
```

Only after confirming the results should you consider deletion.

---

# 21. `locate`

`locate` provides a fast way to search for files.

```bash
locate notes.txt
```

It uses a database of file paths, so it can be faster than scanning the filesystem each time.

Because it relies on a database, a newly created file may not immediately appear.

---

# 22. Update the `locate` Database

On systems using `plocate` or `mlocate`, the database can often be updated with:

```bash
sudo updatedb
```

Then:

```bash
locate notes.txt
```

The exact package and availability can vary by Linux distribution.

---

# 23. `which`

`which` helps locate the executable selected for a command.

Example:

```bash
which bash
```

Output might look like:

```text
/usr/bin/bash
```

Try:

```bash
which ls
```

```bash
which java
```

```bash
which git
```

This is useful when checking whether a program is available through your `PATH`.

---

# 24. `whereis`

`whereis` searches for locations related to a command.

Example:

```bash
whereis bash
```

It may show:

* Binary
* Source
* Manual page

Example:

```text
bash: /usr/bin/bash /usr/share/man/man1/bash.1.gz
```

---

# 25. `find` vs `locate`

| Feature             | `find`            | `locate`            |
| ------------------- | ----------------- | ------------------- |
| Search method       | Filesystem search | Database            |
| Usually current     | Yes               | Depends on database |
| Flexible conditions | Very powerful     | Limited             |
| Search by size      | Yes               | No                  |
| Search by time      | Yes               | No                  |
| Search by type      | Yes               | Limited             |
| Speed               | Can be slower     | Usually very fast   |

---

# 26. `find` vs `which`

These commands have different purposes.

### `find`

Searches files/directories:

```bash
find . -name "notes.txt"
```

### `which`

Finds the executable selected for a command:

```bash
which java
```

Don't confuse them.

---

# 27. Practice Environment

Create a practice directory:

```bash
mkdir Day4-Practice
cd Day4-Practice
```

Create directories:

```bash
mkdir Documents Projects Backup
```

Create files:

```bash
touch Documents/notes.txt
touch Documents/linux.txt
touch Projects/java.java
touch Projects/test.txt
touch Backup/backup.txt
```

Check:

```bash
find .
```

---

# 28. Practice Search

Find all text files:

```bash
find . -type f -name "*.txt"
```

Find all Java files:

```bash
find . -type f -name "*.java"
```

Find the `Documents` directory:

```bash
find . -type d -name "Documents"
```

Find empty files:

```bash
find . -type f -empty
```

---

# 29. Search With Multiple Conditions

Find `.txt` files inside `Documents`:

```bash
find Documents -type f -name "*.txt"
```

Find files modified recently:

```bash
find . -type f -mtime -1
```

Find files larger than 1 MB:

```bash
find . -type f -size +1M
```

---

# 30. Real-World Examples

### Find configuration files

```bash
find /etc -name "*.conf"
```

### Find Java source files

```bash
find ~/Projects -name "*.java"
```

### Find recently modified files

```bash
find . -type f -mmin -60
```

### Find large files

```bash
find . -type f -size +100M
```

These techniques are useful for Linux administration, development, debugging, and server maintenance.

---

# 31. Important Safety Rule ⚠️

Never blindly run a destructive command.

For example, don't immediately do:

```bash
find . -name "*.log" -delete
```

First inspect:

```bash
find . -name "*.log"
```

Understand exactly what will be affected.

Then decide whether deletion is appropriate.

---

# 🧠 Command Cheat Sheet

```bash
find .
find . -name "file.txt"
find . -iname "file.txt"
find . -name "*.txt"
find . -type f
find . -type d
find . -maxdepth 2 -type f
find . -type f -size +10M
find . -type f -size -1M
find . -type f -mtime -1
find . -type f -mmin -30
find . -type f -empty
find . -user username
find . -name "*.txt" -exec ls -l {} \;
locate filename
which command
whereis command
```

---

# 🏆 Day 04 Challenge

Complete these tasks without looking at the answers above.

### Challenge 1

Find all `.txt` files from the current directory.

### Challenge 2

Find all directories named `Projects`.

### Challenge 3

Find all files only.

### Challenge 4

Find files modified within the last 24 hours.

### Challenge 5

Find files larger than 10 MB.

### Challenge 6

Find empty files.

### Challenge 7

Find the location of `bash`.

```bash
which bash
```

### Challenge 8

Find information about the `bash` command.

```bash
whereis bash
```

### Challenge 9

Search for a file without caring about uppercase/lowercase.

### Challenge 10

Use `find` with `-exec` to display detailed information about every `.txt` file.

---

# 🧠 Interview Questions

### Q1. What is `find`?

`find` is a Linux command used to search for files and directories based on conditions such as name, type, size, permissions, and modification time.

### Q2. Difference between `find` and `locate`?

`find` searches the filesystem directly and supports powerful conditions. `locate` searches a prebuilt database and is usually faster, but its results depend on how current the database is.

### Q3. What does `-type f` mean?

It tells `find` to search only regular files.

### Q4. What does `-type d` mean?

It tells `find` to search only directories.

### Q5. What does `-iname` do?

It performs a case-insensitive filename search.

### Q6. What does `-mtime -1` mean?

It finds files modified within the last day.

### Q7. What does `which` do?

It shows the executable that the shell would use for a command.

---

# 🎯 Day 04 Complete

**Search smarter.
Find faster.
Control the filesystem. 🔎🐧**

**DAY 04 / 30 — COMPLETE**
