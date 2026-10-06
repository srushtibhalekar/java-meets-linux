# 🚀 Day 30 — Linux Automation Final Project

## 🐧 Linux System Health & Automation Tool

Congratulations! 🎉

You have completed 29 days of Linux learning.

Today we will combine the important concepts from the complete roadmap into one practical project.

---

# 🎯 Final Project Goal

We will create a Bash-based **Linux System Health & Automation Tool**.

The project will:

* Check system information
* Check disk usage
* Check memory usage
* Check running processes
* Check network connectivity
* Check important services
* Check recent errors
* Generate a system health report
* Save logs
* Create backups
* Use timestamps
* Be suitable for cron automation
* Be stored in Git/GitHub

---

# 📚 Skills Used

This final project combines:

```text
Day 01 → Linux Basics
Day 02 → Files & Directories
Day 03 → File Management
Day 04 → find
Day 05 → grep
Day 06 → Redirection
Day 07 → Pipes
Day 08 → Permissions
Day 09 → Ownership
Day 10 → Users
Day 11 → Groups
Day 12 → Processes
Day 13 → Process Control
Day 14 → Disk Management
Day 15 → Archives
Day 16 → Package Management
Day 17 → Networking
Day 18 → Network Tools
Day 19 → Environment Variables
Day 20 → Bash Fundamentals
Day 21 → Conditions
Day 22 → Loops
Day 23 → Functions
Day 24 → Bash Scripting
Day 25 → Cron
Day 26 → Logs
Day 27 → Services
Day 28 → SSH
Day 29 → Git
Day 30 → Final Project
```

---

# 📁 Project Structure

Create this structure:

```text
linux-system-health/
│
├── system-health.sh
├── backup.sh
├── config.sh
├── README.md
│
├── reports/
│
├── logs/
│
├── backups/
│
└── .gitignore
```

---

# 1. Create the Project

Open Linux/WSL.

Create the project:

```bash
mkdir linux-system-health
```

Enter it:

```bash
cd linux-system-health
```

Create directories:

```bash
mkdir reports logs backups
```

Create files:

```bash
touch system-health.sh
touch backup.sh
touch config.sh
touch README.md
touch .gitignore
```

Check:

```bash
ls -la
```

---

# 2. Create Configuration File

The configuration file keeps values separate from the main script.

Open:

```bash
nano config.sh
```

Add:

```bash
REPORT_DIR="$HOME/linux-system-health/reports"
LOG_DIR="$HOME/linux-system-health/logs"
BACKUP_DIR="$HOME/linux-system-health/backups"

DISK_THRESHOLD=80
MEMORY_THRESHOLD=80
```

Save the file.

---

# 3. Main Script

Open:

```bash
nano system-health.sh
```

Add:

```bash
#!/bin/bash

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

source "$SCRIPT_DIR/config.sh"

TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
REPORT_FILE="$REPORT_DIR/system-health-$TIMESTAMP.txt"
LOG_FILE="$LOG_DIR/system-health.log"

mkdir -p "$REPORT_DIR"
mkdir -p "$LOG_DIR"

log_message() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" >> "$LOG_FILE"
}

section() {
    echo
    echo "========================================"
    echo "$1"
    echo "========================================"
}

log_message "System health check started"

{
    echo "Linux System Health Report"
    echo "Generated: $(date)"
    echo "Hostname: $(hostname)"
    echo

    section "SYSTEM INFORMATION"

    echo "User: $(whoami)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Operating System:"
    uname -a

    section "UPTIME"

    uptime

    section "CPU INFORMATION"

    if command -v nproc >/dev/null 2>&1; then
        echo "CPU Cores: $(nproc)"
    fi

    section "MEMORY USAGE"

    free -h

    section "DISK USAGE"

    df -h

    section "DISK USAGE PERCENTAGE"

    df -h | awk 'NR>1 {print $5, $6}'

    section "TOP CPU PROCESSES"

    ps aux --sort=-%cpu | head -n 6

    section "TOP MEMORY PROCESSES"

    ps aux --sort=-%mem | head -n 6

    section "RUNNING PROCESSES"

    echo "Total Processes: $(ps -e --no-headers | wc -l)"

    section "NETWORK INFORMATION"

    ip addr 2>/dev/null || echo "ip command not available"

    section "NETWORK ROUTES"

    ip route 2>/dev/null || echo "ip route unavailable"

    section "DNS TEST"

    if getent hosts google.com >/dev/null 2>&1; then
        echo "DNS: PASS"
    else
        echo "DNS: FAILED"
    fi

    section "INTERNET CONNECTIVITY"

    if ping -c 2 -W 2 8.8.8.8 >/dev/null 2>&1; then
        echo "Internet connectivity: PASS"
    else
        echo "Internet connectivity: FAILED"
    fi

    section "IMPORTANT SERVICES"

    for service in ssh cron; do
        if command -v systemctl >/dev/null 2>&1; then
            if systemctl is-active --quiet "$service"; then
                echo "$service: RUNNING"
            else
                echo "$service: NOT RUNNING"
            fi
        else
            echo "$service: systemctl unavailable"
        fi
    done

    section "RECENT SYSTEM ERRORS"

    if command -v journalctl >/dev/null 2>&1; then
        journalctl -p err -n 10 --no-pager 2>/dev/null
    else
        echo "journalctl unavailable"
    fi

    section "LARGE DIRECTORIES"

    du -h --max-depth=1 "$HOME" 2>/dev/null | sort -hr | head -n 10

    section "END OF REPORT"

    echo "Report generated successfully."

} > "$REPORT_FILE" 2>&1

log_message "Report generated: $REPORT_FILE"

echo
echo "========================================"
echo "Linux System Health Check Completed"
echo "========================================"
echo
echo "Report:"
echo "$REPORT_FILE"
echo
echo "Log:"
echo "$LOG_FILE"

log_message "System health check completed"
```

---

# 4. Make the Script Executable

Run:

```bash
chmod +x system-health.sh
```

Check:

```bash
ls -l system-health.sh
```

You should see execute permission, similar to:

```text
-rwxr-xr-x
```

---

# 5. Run the Health Check

Run:

```bash
./system-health.sh
```

You should see:

```text
========================================
Linux System Health Check Completed
========================================
```

You will also get a report inside:

```text
reports/
```

Check:

```bash
ls -lh reports
```

---

# 6. Read the Report

Find the generated report:

```bash
ls -lt reports
```

Then:

```bash
cat reports/system-health-YYYY-MM-DD_HH-MM-SS.txt
```

Replace the filename with the actual generated filename.

A simpler way:

```bash
less reports/*
```

---

# 7. Check the Log

Run:

```bash
cat logs/system-health.log
```

You may see:

```text
[2026-10-06 11:30:00] System health check started
[2026-10-06 11:30:01] Report generated: ...
[2026-10-06 11:30:01] System health check completed
```

---

# 8. Why Use Functions?

The script uses:

```bash
log_message()
```

and:

```bash
section()
```

This avoids repeating the same code.

For example:

```bash
section "MEMORY USAGE"
```

prints a consistent heading.

This connects directly with **Day 23 — Bash Functions**.

---

# 9. Memory Monitoring

The script uses:

```bash
free -h
```

Example:

```text
               total   used   free
Mem:            7.7Gi   3.1Gi  ...
```

`-h` means human-readable.

---

# 10. Disk Monitoring

The script uses:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/sda1        50G   30G   20G  60%
```

The important value is:

```text
Use%
```

If disk usage becomes very high, applications may fail.

---

# 11. Process Monitoring

The project uses:

```bash
ps aux --sort=-%cpu
```

This identifies processes consuming high CPU.

It also uses:

```bash
ps aux --sort=-%mem
```

to identify processes consuming high memory.

This connects with:

```text
Day 12 → Processes
Day 13 → Process Control
```

---

# 12. Network Monitoring

The script checks:

```bash
ip addr
```

and:

```bash
ip route
```

It also performs a DNS check:

```bash
getent hosts google.com
```

and an Internet connectivity test:

```bash
ping -c 2 8.8.8.8
```

---

# 13. Service Monitoring

The script checks:

```bash
systemctl is-active ssh
```

and:

```bash
systemctl is-active cron
```

This connects with:

```text
Day 27 → Services & systemctl
```

The exact service names can differ between Linux distributions.

For example, some systems may use:

```text
sshd
```

instead of:

```text
ssh
```

---

# 14. Log Monitoring

The script uses:

```bash
journalctl -p err -n 10
```

This shows recent high-priority errors from the system journal.

This connects with:

```text
Day 26 → Logs & journalctl
```

---

# 15. Directory Size Monitoring

The script uses:

```bash
du -h --max-depth=1 "$HOME"
```

and:

```bash
sort -hr
```

Then:

```bash
head -n 10
```

So the pipeline becomes:

```text
du
 ↓
sort
 ↓
head
```

This connects with:

```text
Day 07 → Pipes
Day 14 → Disk Management
```

---

# 16. Backup Script

Now create a backup script.

Open:

```bash
nano backup.sh
```

Add:

```bash
#!/bin/bash

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

source "$SCRIPT_DIR/config.sh"

TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_FILE="$BACKUP_DIR/linux-system-health-$TIMESTAMP.tar.gz"

mkdir -p "$BACKUP_DIR"

tar -czf "$BACKUP_FILE" \
    --exclude="$BACKUP_DIR" \
    --exclude=".git" \
    "$SCRIPT_DIR"

echo "Backup created:"
echo "$BACKUP_FILE"
```

---

# 17. Make Backup Script Executable

```bash
chmod +x backup.sh
```

Run:

```bash
./backup.sh
```

Check:

```bash
ls -lh backups
```

You should see a `.tar.gz` file.

---

# 18. Test the Backup

List the archive:

```bash
tar -tzf backups/*.tar.gz
```

Extract it into a test directory:

```bash
mkdir backup-test
```

Then:

```bash
tar -xzf backups/*.tar.gz -C backup-test
```

Check:

```bash
find backup-test -type f
```

This combines:

```text
Day 15 → Archives & Compression
Day 04 → find
```

---

# 19. Add .gitignore

Open:

```bash
nano .gitignore
```

Add:

```gitignore
# Generated reports
reports/

# Generated logs
logs/

# Generated backups
backups/

# Temporary files
*.tmp

# Environment files
.env

# Editor files
*.swp
```

We generally don't want generated reports, logs, and backups committed automatically.

If you want sample reports in your portfolio, create a separate sanitized sample file instead.

---

# 20. Test .gitignore

Run:

```bash
git status
```

Generated files under:

```text
reports/
logs/
backups/
```

should be ignored.

Check ignored files:

```bash
git status --ignored
```

---

# 21. Add a README

Open:

```bash
nano README.md
```

Add:

````markdown
# 🐧 Linux System Health & Automation Tool

A Bash-based Linux system monitoring and automation project.

## Features

- System information
- CPU information
- Memory monitoring
- Disk monitoring
- Process monitoring
- Network checks
- DNS testing
- Service monitoring
- Recent error detection
- System health reports
- Log generation
- Automated backups
- Git/GitHub integration

## Technologies

- Linux
- Bash
- Shell scripting
- systemctl
- journalctl
- Git
- GitHub
- tar

## Project Structure

```text
linux-system-health/
├── system-health.sh
├── backup.sh
├── config.sh
├── README.md
├── .gitignore
├── reports/
├── logs/
└── backups/
````

## Run

```bash
chmod +x system-health.sh
./system-health.sh
```

## Backup

```bash
chmod +x backup.sh
./backup.sh
```

## Skills Practiced

* Linux commands
* Bash scripting
* Functions
* Conditions
* Loops
* Pipes
* Redirection
* Processes
* Services
* Logs
* Networking
* Archives
* Cron automation
* Git

````

---

# 22. Test the Complete Project

Run:

```bash
./system-health.sh
````

Then:

```bash
./backup.sh
```

Check everything:

```bash
ls -lah
```

Check reports:

```bash
ls -lh reports
```

Check logs:

```bash
cat logs/system-health.log
```

Check backups:

```bash
ls -lh backups
```

---

# 23. Add Cron Automation

Now automate the health check.

Find the full path of the script:

```bash
pwd
```

Example:

```text
/home/user/linux-system-health
```

Open your crontab:

```bash
crontab -e
```

Add:

```cron
0 * * * * /home/user/linux-system-health/system-health.sh
```

This means:

```text
Every hour
    ↓
Run system-health.sh
```

**Important:** Replace the example path with the actual absolute path on your Linux system.

---

# 24. Verify Cron

List your cron jobs:

```bash
crontab -l
```

You should see:

```cron
0 * * * * /home/user/linux-system-health/system-health.sh
```

Check the generated reports after the scheduled run.

---

# 25. Cron + Logging

You can explicitly redirect cron output:

```cron
0 * * * * /home/user/linux-system-health/system-health.sh >> /home/user/linux-system-health/logs/cron.log 2>&1
```

This combines:

```text
Cron
 +
Output Redirection
 +
Logging
```

from Days 25 and 6.

---

# 26. Optional — Daily Backup

You can schedule a daily backup:

```cron
0 23 * * * /home/user/linux-system-health/backup.sh >> /home/user/linux-system-health/logs/backup.log 2>&1
```

Meaning:

```text
23:00 every day
      ↓
Run backup.sh
```

---

# 27. Check Cron Service

On Debian/Ubuntu:

```bash
systemctl status cron
```

On systems using `crond`:

```bash
systemctl status crond
```

If you have permission and need to start it:

```bash
sudo systemctl start cron
```

or:

```bash
sudo systemctl start crond
```

The exact service name depends on the distribution.

---

# 28. Final Project Architecture

Your project now looks like:

```text
                 Linux System
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      CPU           Memory         Disk
        │             │             │
        └─────────────┼─────────────┘
                      ↓
              system-health.sh
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
    Network        Services         Logs
       │              │              │
       └──────────────┼──────────────┘
                      ↓
                Health Report
                      │
             ┌────────┴────────┐
             ↓                 ↓
           logs             reports
                              
                      ↓
                  backup.sh
                      ↓
                   backups
                      │
                      ↓
                    Git
                      │
                      ↓
                   GitHub
```

---

# 29. Git Workflow

Initialize the project:

```bash
git init
```

Check:

```bash
git status
```

Add important files:

```bash
git add system-health.sh backup.sh config.sh README.md .gitignore
```

Check:

```bash
git status
```

Commit:

```bash
git commit -m "feat: add Linux system health automation tool"
```

---

# 30. Connect to GitHub

Create a new GitHub repository for the project.

Then add the remote:

```bash
git remote add origin git@github.com:USERNAME/linux-system-health.git
```

Replace:

```text
USERNAME
```

with your GitHub username.

Check:

```bash
git remote -v
```

Rename the branch if required:

```bash
git branch -M main
```

Push:

```bash
git push -u origin main
```

---

# 31. Future Updates

When you modify the project:

```bash
git status
```

Review:

```bash
git diff
```

Stage:

```bash
git add .
```

Commit:

```bash
git commit -m "feat: improve system monitoring"
```

Push:

```bash
git push
```

---

# 32. Add a Version Tag

After reaching a stable version:

```bash
git tag v1.0.0
```

View tags:

```bash
git tag
```

Push the tag:

```bash
git push origin v1.0.0
```

This gives your project a release/version marker.

---

# 33. Optional Java Integration

Because you are targeting Java development, you can extend the project later.

For example:

```text
Bash
 ↓
Collect Linux information
 ↓
Save report
 ↓
Java application
 ↓
Read/analyze report
 ↓
Generate dashboard
```

A future version could use:

```text
Java
Spring Boot
REST API
PostgreSQL
Linux
Bash
```

But for this final Linux project, Bash should remain the main implementation.

---

# 34. Security Considerations

This project collects system information.

Be careful when sharing generated reports publicly.

Reports may contain:

* Hostnames
* Usernames
* IP addresses
* Process names
* Service information
* File paths
* Error messages

Before uploading a report to GitHub:

```bash
cat report.txt
```

Check for sensitive information.

Do not commit:

```text
Passwords
Private keys
API keys
Tokens
.env files
Personal credentials
```

---

# 35. Troubleshooting

## Permission denied

Run:

```bash
chmod +x system-health.sh
```

Then:

```bash
./system-health.sh
```

---

## systemctl unavailable

Some environments, especially certain WSL configurations or containers, may not run systemd.

Check:

```bash
command -v systemctl
```

If systemd is unavailable, the script reports that service checks cannot be performed.

---

## journalctl unavailable

Check:

```bash
command -v journalctl
```

Not every minimal environment has a systemd journal.

---

## ping unavailable

Check:

```bash
command -v ping
```

If missing, install the appropriate networking package for your distribution.

---

## df command unavailable

This is uncommon on a normal Linux installation.

Check:

```bash
command -v df
```

---

# 36. Final Testing Checklist

Run each of these:

```bash
./system-health.sh
```

```bash
./backup.sh
```

```bash
ls -lh reports
```

```bash
ls -lh logs
```

```bash
ls -lh backups
```

```bash
git status
```

```bash
git log --oneline
```

```bash
crontab -l
```

Then verify:

```text
[✓] Script executes
[✓] Report generated
[✓] Log generated
[✓] Backup generated
[✓] Disk information works
[✓] Memory information works
[✓] Process information works
[✓] Network checks work
[✓] Service checks work where systemd is available
[✓] .gitignore works
[✓] Git repository works
[✓] Project is documented
```

---

# 🧠 Complete 30-Day Linux Revision

## Days 1–5

```text
Linux Basics
Files & Directories
Viewing Files
find
grep
```

Important commands:

```bash
pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
less
head
tail
find
grep
```

---

## Days 6–10

```text
Redirection
Pipes
Permissions
Ownership
Users
```

Important commands:

```bash
>
>>
2>
|
chmod
chown
chgrp
whoami
id
useradd
passwd
```

---

## Days 11–15

```text
Groups
Processes
Process Control
Disk Management
Archives
```

Important commands:

```bash
groupadd
ps
top
kill
nice
renice
df
du
lsblk
tar
gzip
zip
```

---

## Days 16–20

```text
Package Management
Networking
Network Tools
Environment Variables
Bash Fundamentals
```

Important commands:

```bash
apt
dnf
ip
ping
ss
curl
wget
dig
export
printenv
env
bash
```

---

## Days 21–25

```text
Conditions
Loops
Functions
Bash Scripting
Cron
```

Important concepts:

```text
if
case
for
while
until
function
$1
$2
$?
cron
crontab
```

---

## Days 26–30

```text
Logs
Services
SSH
Git
Final Automation Project
```

Important commands:

```bash
journalctl
systemctl
ssh
scp
sftp
git
```

---

# 🎤 Final Linux Interview Questions

### Q1. What is Linux?

Linux is an open-source operating system kernel used as the foundation for many operating systems and servers.

---

### Q2. What is a shell?

A shell is a command interpreter that allows users to interact with the operating system.

Examples include:

```text
bash
zsh
fish
```

---

### Q3. What is the difference between a process and a program?

A program is a set of instructions stored on disk. A process is a running instance of a program.

---

### Q4. What is a PID?

PID stands for Process ID. It uniquely identifies a running process within the system.

---

### Q5. What is `chmod`?

`chmod` changes file or directory permissions.

Example:

```bash
chmod 755 script.sh
```

---

### Q6. What is `chown`?

`chown` changes file ownership.

Example:

```bash
sudo chown user:group file.txt
```

---

### Q7. What is `grep`?

`grep` searches text for matching patterns.

Example:

```bash
grep "ERROR" application.log
```

---

### Q8. What is a pipe?

A pipe sends the output of one command to another command.

Example:

```bash
ps aux | grep java
```

---

### Q9. What is cron?

Cron is a scheduler used to run commands or scripts automatically at specified times.

---

### Q10. What is `systemctl`?

`systemctl` is commonly used to manage systemd services and units.

Example:

```bash
systemctl status ssh
```

---

### Q11. What is SSH?

SSH provides secure remote access to another system.

Example:

```bash
ssh user@server
```

---

### Q12. How do you copy a file to a remote Linux server?

```bash
scp app.jar user@server:/home/user/
```

---

### Q13. How do you check disk usage?

```bash
df -h
```

---

### Q14. How do you find which directories are using space?

```bash
du -h --max-depth=1
```

---

### Q15. How do you see running processes?

```bash
ps aux
```

---

### Q16. How do you terminate a process?

```bash
kill PID
```

If necessary, a forceful signal can be used:

```bash
kill -9 PID
```

Normally, try graceful termination first.

---

### Q17. What is Git?

Git is a distributed version control system used to track changes in files and collaborate on software projects.

---

### Q18. What is GitHub?

GitHub is a platform for hosting Git repositories and collaborating on software projects.

---

### Q19. What is `.gitignore`?

It defines patterns for untracked files that Git should ignore.

---

### Q20. How do you push code to GitHub?

```bash
git add .
git commit -m "message"
git push
```

---

# 🏆 Final Project Skills

After completing this project, you can say that you practiced:

```text
✓ Linux Command Line
✓ Bash Scripting
✓ File Management
✓ Permissions
✓ Users & Groups
✓ Process Management
✓ Disk Management
✓ Networking
✓ Package Management
✓ Logs
✓ systemd Services
✓ SSH
✓ Cron Automation
✓ Git
✓ GitHub
✓ Backup Automation
✓ System Monitoring
```

---

# 📁 Final Repository Structure

Your complete `java-meets-linux` repository should now look like:

```text
Java-Meets-Linux/
│
├── DAY1/
│   └── Linux-Basics.md
├── DAY2/
│   └── Files-Directories.md
├── DAY3/
│   └── Viewing-Managing-Files.md
├── DAY4/
│   └── File-Searching.md
├── DAY5/
│   └── Grep.md
├── DAY6/
│   └── Redirection.md
├── DAY7/
│   └── Pipes-Chaining.md
├── DAY8/
│   └── File-Permissions.md
├── DAY9/
│   └── Ownership.md
├── DAY10/
│   └── Users.md
├── DAY11/
│   └── Groups.md
├── DAY12/
│   └── Processes.md
├── DAY13/
│   └── Process-Control.md
├── DAY14/
│   └── Disk-Management.md
├── DAY15/
│   └── Archives-Compression.md
├── DAY16/
│   └── Package-Management.md
├── DAY17/
│   └── Networking-Basics.md
├── DAY18/
│   └── Network-Tools.md
├── DAY19/
│   └── Environment-Variables.md
├── DAY20/
│   └── Bash-Fundamentals.md
├── DAY21/
│   └── Bash-Conditions.md
├── DAY22/
│   └── Bash-Loops.md
├── DAY23/
│   └── Bash-Functions.md
├── DAY24/
│   └── Bash-Scripting.md
├── DAY25/
│   └── Cron-Automation.md
├── DAY26/
│   └── Logs-Journalctl.md
├── DAY27/
│   └── Services-Systemctl.md
├── DAY28/
│   └── SSH-Remote-Access.md
├── DAY29/
│   └── Linux-Git.md
└── DAY30/
    └── Final-Project.md
```

---

# 🚀 Push Day 30 to GitHub

For your existing repository on Windows:

```powershell
cd C:\Desktop\Java-Meets-Linux
```

Check:

```powershell
git status
```

Add:

```powershell
git add Day30\Final-Project.md
```

Check:

```powershell
git status
```

Commit:

```powershell
git commit -m "docs: add day 30 Linux automation final project"
```

Push:

```powershell
git push
```

---

# 🎓 30-Day Linux Roadmap Completed!

```text
DAY 01  ✓ Linux Basics
DAY 02  ✓ Files & Directories
DAY 03  ✓ Viewing & Managing Files
DAY 04  ✓ File Searching
DAY 05  ✓ grep
DAY 06  ✓ Redirection
DAY 07  ✓ Pipes & Chaining
DAY 08  ✓ Permissions
DAY 09  ✓ Ownership
DAY 10  ✓ Users
DAY 11  ✓ Groups
DAY 12  ✓ Processes
DAY 13  ✓ Process Control
DAY 14  ✓ Disk Management
DAY 15  ✓ Archives & Compression
DAY 16  ✓ Package Management
DAY 17  ✓ Networking Basics
DAY 18  ✓ Network Tools
DAY 19  ✓ Environment Variables
DAY 20  ✓ Bash Fundamentals
DAY 21  ✓ Bash Conditions
DAY 22  ✓ Bash Loops
DAY 23  ✓ Bash Functions
DAY 24  ✓ Bash Scripting
DAY 25  ✓ Cron Automation
DAY 26  ✓ Logs & journalctl
DAY 27  ✓ Services & systemctl
DAY 28  ✓ SSH & Remote Access
DAY 29  ✓ Linux + Git
DAY 30  ✓ Final Automation Project
```

## 🏆 Final Outcome

You now have a structured Linux learning repository plus a practical automation project that demonstrates:

```text
Linux
+
Bash
+
System Administration
+
Networking
+
Automation
+
Git
+
GitHub
+
Java Developer Workflow
```

This is a strong foundation for **Java Developer, Backend Developer, Linux-based development, and entry-level DevOps-oriented roles**.
