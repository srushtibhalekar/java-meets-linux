# 📦 Day 15 — Linux Archives & Compression

> Learn how to package, compress, extract, and manage files and directories in Linux.

---

## 🎯 What You Will Learn

By the end of Day 15, you will understand:

* What archiving means
* What compression means
* `tar`
* `gzip`
* `gunzip`
* `zip`
* `unzip`
* `.tar`
* `.tar.gz`
* `.tar.bz2`
* `.tar.xz`
* Creating archives
* Extracting archives
* Listing archive contents
* Backing up Java projects
* Practical Linux archive workflows

---

# 1. What Is Archiving?

**Archiving** means combining multiple files and directories into one file.

For example:

```text
project/
├── src/
├── config/
├── README.md
└── pom.xml
```

can be combined into:

```text
project.tar
```

Important:

> Archiving does not necessarily reduce file size.

It mainly combines files into one package.

---

# 2. What Is Compression?

**Compression** reduces the amount of storage required by data.

For example:

```text
large-file
     ↓
compression
     ↓
smaller-file
```

Common compression tools:

```text
gzip
bzip2
xz
```

---

# 3. Archive vs Compression

This is an important interview concept.

### Archive

Combines files.

```text
tar
```

### Compression

Reduces size.

```text
gzip
bzip2
xz
```

They can be used together.

For example:

```text
project/
   ↓
tar
   ↓
project.tar
   ↓
gzip
   ↓
project.tar.gz
```

---

# 4. What Is `tar`?

`tar` originally means:

> Tape Archive

Today, `tar` is commonly used to package files and directories.

Basic syntax:

```bash
tar [options] archive-name files
```

---

# 5. Create a TAR Archive

Suppose you have:

```text
project/
├── src/
├── config/
└── README.md
```

Create an archive:

```bash
tar -cf project.tar project/
```

Meaning:

```text
-c → create
-f → file/archive name
```

Check:

```bash
ls -lh project.tar
```

---

# 6. List TAR Contents

You do not need to extract an archive just to see what is inside.

Use:

```bash
tar -tf project.tar
```

Meaning:

```text
-t → list
-f → archive file
```

Example:

```text
project/
project/src/
project/config/
project/README.md
```

---

# 7. Extract a TAR Archive

Use:

```bash
tar -xf project.tar
```

Meaning:

```text
-x → extract
-f → archive file
```

---

# 8. Extract to a Specific Directory

Create destination:

```bash
mkdir extracted
```

Extract:

```bash
tar -xf project.tar -C extracted
```

`-C` means:

> Change to this directory before performing the operation.

---

# 9. Create a TAR Archive with Multiple Files

Example:

```bash
tar -cf backup.tar file1.txt file2.txt file3.txt
```

You can also include directories:

```bash
tar -cf backup.tar documents/ images/ notes.txt
```

---

# 10. Create a TAR Archive Using Wildcards

Example:

```bash
tar -cf text-files.tar *.txt
```

This archives matching `.txt` files.

---

# 11. What Is GZIP?

`gzip` is a compression tool.

Compress:

```bash
gzip file.txt
```

The original file is normally replaced by:

```text
file.txt.gz
```

Check:

```bash
ls -lh
```

---

# 12. Decompress GZIP

Use:

```bash
gunzip file.txt.gz
```

This produces:

```text
file.txt
```

---

# 13. Important GZIP Concept

`gzip` normally compresses individual files.

For directories, you generally use:

```text
tar + gzip
```

For example:

```text
project/
    ↓
tar
    ↓
project.tar
    ↓
gzip
    ↓
project.tar.gz
```

---

# 14. Create `.tar.gz`

This is one of the most important Linux commands.

Use:

```bash
tar -czf project.tar.gz project/
```

Meaning:

```text
-c → create
-z → gzip compression
-f → archive file
```

---

# 15. Extract `.tar.gz`

Use:

```bash
tar -xzf project.tar.gz
```

Meaning:

```text
-x → extract
-z → gzip
-f → archive
```

---

# 16. List `.tar.gz`

You can inspect it without extracting:

```bash
tar -tzf project.tar.gz
```

This is useful before extracting an archive downloaded from somewhere.

---

# 17. Create `.tar.bz2`

Bzip2 compression:

```bash
tar -cjf project.tar.bz2 project/
```

Here:

```text
-j → bzip2
```

Extract:

```bash
tar -xjf project.tar.bz2
```

List:

```bash
tar -tjf project.tar.bz2
```

---

# 18. Create `.tar.xz`

XZ compression:

```bash
tar -cJf project.tar.xz project/
```

Here:

```text
-J → xz
```

Extract:

```bash
tar -xJf project.tar.xz
```

List:

```bash
tar -tJf project.tar.xz
```

---

# 19. Common Archive Extensions

| Extension  | Meaning                 |
| ---------- | ----------------------- |
| `.tar`     | TAR archive             |
| `.gz`      | GZIP compressed         |
| `.tar.gz`  | TAR + GZIP              |
| `.tgz`     | Usually TAR + GZIP      |
| `.bz2`     | BZIP2 compressed        |
| `.tar.bz2` | TAR + BZIP2             |
| `.xz`      | XZ compressed           |
| `.tar.xz`  | TAR + XZ                |
| `.zip`     | ZIP archive/compression |

---

# 20. ZIP Files

ZIP is another popular archive format.

Create:

```bash
zip backup.zip file1.txt file2.txt
```

Create a ZIP from a directory:

```bash
zip -r project.zip project/
```

`-r` means recursive.

---

# 21. Extract ZIP

Use:

```bash
unzip project.zip
```

Extract to a directory:

```bash
unzip project.zip -d extracted/
```

---

# 22. List ZIP Contents

Before extracting:

```bash
unzip -l project.zip
```

This shows the files inside.

---

# 23. ZIP vs TAR

### ZIP

```text
zip
```

can archive and compress files.

### TAR

```text
tar
```

primarily archives files.

Compression is usually added using:

```text
gzip
bzip2
xz
```

Example:

```text
tar + gzip
```

becomes:

```text
.tar.gz
```

---

# 24. TAR Command Cheat Sheet

### Create

```bash
tar -cf archive.tar directory/
```

### Extract

```bash
tar -xf archive.tar
```

### List

```bash
tar -tf archive.tar
```

### Create GZIP

```bash
tar -czf archive.tar.gz directory/
```

### Extract GZIP

```bash
tar -xzf archive.tar.gz
```

### List GZIP

```bash
tar -tzf archive.tar.gz
```

### Create BZIP2

```bash
tar -cjf archive.tar.bz2 directory/
```

### Extract BZIP2

```bash
tar -xjf archive.tar.bz2
```

### Create XZ

```bash
tar -cJf archive.tar.xz directory/
```

### Extract XZ

```bash
tar -xJf archive.tar.xz
```

---

# 25. Understanding TAR Options

A useful way to remember:

```text
c = create
x = extract
t = list
z = gzip
j = bzip2
J = xz
f = file
```

Examples:

```bash
tar -czf
```

means:

```text
create + gzip + file
```

```bash
tar -xzf
```

means:

```text
extract + gzip + file
```

---

# 26. Practice Lab

Create a Day 15 directory:

```bash
mkdir -p ~/day15-archive-lab
cd ~/day15-archive-lab
```

Create directories:

```bash
mkdir project logs config
```

Create files:

```bash
touch project/app.java
touch project/README.md
touch logs/application.log
touch config/application.properties
```

Check:

```bash
find .
```

---

# 27. Create Your First TAR

Run:

```bash
tar -cf project-backup.tar project/
```

Check:

```bash
ls -lh
```

List archive contents:

```bash
tar -tf project-backup.tar
```

---

# 28. Extract TAR

Create an extraction directory:

```bash
mkdir extracted
```

Extract:

```bash
tar -xf project-backup.tar -C extracted
```

Check:

```bash
find extracted
```

---

# 29. Create TAR.GZ

Run:

```bash
tar -czf project-backup.tar.gz project/
```

Check:

```bash
ls -lh project-backup.tar.gz
```

List contents:

```bash
tar -tzf project-backup.tar.gz
```

Extract:

```bash
mkdir gzip-extracted
tar -xzf project-backup.tar.gz -C gzip-extracted
```

---

# 30. Create ZIP Backup

Run:

```bash
zip -r project-backup.zip project/
```

Check:

```bash
ls -lh project-backup.zip
```

List:

```bash
unzip -l project-backup.zip
```

Extract:

```bash
mkdir zip-extracted
unzip project-backup.zip -d zip-extracted
```

---

# 31. Compare Archive Sizes

Run:

```bash
ls -lh project-backup.tar project-backup.tar.gz project-backup.zip
```

You can compare:

```text
TAR
TAR.GZ
ZIP
```

Remember that compression results depend on the actual data.

---

# 32. Java Developer Example

Suppose you have a Java application:

```text
my-java-app/
├── app.jar
├── config/
├── logs/
└── README.md
```

Create a deployment package:

```bash
tar -czf my-java-app.tar.gz my-java-app/
```

Now you have:

```text
my-java-app.tar.gz
```

This can be transferred to another Linux server and extracted.

---

# 33. Java Log Backup

Suppose logs are stored in:

```text
/opt/myapp/logs/
```

Create an archive:

```bash
tar -czf logs-backup.tar.gz /opt/myapp/logs/
```

For production systems, however, log rotation and retention policies should normally be used instead of manually creating unlimited backups.

---

# 34. Backup Workflow

A basic backup workflow:

```text
Application
     ↓
Collect files
     ↓
tar
     ↓
Compress
     ↓
.tar.gz
     ↓
Store backup
```

Example:

```bash
tar -czf backup.tar.gz project/
```

---

# 35. Excluding Files

Sometimes you don't want certain files in your archive.

Example:

```bash
tar --exclude='*.log' -czf project.tar.gz project/
```

This excludes matching `.log` files.

You might also exclude:

```text
node_modules/
target/
.git/
temporary files
large logs
```

Example:

```bash
tar --exclude='target' -czf project.tar.gz project/
```

---

# 36. View Archive Before Extracting

A good habit is:

```bash
tar -tzf archive.tar.gz
```

or:

```bash
unzip -l archive.zip
```

First inspect.

Then extract.

This helps avoid accidentally extracting unexpected files into the current directory.

---

# 37. Extracting into a Separate Directory

Instead of:

```bash
tar -xzf backup.tar.gz
```

you can use:

```bash
mkdir restore
tar -xzf backup.tar.gz -C restore
```

This keeps your current directory clean.

---

# 38. Common Mistakes

### Mistake 1

Using:

```bash
gzip project/
```

for a directory.

Better:

```bash
tar -czf project.tar.gz project/
```

---

### Mistake 2

Forgetting `-f`.

Incorrect:

```bash
tar -cz project.tar.gz project/
```

Correct:

```bash
tar -czf project.tar.gz project/
```

---

### Mistake 3

Extracting into the wrong directory.

Use:

```bash
mkdir extracted
tar -xzf backup.tar.gz -C extracted
```

---

### Mistake 4

Not checking archive contents.

Use:

```bash
tar -tzf backup.tar.gz
```

---

# 39. Archive Security

Be careful with archives received from unknown sources.

Before extracting:

```bash
tar -tzf archive.tar.gz
```

Look at the paths.

Avoid blindly extracting untrusted archives as `root`.

Archives can contain unexpected paths or files.

---

# 40. Real-World Linux Workflow

A developer may receive:

```text
application-release.tar.gz
```

Workflow:

```bash
ls -lh application-release.tar.gz
```

Inspect:

```bash
tar -tzf application-release.tar.gz
```

Create destination:

```bash
mkdir release
```

Extract:

```bash
tar -xzf application-release.tar.gz -C release
```

Check:

```bash
find release
```

This is a common Linux workflow.

---

# 🧪 Day 15 Mini Project — Java Application Backup

Create:

```bash
mkdir -p ~/day15-archive-lab/java-app/{config,logs,lib}
```

Create sample files:

```bash
touch ~/day15-archive-lab/java-app/app.jar
touch ~/day15-archive-lab/java-app/config/application.properties
touch ~/day15-archive-lab/java-app/logs/application.log
```

Create backup:

```bash
cd ~/day15-archive-lab
tar -czf java-app-backup.tar.gz java-app/
```

Check:

```bash
ls -lh java-app-backup.tar.gz
```

Inspect:

```bash
tar -tzf java-app-backup.tar.gz
```

Create restore directory:

```bash
mkdir restore
```

Restore:

```bash
tar -xzf java-app-backup.tar.gz -C restore
```

Verify:

```bash
find restore
```

You have now created and restored a simulated Java application backup.

---

# 🎯 Day 15 Challenge

Try these without looking at the answers.

### Task 1

Create `backup.tar` from a directory called `project`.

```bash
__________
```

### Task 2

List the contents of `backup.tar`.

```bash
__________
```

### Task 3

Extract `backup.tar`.

```bash
__________
```

### Task 4

Create a gzip-compressed TAR archive.

```bash
__________
```

### Task 5

Extract a `.tar.gz` archive.

```bash
__________
```

### Task 6

Create a ZIP archive from a directory.

```bash
__________
```

### Task 7

List ZIP contents without extracting.

```bash
__________
```

### Task 8

Extract a ZIP file into `restore/`.

```bash
__________
```

---

# 💼 Interview Questions

### 1. What is an archive?

An archive is a single file containing multiple files and/or directories.

### 2. What is compression?

Compression reduces the amount of storage required by data.

### 3. Is TAR compression?

Not by itself. TAR primarily packages files into an archive.

### 4. What is `.tar.gz`?

It is a TAR archive compressed using GZIP.

### 5. What does `tar -czf` mean?

```text
-c → create
-z → gzip
-f → specify archive file
```

### 6. What does `tar -xzf` do?

It extracts a GZIP-compressed TAR archive.

### 7. How do you list the contents of a TAR archive?

```bash
tar -tf archive.tar
```

### 8. How do you list a `.tar.gz` archive?

```bash
tar -tzf archive.tar.gz
```

### 9. What is the difference between ZIP and TAR?

ZIP can archive and compress files as part of one format. TAR primarily archives files and is commonly combined with separate compression tools such as GZIP, BZIP2, or XZ.

### 10. How would you backup a Java application?

A simple example is:

```bash
tar -czf java-app-backup.tar.gz java-app/
```


