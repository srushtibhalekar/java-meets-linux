# 🐧 Day 02 — Files & Directories

> **30 Days of Linux — Day 02**

## 🎯 Mission

**Understand the Linux filesystem. Create it. Navigate it. Organize it.**

---

# 1. Linux Filesystem

Linux organizes everything in a hierarchical structure.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
│   └── user
│       ├── Documents
│       ├── Downloads
│       └── Projects
├── lib
├── opt
├── tmp
├── usr
└── var
```

The top-level directory is:

```text
/
```

This is called the **root directory**.

---

# 2. Everything is Organized as Files

Linux follows the idea that many things can be represented through the filesystem.

Examples include:

* Regular files
* Directories
* Devices
* Configuration files
* System information

This is one of the fundamental ideas behind Unix/Linux systems.

---

# 3. `mkdir` — Create Directories

Create a directory:

```bash
mkdir projects
```

Create multiple directories:

```bash
mkdir java linux git
```

Create nested directories:

```bash
mkdir -p projects/linux/day2
```

The `-p` option creates parent directories when necessary.

Check:

```bash
ls
```

---

# 4. `touch` — Create Files

Create an empty file:

```bash
touch notes.txt
```

Create multiple files:

```bash
touch file1.txt file2.txt file3.txt
```

Create files inside a directory:

```bash
touch projects/notes.txt
```

Check:

```bash
ls projects
```

---

# 5. `cd` — Navigate Directories

Enter a directory:

```bash
cd projects
```

Go back:

```bash
cd ..
```

Go to your home directory:

```bash
cd ~
```

Go to root:

```bash
cd /
```

Return to the previous directory:

```bash
cd -
```

---

# 6. `ls` — View Directory Contents

Basic:

```bash
ls
```

Detailed:

```bash
ls -l
```

Hidden files:

```bash
ls -a
```

Detailed + hidden:

```bash
ls -la
```

Human-readable sizes:

```bash
ls -lh
```

---

# 7. Hidden Files

Linux files beginning with `.` are normally hidden.

Example:

```text
.config
.bashrc
.gitconfig
```

Use:

```bash
ls -a
```

to display them.

A common special entry is:

```text
.
..
```

Meaning:

```text
.   → current directory
..  → parent directory
```

---

# 8. Absolute Paths

An absolute path starts from `/`.

Example:

```bash
/home/user/Documents
```

You can navigate directly:

```bash
cd /home/user/Documents
```

Absolute paths do not depend on your current directory.

---

# 9. Relative Paths

Relative paths start from your current location.

Suppose you are here:

```text
/home/user
```

You can enter Documents with:

```bash
cd Documents
```

You don't need to type:

```bash
cd /home/user/Documents
```

---

# 10. `cp` — Copy Files

Copy a file:

```bash
cp notes.txt backup.txt
```

Copy a file into a directory:

```bash
cp notes.txt projects/
```

Copy a directory:

```bash
cp -r projects projects-backup
```

The `-r` option means **recursive** and is required for copying directories.

---

# 11. `mv` — Move Files

Move a file:

```bash
mv notes.txt projects/
```

Move multiple files:

```bash
mv file1.txt file2.txt projects/
```

---

# 12. `mv` — Rename Files

`mv` can also rename files.

```bash
mv old.txt new.txt
```

Rename a directory:

```bash
mv old-folder new-folder
```

Linux doesn't need a separate `rename` command for basic renaming.

---

# 13. `rm` — Remove Files

Remove a file:

```bash
rm notes.txt
```

Remove multiple files:

```bash
rm file1.txt file2.txt
```

### ⚠️ Be Careful

`rm` normally removes files without sending them to a recycle bin.

Always check the filename before deleting.

---

# 14. Remove Directories

Remove an empty directory:

```bash
rmdir projects
```

Remove a directory and its contents:

```bash
rm -r projects
```

### ⚠️ Dangerous Command

Avoid running commands such as:

```bash
rm -rf /
```

Never experiment with destructive commands on important systems.

---

# 15. `tree` — Visualize Directories

If `tree` is installed:

```bash
tree
```

Example:

```text
.
├── Documents
│   ├── notes.txt
│   └── project.txt
├── Downloads
└── Projects
    └── Linux
        └── Day2
```

This makes directory structures easy to understand.

---

# 16. Working With Paths

You can specify complete paths directly.

Create a file:

```bash
touch Documents/linux.txt
```

Create a directory:

```bash
mkdir Documents/Linux
```

Copy:

```bash
cp Documents/linux.txt Documents/Linux/
```

Move:

```bash
mv Documents/linux.txt Documents/Linux/
```

---

# 17. Spaces in File Names

Suppose a directory is named:

```text
My Projects
```

You can use quotes:

```bash
cd "My Projects"
```

Or escape the space:

```bash
cd My\ Projects
```

Quotes are usually easier for beginners.

---

# 18. Wildcards

Linux supports wildcards for matching filenames.

### `*`

Matches multiple characters.

```bash
ls *.txt
```

This displays `.txt` files.

Example:

```text
notes.txt
project.txt
linux.txt
```

### `?`

Matches one character.

```bash
ls file?.txt
```

Could match:

```text
file1.txt
file2.txt
fileA.txt
```

---

# 19. Directory Navigation Exercise

Start from your home directory:

```bash
cd ~
```

Create:

```bash
mkdir LinuxPractice
```

Enter it:

```bash
cd LinuxPractice
```

Create directories:

```bash
mkdir Day1 Day2 Day3
```

Check:

```bash
ls
```

Create files:

```bash
touch Day1/notes.txt
touch Day2/commands.txt
touch Day3/practice.txt
```

Check:

```bash
ls Day1
ls Day2
ls Day3
```

---

# 20. Copy & Move Exercise

Copy:

```bash
cp Day1/notes.txt Day2/
```

Check:

```bash
ls Day2
```

Move:

```bash
mv Day3/practice.txt Day1/
```

Check:

```bash
ls Day1
ls Day3
```

---

# 21. Rename Exercise

Rename:

```bash
mv Day1/notes.txt Day1/linux-notes.txt
```

Check:

```bash
ls Day1
```

---

# 22. Delete Exercise

Create a temporary file:

```bash
touch temporary.txt
```

Check:

```bash
ls
```

Delete it:

```bash
rm temporary.txt
```

Check again:

```bash
ls
```

---

# 23. Complete Practice Project

Build this structure:

```text
LinuxPractice/
├── Documents/
│   ├── notes.txt
│   └── commands.txt
│
├── Projects/
│   ├── Java/
│   └── Linux/
│       └── Day2/
│
└── Backup/
```

Commands:

```bash
mkdir -p LinuxPractice/Documents
mkdir -p LinuxPractice/Projects/Java
mkdir -p LinuxPractice/Projects/Linux/Day2
mkdir -p LinuxPractice/Backup
```

Create files:

```bash
touch LinuxPractice/Documents/notes.txt
touch LinuxPractice/Documents/commands.txt
```

Verify:

```bash
ls -R LinuxPractice
```

---

# 24. Important Commands

| Command    | Purpose                     |
| ---------- | --------------------------- |
| `pwd`      | Show current directory      |
| `ls`       | List contents               |
| `cd`       | Change directory            |
| `mkdir`    | Create directory            |
| `mkdir -p` | Create nested directories   |
| `touch`    | Create empty file           |
| `cp`       | Copy files/directories      |
| `mv`       | Move or rename              |
| `rm`       | Remove files                |
| `rmdir`    | Remove empty directory      |
| `tree`     | Display directory structure |

---

# 🧠 Key Concepts

### Directory

A container used to organize files and other directories.

### File

A unit of stored data.

### Absolute Path

A complete path beginning from `/`.

Example:

```text
/home/user/Documents/file.txt
```

### Relative Path

A path based on your current location.

Example:

```text
Documents/file.txt
```

### Parent Directory

Represented by:

```text
..
```

### Current Directory

Represented by:

```text
.
```

---

# 🏆 Day 02 Challenge

Without looking at the commands above, try to:

1. Create a directory called `Linux-Day2`
2. Enter it
3. Create three directories
4. Create two files
5. Copy one file
6. Move one file
7. Rename one file
8. Delete one file
9. Display hidden files
10. Display the final directory structure

### Final check

```bash
pwd
ls -la
```

If `tree` is available:

```bash
tree
```

---

# 🎯 Day 02 Complete

**Files organized.
Directories understood.
Linux filesystem unlocked. 🐧📁**

**Day 02 / 30 — COMPLETE**
