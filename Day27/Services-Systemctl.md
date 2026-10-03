# ⚙️ Day 27 — Linux Services & systemctl

## 🎯 Learning Objectives

By the end of Day 27, you will understand:

* What a Linux service is
* What `systemd` does
* What `systemctl` is
* How to start and stop services
* How to restart and reload services
* How to enable services at boot
* How to check service status
* How to view service logs
* How to troubleshoot failed services
* How to create a custom Linux service
* How to run a Java application as a Linux service

---

# 1. What is a Linux Service?

A **service** is a program that runs in the background and provides functionality to the system or other applications.

Examples:

```text
SSH
Web Server
Database Server
Docker
Cron
PostgreSQL
Apache
Nginx
```

A service usually continues running without requiring a terminal window to stay open.

For example:

```text
Java Application
      ↓
Linux Service
      ↓
Runs in Background
      ↓
Restarts if configured
      ↓
Starts automatically after boot
```

---

# 2. What is systemd?

Modern Linux distributions commonly use **systemd** to manage system services.

It is responsible for:

* Starting services
* Stopping services
* Restarting services
* Managing dependencies
* Starting services during boot
* Monitoring service state
* Managing service logs through the journal

You can think of:

```text
systemd = Service Manager
systemctl = Command used to control systemd
```

---

# 3. What is systemctl?

`systemctl` is the main command used to communicate with `systemd`.

Basic syntax:

```bash
systemctl COMMAND SERVICE
```

Example:

```bash
systemctl status ssh
```

---

# 4. Check Service Status

Syntax:

```bash
systemctl status SERVICE
```

Example:

```bash
systemctl status ssh
```

On some systems the service may be:

```bash
systemctl status sshd
```

You may see:

```text
Active: active (running)
```

This means the service is currently running.

---

# 5. Start a Service

Start a service:

```bash
sudo systemctl start ssh
```

Check:

```bash
systemctl status ssh
```

### Important

`start` starts the service **now**.

It does not automatically mean that the service will start after the next reboot.

---

# 6. Stop a Service

Stop:

```bash
sudo systemctl stop ssh
```

Check:

```bash
systemctl status ssh
```

Be careful when stopping important system services.

---

# 7. Restart a Service

Restart:

```bash
sudo systemctl restart ssh
```

This:

```text
Stop
 ↓
Start
```

is commonly used after changing service configuration.

---

# 8. Reload a Service

Some services support configuration reload without completely stopping the service.

```bash
sudo systemctl reload SERVICE
```

Example:

```bash
sudo systemctl reload nginx
```

Difference:

```text
restart → stop + start
reload  → reload configuration
```

Not every service supports `reload`.

---

# 9. Enable a Service

Suppose you want a service to start automatically when Linux boots.

Use:

```bash
sudo systemctl enable SERVICE
```

Example:

```bash
sudo systemctl enable ssh
```

Check:

```bash
systemctl is-enabled ssh
```

Possible output:

```text
enabled
```

---

# 10. Disable a Service

Prevent a service from automatically starting during boot:

```bash
sudo systemctl disable SERVICE
```

Example:

```bash
sudo systemctl disable ssh
```

Check:

```bash
systemctl is-enabled ssh
```

---

# 11. Enable and Start Together

Instead of:

```bash
sudo systemctl enable myservice
sudo systemctl start myservice
```

you can use:

```bash
sudo systemctl enable --now myservice
```

This means:

```text
enable → start automatically at boot
--now  → start it immediately
```

---

# 12. Check if a Service is Running

Use:

```bash
systemctl is-active SERVICE
```

Example:

```bash
systemctl is-active ssh
```

Possible output:

```text
active
```

---

# 13. Check if a Service is Enabled

Use:

```bash
systemctl is-enabled SERVICE
```

Example:

```bash
systemctl is-enabled ssh
```

Possible output:

```text
enabled
```

---

# 14. List Running Services

Use:

```bash
systemctl list-units --type=service
```

This shows currently loaded service units.

You can also use:

```bash
systemctl --type=service
```

---

# 15. List All Installed Service Units

Use:

```bash
systemctl list-unit-files --type=service
```

This shows service unit files and their enabled/disabled state.

Example:

```text
ssh.service       enabled
cron.service      enabled
someapp.service   disabled
```

---

# 16. Search for a Service

You can combine `systemctl` with `grep`.

Example:

```bash
systemctl list-unit-files --type=service | grep ssh
```

Another example:

```bash
systemctl list-units --type=service | grep java
```

---

# 17. View a Service Configuration

Use:

```bash
systemctl cat SERVICE
```

Example:

```bash
systemctl cat ssh
```

This displays the service unit configuration.

---

# 18. View Service Properties

Use:

```bash
systemctl show SERVICE
```

Example:

```bash
systemctl show ssh
```

This can display information such as:

```text
MainPID
ExecStart
User
Group
Restart
WorkingDirectory
Environment
```

---

# 19. Service Lifecycle

A typical service lifecycle looks like:

```text
Installed
   ↓
Configured
   ↓
Started
   ↓
Running
   ↓
Stopped
   ↓
Restarted
```

For boot configuration:

```text
enable
  ↓
Linux boots
  ↓
systemd starts service
```

---

# 20. Service States

Common states include:

```text
active
inactive
failed
activating
deactivating
```

### Active

```text
active (running)
```

The service is running.

### Inactive

```text
inactive (dead)
```

The service is not running.

### Failed

```text
failed
```

The service attempted to start but encountered an error.

---

# 21. View Service Logs

One of the most important troubleshooting commands is:

```bash
journalctl
```

For a specific service:

```bash
journalctl -u SERVICE
```

Example:

```bash
journalctl -u ssh
```

Show recent logs:

```bash
journalctl -u ssh -n 50
```

Follow logs live:

```bash
journalctl -u ssh -f
```

---

# 22. Service Status + Logs

When a service fails, use:

```bash
systemctl status SERVICE
```

Then:

```bash
journalctl -u SERVICE -n 50
```

Example:

```bash
systemctl status my-java-app
journalctl -u my-java-app -n 50
```

This is a very useful troubleshooting workflow.

---

# 23. Service Dependencies

Services may depend on other services.

For example:

```text
Java Application
      ↓
Network
      ↓
Database
```

A service may need networking before it starts.

In a service file you can specify:

```ini
After=network.target
```

This tells systemd to start the service after the network target.

---

# 24. Common systemd Targets

A target groups systemd units together.

One commonly used target is:

```text
multi-user.target
```

It is commonly used for server-style services.

For a custom service you may see:

```ini
[Install]
WantedBy=multi-user.target
```

---

# 25. What is a `.service` File?

A systemd service is normally defined using a file ending in:

```text
.service
```

Example:

```text
my-java-app.service
```

A custom service may be placed under:

```text
/etc/systemd/system/
```

Example:

```text
/etc/systemd/system/my-java-app.service
```

---

# 26. Service File Structure

A service file commonly contains:

```ini
[Unit]

[Service]

[Install]
```

Each section has a purpose.

---

# 27. `[Unit]`

The `[Unit]` section contains general information and dependencies.

Example:

```ini
[Unit]
Description=Java Demo Application
After=network.target
```

---

# 28. `[Service]`

The `[Service]` section tells systemd how to run the application.

Example:

```ini
[Service]
User=<username>
WorkingDirectory=/home/<username>/java-app
ExecStart=/usr/bin/java -jar /home/<username>/java-app/app.jar
Restart=on-failure
RestartSec=5
```

Important:

Replace:

```text
<username>
```

with your actual Linux username.

Also verify the Java path:

```bash
which java
```

---

# 29. `ExecStart`

`ExecStart` tells systemd what command to execute.

Example:

```ini
ExecStart=/usr/bin/java -jar /home/user/java-app/app.jar
```

You should use the actual Java path from:

```bash
which java
```

For example:

```bash
which java
```

might return:

```text
/usr/bin/java
```

---

# 30. `WorkingDirectory`

This defines the directory where the application runs.

Example:

```ini
WorkingDirectory=/home/user/java-app
```

This is useful when your application uses relative paths.

---

# 31. Restart Policy

You can configure automatic restart:

```ini
Restart=on-failure
```

This means systemd can restart the application when it exits because of a failure.

You can also specify:

```ini
RestartSec=5
```

This waits five seconds before attempting a restart.

---

# 32. Run Java Application as a Linux Service

Suppose your application is:

```text
app.jar
```

Directory:

```text
/home/user/java-app/
```

You want:

```text
Java Application
       ↓
systemd
       ↓
Background Service
       ↓
Automatic Restart
```

Create:

```text
/etc/systemd/system/my-java-app.service
```

Example:

```ini
[Unit]
Description=Java Demo Application
After=network.target

[Service]
User=<username>
WorkingDirectory=/home/<username>/java-app
ExecStart=/usr/bin/java -jar /home/<username>/java-app/app.jar
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Important

This is an example template.

You must replace:

```text
<username>
```

and:

```text
/usr/bin/java
```

if your Java installation is somewhere else.

Check Java:

```bash
which java
```

Check the JAR:

```bash
ls -l /home/<username>/java-app/app.jar
```

---

# 33. Create the Service File

Open the file:

```bash
sudo nano /etc/systemd/system/my-java-app.service
```

Paste your service configuration.

Save in nano:

```text
Ctrl + O
Enter
Ctrl + X
```

---

# 34. Reload systemd

Whenever you create or modify a service file, run:

```bash
sudo systemctl daemon-reload
```

This tells systemd to reload its unit configuration.

---

# 35. Start the Java Service

Run:

```bash
sudo systemctl start my-java-app
```

Check:

```bash
systemctl status my-java-app
```

---

# 36. Enable Java Service at Boot

Run:

```bash
sudo systemctl enable my-java-app
```

Or:

```bash
sudo systemctl enable --now my-java-app
```

Now the service will be configured to start automatically when the system boots.

---

# 37. View Java Service Logs

Show recent logs:

```bash
journalctl -u my-java-app -n 50
```

Follow logs:

```bash
journalctl -u my-java-app -f
```

Press:

```text
Ctrl + C
```

to stop following the logs.

---

# 38. Restart Java Service

After updating the JAR:

```bash
sudo systemctl restart my-java-app
```

Then:

```bash
systemctl status my-java-app
```

---

# 39. Stop Java Service

```bash
sudo systemctl stop my-java-app
```

Check:

```bash
systemctl is-active my-java-app
```

---

# 40. Disable Java Service

To stop automatic startup:

```bash
sudo systemctl disable my-java-app
```

To disable and stop immediately:

```bash
sudo systemctl disable --now my-java-app
```

---

# 41. Troubleshooting Failed Services

Suppose:

```bash
systemctl status my-java-app
```

shows:

```text
failed
```

Don't immediately recreate everything.

Follow this process.

### Step 1 — Check status

```bash
systemctl status my-java-app
```

### Step 2 — Check logs

```bash
journalctl -u my-java-app -n 50
```

### Step 3 — Check Java

```bash
which java
java -version
```

### Step 4 — Check JAR

```bash
ls -l /home/<username>/java-app/app.jar
```

### Step 5 — Check service file

```bash
systemctl cat my-java-app
```

### Step 6 — Reload systemd

```bash
sudo systemctl daemon-reload
```

### Step 7 — Restart

```bash
sudo systemctl restart my-java-app
```

### Step 8 — Check again

```bash
systemctl status my-java-app
```

---

# 42. Common Java Service Problems

## Problem 1 — Wrong Java path

Service:

```ini
ExecStart=/wrong/path/java -jar app.jar
```

Check:

```bash
which java
```

---

## Problem 2 — Wrong JAR path

Check:

```bash
ls -l /home/user/java-app/app.jar
```

---

## Problem 3 — Wrong user

If the service uses:

```ini
User=user1
```

make sure that user can access the application directory and JAR.

---

## Problem 4 — Permission denied

Check:

```bash
ls -ld /home/user/java-app
ls -l /home/user/java-app/app.jar
```

---

## Problem 5 — Changed service file but did not reload systemd

Run:

```bash
sudo systemctl daemon-reload
```

Then:

```bash
sudo systemctl restart my-java-app
```

---

# 43. Useful systemctl Commands

| Command                           | Purpose              |
| --------------------------------- | -------------------- |
| `systemctl status service`        | Check status         |
| `systemctl start service`         | Start                |
| `systemctl stop service`          | Stop                 |
| `systemctl restart service`       | Restart              |
| `systemctl reload service`        | Reload configuration |
| `systemctl enable service`        | Start at boot        |
| `systemctl disable service`       | Disable boot startup |
| `systemctl enable --now service`  | Enable + start       |
| `systemctl disable --now service` | Disable + stop       |
| `systemctl is-active service`     | Check active state   |
| `systemctl is-enabled service`    | Check boot state     |
| `systemctl cat service`           | View service file    |
| `systemctl show service`          | Show properties      |
| `systemctl daemon-reload`         | Reload unit files    |

---

# 44. Useful journalctl Commands

```bash
journalctl
```

All journal logs.

```bash
journalctl -n 50
```

Last 50 entries.

```bash
journalctl -f
```

Follow logs.

```bash
journalctl -u my-java-app
```

Logs for a service.

```bash
journalctl -u my-java-app -n 50
```

Last 50 service logs.

```bash
journalctl -u my-java-app -f
```

Follow Java service logs.

---

# 45. Practice Lab

Create a simple service-learning checklist.

### Step 1

Check a service:

```bash
systemctl status cron
```

On some distributions:

```bash
systemctl status crond
```

### Step 2

Check whether it is active:

```bash
systemctl is-active cron
```

### Step 3

Check whether it is enabled:

```bash
systemctl is-enabled cron
```

### Step 4

View logs:

```bash
journalctl -u cron -n 20
```

If your system uses `crond`:

```bash
journalctl -u crond -n 20
```

### Step 5

List services:

```bash
systemctl list-units --type=service
```

### Step 6

Search:

```bash
systemctl list-units --type=service | grep -E 'ssh|cron'
```

---

# 46. Mini Project — Java Application Service

## Goal

Run a Java JAR as a Linux service.

### Project structure

```text
Day27-Project/
├── app.jar
└── my-java-app.service
```

### Requirements

Your service should:

* Start the Java application
* Run in the background
* Restart after failure
* Start automatically after boot
* Write logs to the system journal
* Be manageable using `systemctl`

### Expected commands

```bash
sudo systemctl daemon-reload
sudo systemctl start my-java-app
systemctl status my-java-app
journalctl -u my-java-app -n 50
sudo systemctl enable my-java-app
```

---

# 47. Java Developer Connection

As a Java developer, you may deploy:

```text
Spring Boot Application
        ↓
JAR file
        ↓
Linux Server
        ↓
systemd service
        ↓
Application running
```

For example:

```text
student-management.jar
```

could run as:

```text
student-management.service
```

Then an administrator can use:

```bash
systemctl status student-management
```

instead of manually running:

```bash
java -jar student-management.jar
```

This is a common server-side deployment concept.

---

# 48. Service vs Normal Program

### Normal program

```bash
java -jar app.jar
```

The program is started from your terminal.

### Service

```bash
sudo systemctl start my-java-app
```

systemd manages the application.

It can provide:

```text
Start
Stop
Restart
Boot startup
Failure restart
Logging
Status
```

---

# 49. Important Commands to Remember

```bash
systemctl status SERVICE
```

```bash
sudo systemctl start SERVICE
```

```bash
sudo systemctl stop SERVICE
```

```bash
sudo systemctl restart SERVICE
```

```bash
sudo systemctl enable SERVICE
```

```bash
sudo systemctl disable SERVICE
```

```bash
systemctl is-active SERVICE
```

```bash
systemctl is-enabled SERVICE
```

```bash
systemctl list-units --type=service
```

```bash
journalctl -u SERVICE
```

```bash
sudo systemctl daemon-reload
```

---

# 50. Common Mistakes

### Mistake 1

Using:

```bash
service start ssh
```

instead of:

```bash
systemctl start ssh
```

### Mistake 2

Changing a `.service` file but forgetting:

```bash
sudo systemctl daemon-reload
```

### Mistake 3

Using a relative path in `ExecStart`.

Prefer an absolute path:

```ini
ExecStart=/usr/bin/java -jar /home/user/app/app.jar
```

### Mistake 4

Not checking logs.

Always use:

```bash
journalctl -u SERVICE
```

when troubleshooting.

### Mistake 5

Assuming every distribution has exactly the same service name.

For example:

```text
ssh
```

or:

```text
sshd
```

may differ by distribution.

---

# 51. Interview Questions

### Q1. What is a Linux service?

A service is a background program managed by the operating system that provides a specific function.

---

### Q2. What is systemd?

`systemd` is a system and service manager commonly used by modern Linux distributions.

---

### Q3. What is systemctl?

`systemctl` is the command-line utility used to manage systemd services and units.

---

### Q4. How do you check a service status?

```bash
systemctl status SERVICE
```

---

### Q5. How do you start a service?

```bash
sudo systemctl start SERVICE
```

---

### Q6. How do you stop a service?

```bash
sudo systemctl stop SERVICE
```

---

### Q7. Difference between start and enable?

```text
start  → starts the service now
enable → configures it to start during boot
```

---

### Q8. How do you restart a service?

```bash
sudo systemctl restart SERVICE
```

---

### Q9. What is daemon-reload?

```bash
sudo systemctl daemon-reload
```

It tells systemd to reload service/unit configuration after unit files have been created or changed.

---

### Q10. How do you view logs of a service?

```bash
journalctl -u SERVICE
```

---

### Q11. How can you run a Java application as a service?

Create a `.service` unit containing an `ExecStart` command such as:

```ini
ExecStart=/usr/bin/java -jar /path/to/app.jar
```

Then reload systemd and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl start my-java-app
```

---

### Q12. How do you configure automatic restart?

Example:

```ini
Restart=on-failure
RestartSec=5
```

---

# 🧠 Day 27 Cheat Sheet

```text
systemd
   ↓
Service Manager
   ↓
systemctl
```

### Service control

```bash
systemctl status SERVICE
systemctl start SERVICE
systemctl stop SERVICE
systemctl restart SERVICE
systemctl reload SERVICE
```

### Boot configuration

```bash
systemctl enable SERVICE
systemctl disable SERVICE
systemctl enable --now SERVICE
```

### Checking

```bash
systemctl is-active SERVICE
systemctl is-enabled SERVICE
```

### Listing

```bash
systemctl list-units --type=service
systemctl list-unit-files --type=service
```

### Logs

```bash
journalctl -u SERVICE
journalctl -u SERVICE -n 50
journalctl -u SERVICE -f
```

### Custom service

```text
/etc/systemd/system/my-java-app.service
```

### After changing service file

```bash
sudo systemctl daemon-reload
```

---

