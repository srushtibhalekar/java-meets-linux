# ⏰ Day 25 — Cron Jobs & Automation

## 🎯 Learning Objectives

By the end of Day 25, you will understand:

* What cron is
* What a cron job is
* `crontab`
* Cron syntax
* Scheduling commands
* Scheduling Bash scripts
* `cron.hourly`, `cron.daily`, `cron.weekly`
* Viewing cron jobs
* Removing cron jobs
* Cron environment variables
* Logging cron jobs
* Troubleshooting cron
* Automated backups
* Automated monitoring
* Java developer use cases
* Building a practical automation project

---

# 1. What Is Cron?

**Cron** is a Linux service used to automatically execute commands or scripts at scheduled times.

For example:

```text
Every minute
Every hour
Every day
Every week
Every Monday
Every night at 11 PM
```

Instead of manually running:

```bash
./backup.sh
```

you can tell Linux:

> Run this script automatically every day at 11 PM.

---

# 2. What Is a Cron Job?

A **cron job** is a scheduled command.

Example:

```bash
0 23 * * * /home/user/backup.sh
```

This means:

```text
At 23:00
Every day
Run backup.sh
```

---

# 3. Why Is Cron Important?

Cron is commonly used for:

* Backups
* Log cleanup
* Monitoring
* Report generation
* Database backups
* File synchronization
* Temporary file cleanup
* System maintenance
* Health checks
* Automated scripts

For a Java developer:

```text
Java Application
       ↓
Generate logs
       ↓
Cron
       ↓
Backup logs
       ↓
Archive
       ↓
Store backup
```

---

# 4. Check Whether Cron Is Available

On many Linux distributions:

```bash
systemctl status cron
```

On some distributions the service is named:

```bash
systemctl status crond
```

You may see:

```text
active (running)
```

The exact service name depends on the Linux distribution.

---

# 5. What Is `crontab`?

`crontab` stands for **cron table**.

It stores scheduled jobs for a user.

View your current cron jobs:

```bash
crontab -l
```

If you have no jobs, you may see:

```text
no crontab for user
```

---

# 6. Open Your Crontab

Use:

```bash
crontab -e
```

This opens your user's cron configuration.

You can add scheduled commands there.

---

# 7. Basic Cron Syntax

A cron entry has five time fields followed by the command:

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of week
│ │ │ └──── Month
│ │ └────── Day of month
│ └──────── Hour
└────────── Minute
```

General format:

```text
minute hour day-of-month month day-of-week command
```

---

# 8. Cron Field Values

| Field        | Allowed values |
| ------------ | -------------- |
| Minute       | `0-59`         |
| Hour         | `0-23`         |
| Day of month | `1-31`         |
| Month        | `1-12`         |
| Day of week  | `0-7`          |

For day of week:

```text
0 or 7 = Sunday
1 = Monday
2 = Tuesday
3 = Wednesday
4 = Thursday
5 = Friday
6 = Saturday
```

---

# 9. The `*` Symbol

`*` means:

> Every possible value.

Example:

```bash
* * * * * command
```

means:

> Run every minute.

---

# 10. Run Every Minute

Example:

```text
* * * * * /home/user/test.sh
```

This runs:

```text
12:01
12:02
12:03
12:04
...
```

every minute.

### ⚠️ Beginner warning

Do not use frequent cron jobs for expensive commands on a real server without understanding their impact.

For practice, use a simple logging script.

---

# 11. Run Every Hour

```text
0 * * * * /home/user/script.sh
```

Meaning:

```text
At minute 0 of every hour
```

Examples:

```text
10:00
11:00
12:00
13:00
```

---

# 12. Run Every Day

```text
0 0 * * * /home/user/script.sh
```

Meaning:

```text
Every day at midnight
```

---

# 13. Run Every Day at 11 PM

```text
0 23 * * * /home/user/script.sh
```

Meaning:

```text
23:00 every day
```

---

# 14. Run Every Monday

```text
0 9 * * 1 /home/user/script.sh
```

Meaning:

```text
Every Monday at 9:00 AM
```

---

# 15. Run on a Specific Day of the Month

Example:

```text
0 8 1 * * /home/user/report.sh
```

Meaning:

```text
8:00 AM
on the first day
of every month
```

---

# 16. Run Every 5 Minutes

```text
*/5 * * * * /home/user/script.sh
```

Meaning:

```text
Every 5 minutes
```

Examples:

```text
10:00
10:05
10:10
10:15
...
```

---

# 17. Run Every 10 Minutes

```text
*/10 * * * * /home/user/script.sh
```

---

# 18. Run Every 30 Minutes

```text
*/30 * * * * /home/user/script.sh
```

---

# 19. Run During Specific Hours

Example:

```text
0 9-17 * * * /home/user/script.sh
```

Runs at:

```text
09:00
10:00
11:00
...
17:00
```

---

# 20. Multiple Values

You can specify multiple values.

Example:

```text
0 9,13,18 * * * /home/user/script.sh
```

Runs at:

```text
09:00
13:00
18:00
```

---

# 21. Ranges

Example:

```text
0 9-17 * * 1-5 /home/user/script.sh
```

Meaning:

```text
9 AM to 5 PM
Monday to Friday
```

---

# 22. Combining Values

Example:

```text
*/15 9-17 * * 1-5 /home/user/script.sh
```

Meaning:

```text
Every 15 minutes
Between 9 AM and 5 PM
Monday through Friday
```

---

# 23. Cron Examples Cheat Sheet

| Cron             | Meaning                     |
| ---------------- | --------------------------- |
| `* * * * *`      | Every minute                |
| `0 * * * *`      | Every hour                  |
| `0 0 * * *`      | Every day at midnight       |
| `0 23 * * *`     | Every day at 11 PM          |
| `0 9 * * 1`      | Monday at 9 AM              |
| `*/5 * * * *`    | Every 5 minutes             |
| `*/15 * * * *`   | Every 15 minutes            |
| `0 9-17 * * 1-5` | Hourly, 9 AM–5 PM, weekdays |
| `0 0 1 * *`      | First day of every month    |

---

# 24. Create Your First Scheduled Script

Create:

```bash
nano cron-test.sh
```

Add:

```bash
#!/bin/bash

echo "Cron executed at $(date)" >> "$HOME/cron-test.log"
```

Make it executable:

```bash
chmod +x cron-test.sh
```

Test manually:

```bash
./cron-test.sh
```

Check:

```bash
cat "$HOME/cron-test.log"
```

---

# 25. Get the Full Script Path

Cron should generally use an absolute path.

Find your current directory:

```bash
pwd
```

Example:

```text
/home/srushti/scripts
```

Your script would be:

```text
/home/srushti/scripts/cron-test.sh
```

---

# 26. Add Your First Cron Job

Open:

```bash
crontab -e
```

Add:

```text
* * * * * /home/srushti/scripts/cron-test.sh
```

Replace the path with the actual path from your system.

Save and exit.

---

# 27. Verify the Cron Job

Run:

```bash
crontab -l
```

You should see:

```text
* * * * * /home/srushti/scripts/cron-test.sh
```

Wait for the next minute.

Then:

```bash
cat "$HOME/cron-test.log"
```

You should see timestamps.

---

# 28. Remove a Cron Job

Open:

```bash
crontab -e
```

Remove the specific line.

Then save.

Verify:

```bash
crontab -l
```

---

# 29. Remove All User Cron Jobs

Use:

```bash
crontab -r
```

### ⚠️ Warning

This removes the user's entire crontab.

Do not run it if you have important scheduled jobs.

---

# 30. Cron and Absolute Paths

A common mistake is:

```text
* * * * * ./backup.sh
```

This may fail because cron does not necessarily start in the directory you expect.

Prefer:

```text
* * * * * /home/user/scripts/backup.sh
```

---

# 31. Cron and `PATH`

Your interactive shell may have:

```bash
echo "$PATH"
```

with many directories.

Cron may have a more limited environment.

Therefore this:

```bash
java
```

might behave differently under cron.

A safer approach is to use the correct executable path or explicitly configure the environment.

Find Java:

```bash
command -v java
```

Example:

```text
/usr/bin/java
```

---

# 32. Setting `PATH` in Crontab

You can define environment variables in your crontab:

```text
PATH=/usr/local/bin:/usr/bin:/bin

0 23 * * * /home/user/backup.sh
```

Keep environment requirements explicit for important jobs.

---

# 33. Cron and Java

Suppose Java is installed at:

```text
/usr/bin/java
```

You could use:

```bash
#!/bin/bash

/usr/bin/java -version
```

For applications that depend on `JAVA_HOME`, configure the environment explicitly rather than assuming your interactive shell configuration will be loaded.

---

# 34. Cron Logging

Instead of:

```text
0 23 * * * /home/user/backup.sh
```

you can redirect output:

```text
0 23 * * * /home/user/backup.sh >> /home/user/backup.log 2>&1
```

This means:

```text
stdout → backup.log
stderr → backup.log
```

---

# 35. Understanding `2>&1`

Remember Day 6?

```bash
2>&1
```

means:

> Send standard error to the same destination as standard output.

So:

```text
>> backup.log 2>&1
```

stores both normal output and errors in the same log file.

---

# 36. Cron Logging Example

Crontab:

```text
*/5 * * * * /home/user/monitor.sh >> /home/user/monitor.log 2>&1
```

The script runs every 5 minutes.

Output goes to:

```text
monitor.log
```

---

# 37. Create a Backup Script

Create:

```bash
nano backup.sh
```

Add:

```bash
#!/bin/bash

SOURCE="$HOME/project"
BACKUP_DIR="$HOME/backups"

mkdir -p "$BACKUP_DIR"

if [ ! -d "$SOURCE" ]; then
    echo "Source directory not found."
    exit 1
fi

TIMESTAMP=$(date +"%Y%m%d_%H%M%S")

BACKUP_FILE="$BACKUP_DIR/project_$TIMESTAMP.tar.gz"

tar -czf "$BACKUP_FILE" "$SOURCE"

echo "Backup created: $BACKUP_FILE"
```

Make executable:

```bash
chmod +x backup.sh
```

Test:

```bash
./backup.sh
```

---

# 38. Schedule the Backup

Open:

```bash
crontab -e
```

Add:

```text
0 23 * * * /home/user/backup.sh >> /home/user/backup.log 2>&1
```

Now Linux can automatically run the backup every night at 11 PM.

---

# 39. Daily Backup Workflow

The complete workflow becomes:

```text
Cron
  ↓
Run backup.sh
  ↓
Check project directory
  ↓
Create timestamp
  ↓
Create tar.gz
  ↓
Save backup
  ↓
Write result to log
```

---

# 40. `cron.daily`

Some Linux systems provide directories such as:

```text
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
```

These provide another way to organize periodic tasks.

For example:

```text
/etc/cron.daily/
```

contains jobs intended to run daily.

The exact execution mechanism and timing can vary by distribution and configuration.

---

# 41. Cron vs `systemd` Timers

Linux systems may use:

```text
cron
```

or:

```text
systemd timers
```

for scheduled tasks.

### Cron

Simple and widely used for:

* Backups
* Small scripts
* Periodic jobs

### systemd timers

Useful when you need:

* Service integration
* Detailed status
* Dependencies
* More control over execution
* Journal logging

You will learn `systemctl` and services later in this roadmap.

---

# 42. View Cron Service Status

Depending on the distribution:

```bash
systemctl status cron
```

or:

```bash
systemctl status crond
```

If available, you may see:

```text
Active: active (running)
```

---

# 43. Start Cron Service

On systems using `cron`:

```bash
sudo systemctl start cron
```

On systems using `crond`:

```bash
sudo systemctl start crond
```

---

# 44. Enable Cron at Boot

Depending on the service name:

```bash
sudo systemctl enable cron
```

or:

```bash
sudo systemctl enable crond
```

Some distributions configure this differently, so always check the service name first.

---

# 45. Check Cron Logs

On many systemd-based Linux systems, you can inspect logs with:

```bash
journalctl
```

You can filter for cron:

```bash
journalctl -u cron
```

or:

```bash
journalctl -u crond
```

The exact logging setup depends on the distribution.

---

# 46. Troubleshooting Cron

If a cron job does not run, check these in order:

### 1. Is the job present?

```bash
crontab -l
```

### 2. Is the script executable?

```bash
ls -l backup.sh
```

### 3. Does the script run manually?

```bash
./backup.sh
```

### 4. Is the path absolute?

Use:

```text
/home/user/scripts/backup.sh
```

instead of:

```text
./backup.sh
```

### 5. Check the script's environment.

### 6. Check logs.

### 7. Check cron service status.

---

# 47. Common Cron Mistake — Relative Paths

Avoid:

```text
0 23 * * * ./backup.sh
```

Prefer:

```text
0 23 * * * /home/user/scripts/backup.sh
```

---

# 48. Common Cron Mistake — Missing Execute Permission

Check:

```bash
ls -l backup.sh
```

If necessary:

```bash
chmod +x backup.sh
```

---

# 49. Common Cron Mistake — Script Works Manually but Not in Cron

This often happens because the environments differ.

Check:

```bash
echo "$PATH"
```

Find commands:

```bash
command -v java
command -v tar
command -v grep
```

Use appropriate absolute paths or configure `PATH` explicitly.

---

# 50. Common Cron Mistake — No Logging

Bad:

```text
0 23 * * * /home/user/backup.sh
```

Better for troubleshooting:

```text
0 23 * * * /home/user/backup.sh >> /home/user/backup.log 2>&1
```

---

# 51. Cron and Environment Variables

Cron may not load your normal interactive shell configuration.

For example, variables from:

```text
~/.bashrc
```

should not automatically be assumed to be available in every cron execution context.

If your script needs a variable, define it explicitly or load the required environment deliberately.

---

# 52. Example — Java Environment

Check:

```bash
command -v java
echo "$JAVA_HOME"
```

If your Java application requires `JAVA_HOME`, the script can define it explicitly:

```bash
export JAVA_HOME="/path/to/java"
export PATH="$JAVA_HOME/bin:$PATH"
```

Use the actual Java installation path on your system.

---

# 53. Monitoring Script

Create:

```bash
nano monitor.sh
```

Add:

```bash
#!/bin/bash

echo "===== SYSTEM MONITOR ====="
echo "Date: $(date)"
echo "User: $(whoami)"
echo

echo "Disk:"
df -h /

echo
echo "Memory:"
free -h

echo
echo "Java:"
if command -v java >/dev/null 2>&1; then
    java -version
else
    echo "Java not installed."
fi
```

Run:

```bash
chmod +x monitor.sh
./monitor.sh
```

---

# 54. Schedule Monitoring

For testing:

```text
*/10 * * * * /home/user/monitor.sh >> /home/user/monitor.log 2>&1
```

This runs every 10 minutes.

For production systems, choose a frequency appropriate to the workload and monitoring requirements.

---

# 🧪 55. Practice Lab 1 — Every Minute

Create:

```text
cron-practice.sh
```

Script:

```bash
#!/bin/bash

echo "Cron ran at $(date)" >> "$HOME/cron-practice.log"
```

Make executable:

```bash
chmod +x cron-practice.sh
```

Add a one-minute cron entry:

```text
* * * * * /absolute/path/to/cron-practice.sh
```

Wait and check:

```bash
cat "$HOME/cron-practice.log"
```

After testing, **remove the cron entry**.

---

# 🧪 56. Practice Lab 2 — Scheduled Backup

Create:

```text
backup.sh
```

Requirements:

* Accept a source directory.
* Create a backup directory.
* Generate a timestamp.
* Create `.tar.gz`.
* Log success or failure.

Example:

```bash
./backup.sh "$HOME/project"
```

---

# 🧪 57. Practice Lab 3 — Disk Monitoring

Create:

```text
disk-monitor.sh
```

Display:

```text
Date
Hostname
Disk usage
```

Use:

```bash
df -h /
```

Add a cron job that writes the result to:

```text
disk-monitor.log
```

---

# 🧪 58. Practice Lab 4 — Java Environment Monitor

Create:

```text
java-monitor.sh
```

Check:

```text
Java
Javac
Git
Maven
```

Write the results to:

```text
java-monitor.log
```

Schedule it using cron.

---

# 🚀 59. Mini Project — Automated Linux Backup System

Create:

```text
Day25-Project/
├── backup.sh
├── backup.log
└── README.md
```

The script should:

1. Accept a project directory.
2. Check that it exists.
3. Create a backup directory.
4. Generate a timestamp.
5. Create `.tar.gz`.
6. Record the result.
7. Return an appropriate exit code.

Example:

```bash
./backup.sh "$HOME/JavaProject"
```

Expected:

```text
Starting backup...
Source: /home/user/JavaProject
Backup: /home/user/backups/JavaProject_20261001_230000.tar.gz
Backup completed successfully.
```

---

# 60. Add Automatic Scheduling

After testing manually, schedule it:

```text
0 23 * * * /home/user/Day25-Project/backup.sh /home/user/JavaProject >> /home/user/Day25-Project/backup.log 2>&1
```

Meaning:

```text
Every day
At 11 PM
Run backup.sh
Backup JavaProject
Store output in backup.log
```

Replace the paths with your actual Linux paths.

---

# 61. Backup Retention

A backup system can eventually create many files.

You can remove backups older than a certain number of days.

Example:

```bash
find "$BACKUP_DIR" -type f -name "*.tar.gz" -mtime +7 -delete
```

Meaning:

> Delete matching backup files older than 7 days.

### ⚠️ Important

Test destructive `find` commands carefully.

First inspect:

```bash
find "$BACKUP_DIR" -type f -name "*.tar.gz" -mtime +7
```

Only add:

```text
-delete
```

after confirming the files are correct.

---

# 62. Improved Backup Workflow

```text
        Cron
          ↓
     backup.sh
          ↓
   Check source
          ↓
   Create timestamp
          ↓
    Create archive
          ↓
    Write log
          ↓
 Remove old backups
          ↓
       Done
```

---

# ❓ 63. Interview Questions

### Q1. What is cron?

Cron is a Linux scheduling mechanism used to automatically execute commands or scripts at specified times.

### Q2. What is a cron job?

A cron job is a scheduled command stored in a user's crontab or system scheduling configuration.

### Q3. How do you view cron jobs?

```bash
crontab -l
```

### Q4. How do you edit cron jobs?

```bash
crontab -e
```

### Q5. What does this mean?

```text
0 23 * * *
```

It means at 11:00 PM every day.

### Q6. What does this mean?

```text
*/5 * * * *
```

It means every 5 minutes.

### Q7. What are the five cron time fields?

```text
Minute
Hour
Day of month
Month
Day of week
```

### Q8. Why should cron scripts usually use absolute paths?

Cron runs with a different execution environment and working directory than an interactive shell, so relative paths can fail.

### Q9. How do you log cron output?

```text
command >> logfile 2>&1
```

### Q10. How do you remove a cron job?

Open:

```bash
crontab -e
```

and remove the relevant entry.

### Q11. What is `crontab -r`?

It removes the user's entire crontab.

### Q12. Why might a script work manually but fail under cron?

Possible reasons include:

* Different `PATH`
* Different environment variables
* Relative paths
* Permissions
* Different working directory
* Missing interpreter or executable path

### Q13. What is the difference between cron and a Bash script?

A Bash script contains the commands.

Cron schedules when those commands should run.

### Q14. Why is logging important for cron jobs?

Cron jobs often run without an interactive terminal, so logs help identify whether the job ran and whether it failed.

---

# 📌 64. Day 25 Cheat Sheet

| Command / Syntax        | Purpose                               |
| ----------------------- | ------------------------------------- |
| `crontab -l`            | List cron jobs                        |
| `crontab -e`            | Edit cron jobs                        |
| `crontab -r`            | Remove all user cron jobs             |
| `*`                     | Every value                           |
| `*/5`                   | Every 5 units                         |
| `0 23 * * *`            | Daily at 11 PM                        |
| `0 * * * *`             | Every hour                            |
| `0 0 * * *`             | Daily at midnight                     |
| `0 9 * * 1`             | Monday at 9 AM                        |
| `>> file`               | Append output                         |
| `2>&1`                  | Redirect errors to stdout destination |
| `systemctl status cron` | Check cron service                    |
| `journalctl -u cron`    | View cron service logs                |
| `command -v java`       | Find Java executable                  |
| `date`                  | Current date/time                     |
| `tar -czf`              | Create compressed archive             |
| `find ... -mtime`       | Find by modification age              |

---

