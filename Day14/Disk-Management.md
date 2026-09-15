# 💾 Day 14 — Linux Disk Management

> Learn how Linux manages disks, partitions, filesystems, mount points, and storage usage.

---

## 🎯 What You Will Learn

By the end of Day 14, you will understand:

* Disk vs partition vs filesystem
* Mount points
* `df`
* `du`
* `lsblk`
* `blkid`
* `mount`
* `umount`
* `/etc/fstab`
* Filesystem types
* Inode usage
* Finding what is consuming disk space
* Disk-full troubleshooting
* Disk monitoring for Java applications

---

# 1. What Is Disk Management?

Disk management means:

> Monitoring, inspecting, organizing, and managing storage devices in Linux.

For example, a Linux server may have:

```text
Disk
 │
 ├── Partition 1
 │     └── Filesystem
 │
 └── Partition 2
       └── Filesystem
```

Applications store:

* Java applications
* Logs
* Database files
* Configuration files
* User files
* Temporary files

If the disk becomes full, applications can fail.

---

# 2. Disk vs Partition vs Filesystem

These terms are different.

### Disk

A physical or virtual storage device.

Example:

```text
/dev/sda
```

### Partition

A section of a disk.

Example:

```text
/dev/sda1
/dev/sda2
```

### Filesystem

The structure used to store files.

Common filesystems:

```text
ext4
xfs
btrfs
vfat
ntfs
```

### Mount Point

A directory where a filesystem becomes accessible.

Example:

```text
/mnt/data
```

Think of it like:

```text
/dev/sda1
    ↓
Filesystem
    ↓
/mnt/data
```

---

# 3. Check Disk Space with `df`

`df` means:

> Disk Filesystem

Basic command:

```bash
df
```

Human-readable:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   25G   23G  53% /
```

Important columns:

| Column     | Meaning                   |
| ---------- | ------------------------- |
| Filesystem | Storage device/filesystem |
| Size       | Total space               |
| Used       | Used space                |
| Avail      | Available space           |
| Use%       | Percentage used           |
| Mounted on | Mount point               |

---

# 4. `df -h`

Always prefer:

```bash
df -h
```

`-h` means human-readable.

Instead of:

```text
52428800
```

you may see:

```text
50G
```

---

# 5. Check Filesystem Type

Use:

```bash
df -T
```

Human-readable:

```bash
df -Th
```

Example:

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda2      ext4   50G   25G   23G  53% /
```

Here:

```text
Type = ext4
```

---

# 6. Check Inode Usage

Linux filesystems also have **inodes**.

An inode stores metadata about a file.

For example:

```text
file name
owner
permissions
timestamps
file size
location information
```

Check inode usage:

```bash
df -i
```

Human-readable:

```bash
df -ih
```

You can have free disk space but still run out of inodes if there are extremely large numbers of small files.

---

# 7. `du` — Disk Usage

`du` means:

> Disk Usage

Basic:

```bash
du
```

Human-readable:

```bash
du -h
```

Check current directory:

```bash
du -sh .
```

Example:

```text
250M    .
```

---

# 8. Find Size of a Specific Directory

```bash
du -sh /var/log
```

Example:

```text
2.3G    /var/log
```

This tells you how much storage `/var/log` is using.

---

# 9. Check Subdirectories

```bash
du -h --max-depth=1
```

Example:

```text
100M    ./logs
500M    ./downloads
2.0G    ./project
2.6G    .
```

This is very useful when searching for large directories.

> `--max-depth` is common on GNU/Linux. Some systems may use different options.

---

# 10. Find the Largest Directories

Run:

```bash
du -h --max-depth=1 2>/dev/null | sort -h
```

Largest entries can then be inspected.

For largest first:

```bash
du -h --max-depth=1 2>/dev/null | sort -hr
```

---

# 11. `lsblk`

`lsblk` means:

> List Block Devices

Run:

```bash
lsblk
```

More useful:

```bash
lsblk -f
```

Example:

```text
NAME   FSTYPE FSVER LABEL UUID                                 MOUNTPOINTS
sda
├─sda1 vfat         EFI   XXXX                                 /boot/efi
└─sda2 ext4               XXXX                                 /
```

This helps you understand:

* disks
* partitions
* filesystems
* mount points

---

# 12. `lsblk -o`

You can choose columns.

Example:

```bash
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINTS
```

Useful when investigating a server.

---

# 13. `blkid`

`blkid` displays block-device information.

Run:

```bash
sudo blkid
```

Example:

```text
/dev/sda2: UUID="xxxx" TYPE="ext4"
```

Important information:

```text
UUID
TYPE
LABEL
```

---

# 14. What Is UUID?

UUID means:

> Universally Unique Identifier

A filesystem can have a UUID such as:

```text
UUID=1234-abcd-5678
```

Linux can use this UUID when mounting filesystems.

UUIDs are useful because device names can sometimes change.

---

# 15. Mounting a Filesystem

`mount` connects a filesystem to a directory.

General structure:

```bash
sudo mount DEVICE DIRECTORY
```

Example:

```bash
sudo mount /dev/sdb1 /mnt/data
```

After mounting:

```text
/dev/sdb1
     ↓
/mnt/data
```

Files on `/dev/sdb1` become accessible through `/mnt/data`.

---

# ⚠️ Important Mount Warning

Do **not** randomly mount or modify disks on your computer.

Before using commands involving:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
```

identify the device first:

```bash
lsblk -f
```

Incorrect disk operations can cause data loss.

For learning, use a VM, WSL environment, or a dedicated test disk.

---

# 16. View Mounted Filesystems

Run:

```bash
mount
```

You can also use:

```bash
findmnt
```

`findmnt` gives a cleaner view.

Example:

```bash
findmnt /
```

---

# 17. Unmount a Filesystem

Use:

```bash
sudo umount /mnt/data
```

Important:

```text
umount
```

has no `n`.

It is **not**:

```bash
unmount
```

---

# 18. Why Can `umount` Fail?

You may see:

```text
target is busy
```

This usually means a process is currently using that filesystem.

Check which processes are using it:

```bash
sudo lsof +D /mnt/data
```

or:

```bash
sudo fuser -vm /mnt/data
```

Then investigate the processes before unmounting.

---

# 19. `/etc/fstab`

Linux can automatically mount filesystems during boot.

The configuration file is:

```text
/etc/fstab
```

View it:

```bash
cat /etc/fstab
```

Example structure:

```text
UUID=xxxx  /data  ext4  defaults  0  2
```

Meaning:

```text
UUID
 ↓
Device/filesystem
 ↓
Mount point
 ↓
Filesystem type
 ↓
Mount options
```

---

# ⚠️ `/etc/fstab` Warning

Be careful when editing:

```text
/etc/fstab
```

A wrong entry can cause boot or mounting problems.

Always make sure you understand an entry before changing it.

---

# 20. Common Linux Filesystems

## ext4

Very common Linux filesystem.

Used for:

```text
servers
Linux desktops
VMs
development systems
```

---

## XFS

Common in enterprise/server environments.

Useful for:

```text
large files
large storage systems
servers
```

---

## Btrfs

Modern filesystem with features such as:

* snapshots
* subvolumes
* checksumming

---

## FAT / VFAT

Common for:

```text
USB drives
EFI partitions
memory cards
```

---

## NTFS

Commonly associated with Windows storage.

Linux can work with NTFS depending on the installed filesystem support.

---

# 21. Important Linux Storage Directories

### `/`

Root filesystem.

### `/home`

User files.

Example:

```text
/home/srushti
```

### `/var`

Frequently changing data.

Contains things such as:

```text
logs
cache
application data
```

### `/var/log`

System/application logs.

### `/tmp`

Temporary files.

### `/opt`

Often used for optional/additional software.

---

# 22. Java Developer Example

Suppose your Java application stores logs here:

```text
/opt/myapp/logs/
```

After several months:

```text
application.log
application.log.1
application.log.2
application.log.3
...
```

The directory may become huge.

Check:

```bash
du -sh /opt/myapp/logs
```

Then:

```bash
du -h --max-depth=1 /opt/myapp/logs | sort -hr
```

This helps identify what is consuming space.

---

# 23. Disk Full Problem

Suppose an application reports:

```text
No space left on device
```

First check:

```bash
df -h
```

Then:

```bash
df -i
```

Then investigate large directories:

```bash
du -h --max-depth=1 /var | sort -hr
```

Check logs:

```bash
du -sh /var/log
```

Check application data:

```bash
du -sh /opt/*
```

---

# 24. Disk Space vs Inodes

There are two different problems.

### Problem 1 — Disk space full

Check:

```bash
df -h
```

### Problem 2 — Inodes full

Check:

```bash
df -i
```

For example:

```text
Disk usage:     40%
Inode usage:    100%
```

You may still be unable to create files.

---

# 25. Deleted File Still Using Disk Space

Sometimes a file is deleted but a running process still has it open.

Check:

```bash
sudo lsof +L1
```

You may see something like:

```text
java   1234   user   ...   /var/log/app.log (deleted)
```

The process still holds the file open.

The space may only be released when the process closes the file.

This is an important production troubleshooting technique.

---

# 26. Check a Specific Filesystem

For root:

```bash
df -h /
```

For `/home`:

```bash
df -h /home
```

For `/var`:

```bash
df -h /var
```

---

# 27. Disk Management Workflow

When someone says:

> "The server disk is almost full."

Use this workflow:

```text
1. Check filesystem usage
        ↓
df -h

2. Check inode usage
        ↓
df -i

3. Find large directories
        ↓
du -h --max-depth=1

4. Inspect logs
        ↓
/var/log

5. Inspect application data
        ↓
/opt /home /var

6. Check deleted-open files
        ↓
lsof +L1

7. Investigate safely
```

Do not immediately delete files.

First understand **what** is consuming the storage and **why**.

---

# 28. `free` Is NOT Disk Usage

A common beginner mistake is:

```bash
free -h
```

This checks **RAM/memory**, not disk storage.

### RAM

```bash
free -h
```

### Disk

```bash
df -h
```

Remember:

```text
free → memory
df   → filesystem/disk space
du   → directory/file usage
```

---

# 29. Safe Practice Lab

Create a practice directory:

```bash
mkdir -p ~/day14-disk-lab
cd ~/day14-disk-lab
```

Create some files:

```bash
touch file1.txt file2.txt file3.txt
```

Create directories:

```bash
mkdir logs data backups
```

Create files inside them:

```bash
touch logs/app.log
touch data/database.txt
touch backups/backup.txt
```

Check:

```bash
ls -R
```

Check directory size:

```bash
du -sh .
```

Check subdirectory sizes:

```bash
du -h --max-depth=1
```

Sort them:

```bash
du -h --max-depth=1 | sort -hr
```

---

# 30. Create a Larger Test File

You can safely create a test file inside your home directory:

```bash
dd if=/dev/zero of=~/day14-disk-lab/testfile.bin bs=1M count=10
```

This creates approximately a 10 MB file.

Check:

```bash
ls -lh ~/day14-disk-lab/testfile.bin
```

Check usage:

```bash
du -sh ~/day14-disk-lab
```

Remove the test file when finished:

```bash
rm ~/day14-disk-lab/testfile.bin
```

---

# 31. Disk Usage Report Mini Project

Create:

```bash
mkdir -p ~/day14-disk-lab/report
cd ~/day14-disk-lab/report
```

Run:

```bash
echo "===== DISK USAGE REPORT ====="
date
echo
echo "Filesystem Usage:"
df -h
echo
echo "Inode Usage:"
df -ih
echo
echo "Current Directory Usage:"
du -sh .
```

Save the report:

```bash
{
echo "===== DISK USAGE REPORT ====="
date
echo
echo "Filesystem Usage:"
df -h
echo
echo "Inode Usage:"
df -ih
echo
echo "Directory Usage:"
du -sh .
} > disk-report.txt
```

View it:

```bash
cat disk-report.txt
```

---

# 32. Useful Commands Cheat Sheet

| Command          | Purpose                            |
| ---------------- | ---------------------------------- |
| `df`             | Show filesystem space              |
| `df -h`          | Human-readable disk space          |
| `df -T`          | Show filesystem type               |
| `df -i`          | Show inode usage                   |
| `du`             | Show disk usage                    |
| `du -sh`         | Total directory size               |
| `du -h`          | Human-readable usage               |
| `lsblk`          | List block devices                 |
| `lsblk -f`       | Show filesystem information        |
| `blkid`          | Show UUID/filesystem information   |
| `mount`          | Show/mount filesystems             |
| `umount`         | Unmount filesystem                 |
| `findmnt`        | Show mounted filesystems           |
| `cat /etc/fstab` | View automatic mount configuration |
| `lsof +L1`       | Find deleted files still held open |
| `free -h`        | Check RAM, not disk                |

---

# 🧠 Important Differences

### `df` vs `du`

```text
df
↓
Filesystem-level usage
```

```text
du
↓
Directory/file-level usage
```

Example:

```bash
df -h
```

answers:

> How much space is available on this filesystem?

While:

```bash
du -sh /var/log
```

answers:

> How much space is `/var/log` using?

---

# 🎯 Day 14 Challenge

Without looking at the cheat sheet, try to answer:

### Task 1

Check total disk usage.

```bash
__________
```

### Task 2

Check human-readable disk usage.

```bash
__________
```

### Task 3

Check inode usage.

```bash
__________
```

### Task 4

Find the size of `/var/log`.

```bash
__________
```

### Task 5

List disks and partitions.

```bash
__________
```

### Task 6

Show filesystem types.

```bash
__________
```

### Task 7

Find deleted files still held open.

```bash
__________
```

---

# 💼 Interview Questions

### 1. What is disk management in Linux?

Disk management involves monitoring and managing storage devices, partitions, filesystems, mount points, and disk usage.

### 2. What is the difference between `df` and `du`?

`df` shows filesystem-level disk usage, while `du` shows the storage used by files and directories.

### 3. What does `df -h` do?

It displays filesystem disk usage in human-readable units.

### 4. What does `du -sh` do?

It displays the total size of a directory in human-readable format.

### 5. What is `lsblk`?

It lists block devices such as disks and partitions.

### 6. What is a mount point?

A directory through which a filesystem is accessed.

### 7. What is `/etc/fstab`?

It contains filesystem mount configuration used for mounting filesystems, commonly during system boot.

### 8. What is an inode?

An inode stores metadata about a filesystem object such as ownership, permissions, timestamps, and file information.

### 9. How do you troubleshoot a full disk?

Start with:

```bash
df -h
df -i
```

Then use `du` to identify large directories and investigate logs, application data, and deleted-open files.

### 10. What is the difference between disk space and inode space?

Disk space refers to storage capacity for file data. Inodes represent filesystem metadata entries for files and directories.

---

# 🚀 Real-World Java/Linux Connection

As a Java developer working on Linux servers, you may need to investigate:

```text
Java application logs
JAR files
Tomcat logs
Spring Boot logs
database backups
temporary files
deployment directories
```

Useful commands include:

```bash
df -h
du -sh
du -h --max-depth=1
lsblk -f
findmnt
lsof +L1
```

A developer who understands these commands can quickly identify many storage-related production problems.

---

# 🏁 Day 14 Summary

Today you learned:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount Point
 ↓
Files & Directories
```

Most important commands:

```bash
df -h
df -Th
df -ih
du -sh
du -h --max-depth=1
lsblk -f
blkid
findmnt
mount
umount
lsof +L1
```
🔥 Key Rule

Never modify or format a disk/partition unless you are absolutely sure which device you are operating on.

For learning, prefer safe inspection commands and a dedicated VM/test environment.