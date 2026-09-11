# 🐧 Day 12 — Linux Processes

> **30 Days of Linux — Java Meets Linux**

Today we learn how Linux manages **running programs** using processes.

---

## 🎯 Today's Goals

By the end of Day 12, you should understand:

* What is a process?
* Program vs Process
* PID and PPID
* Process hierarchy
* Process states
* Foreground vs Background processes
* `ps`
* `top`
* `htop`
* `pgrep`
* `pstree`
* `/proc`
* CPU and memory usage
* Finding Java processes
* Basic process monitoring

---

# 1. What Is a Process?

A **process** is a program that is currently running.

For example:

```bash
java MyApplication
```

The Java program stored on disk is a **program**.

When you execute it:

```bash
java MyApplication
```

Linux creates a **process**.

### Simple idea

```text
Program
   ↓
Execute
   ↓
Process
   ↓
CPU + Memory + Resources
```

---

# 2. Program vs Process

| Program             | Process                     |
| ------------------- | --------------------------- |
| Stored on disk      | Running in memory           |
| Passive             | Active                      |
| Does not have a PID | Has a PID                   |
| Example: `app.jar`  | Running `java -jar app.jar` |

Example:

```bash
myapp.jar
```

is a file.

But:

```bash
java -jar myapp.jar
```

creates a running process.

---

# 3. What Is a PID?

PID means:

> **Process ID**

Every running process gets a unique process ID.

Example:

```text
PID
2451
```

You can find your current shell's PID using:

```bash
echo $$
```

Example:

```text
2451
```

Here:

```text
2451 = PID of the current shell
```

---

# 4. PPID

PPID means:

> **Parent Process ID**

Processes can create other processes.

Example:

```text
Terminal
   |
   └── Bash
        |
        └── Java
```

The Java process has a parent process.

You can see PID and PPID using:

```bash
ps -ef
```

---

# 5. Process Hierarchy

Linux processes form a tree.

At the top is usually:

```text
systemd
   |
   ├── services
   ├── ssh
   ├── cron
   ├── shells
   └── applications
```

You can view the process tree using:

```bash
pstree
```

For PIDs:

```bash
pstree -p
```

Example:

```text
systemd(1)
 ├─sshd(800)
 │   └─bash(1200)
 │       └─java(1500)
 └─cron(900)
```

---

# 6. PID 1

The first userspace process normally has:

```text
PID = 1
```

On many modern Linux distributions, this is:

```text
systemd
```

Check it:

```bash
ps -p 1
```

Or:

```bash
ps -p 1 -f
```

---

# 7. View Running Processes

The most important command:

```bash
ps
```

Example:

```text
PID TTY          TIME CMD
1234 pts/0    00:00:00 bash
```

By default, `ps` shows processes associated with your current terminal/session.

---

# 8. `ps -ef`

One of the most commonly used commands:

```bash
ps -ef
```

Example:

```text
UID     PID   PPID  C STIME TTY      TIME CMD
user   1200   1100  0 10:20 pts/0    00:00 bash
user   1500   1200  1 10:21 pts/0    00:05 java App
```

Important columns:

| Column | Meaning                   |
| ------ | ------------------------- |
| UID    | User                      |
| PID    | Process ID                |
| PPID   | Parent Process ID         |
| C      | CPU utilization indicator |
| STIME  | Start time                |
| TTY    | Terminal                  |
| TIME   | CPU time                  |
| CMD    | Command                   |

---

# 9. `ps aux`

Another extremely important command:

```bash
ps aux
```

It gives detailed process information.

Example:

```text
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
```

Important columns:

### USER

User who owns the process.

### PID

Process ID.

### %CPU

CPU usage.

### %MEM

Memory usage.

### STAT

Process state.

### COMMAND

Command used to start the process.

---

# 10. `ps -ef` vs `ps aux`

Both are useful.

```bash
ps -ef
```

is commonly used when you want:

* PID
* PPID
* full command
* process hierarchy information

```bash
ps aux
```

is commonly used when you want:

* CPU usage
* memory usage
* user
* process state

---

# 11. Find a Specific Process

Suppose you want to find Java processes.

Use:

```bash
ps aux | grep java
```

Better:

```bash
pgrep -a java
```

Example:

```text
1500 java -jar campus-erp.jar
1650 java MyApplication
```

---

# 12. `pgrep`

`pgrep` searches processes by name.

Example:

```bash
pgrep java
```

Output:

```text
1500
1650
```

To show PID and command:

```bash
pgrep -a java
```

---

# 13. Find a Process by PID

Suppose PID is:

```text
1500
```

Run:

```bash
ps -p 1500
```

Detailed:

```bash
ps -p 1500 -f
```

---

# 14. Process Information

You can use:

```bash
ps -p 1500 -o pid,ppid,user,%cpu,%mem,stat,cmd
```

This gives a clean view:

```text
PID  PPID USER %CPU %MEM STAT CMD
1500 1200 user 2.0 5.1 S java -jar app.jar
```

This is very useful when troubleshooting applications.

---

# 15. Process States

Processes can have different states.

Common states include:

| State | Meaning               |
| ----- | --------------------- |
| R     | Running/Runnable      |
| S     | Sleeping              |
| D     | Uninterruptible sleep |
| T     | Stopped               |
| Z     | Zombie                |
| I     | Idle kernel thread    |

---

# 16. Running — `R`

`R` means the process is:

> Running or ready to run.

Example:

```text
R
```

The process is actively executing or waiting for CPU time.

---

# 17. Sleeping — `S`

`S` means:

> Interruptible sleep.

Many processes spend most of their time sleeping while waiting for something.

For example:

```bash
sleep 100
```

The process is not continuously consuming CPU.

---

# 18. Uninterruptible Sleep — `D`

`D` usually means the process is waiting for certain kernel/I/O operations.

For example:

```text
D
```

A process stuck in `D` state can sometimes indicate an I/O problem.

---

# 19. Stopped — `T`

A process can be stopped.

For example, terminal job control can stop a process.

We will study this more deeply on:

> **Day 13 — Process Control**

---

# 20. Zombie Process

A zombie is a process that has finished execution but still has an entry in the process table because its parent has not yet collected its exit status.

Example:

```text
Parent
  |
  └── Zombie
```

Zombie processes have:

```text
STAT = Z
```

Find them:

```bash
ps aux | awk '$8 ~ /^Z/'
```

---

# 21. Foreground Process

When you run:

```bash
sleep 30
```

the terminal waits for the command to finish.

You cannot normally use that terminal for another command until it completes.

This is a:

> Foreground process.

---

# 22. Background Process

You can start a command in the background using:

```bash
sleep 30 &
```

The `&` tells the shell to start the command as a background job.

Example:

```text
[1] 2500
```

Here:

```text
2500 = PID
```

You can continue using the terminal.

---

# 23. `jobs`

View background jobs:

```bash
jobs
```

Example:

```text
[1]+  Running    sleep 30 &
```

---

# 24. `bg` and `fg`

These commands are related to process control.

```bash
bg
```

continues a stopped job in the background.

```bash
fg
```

brings a background job to the foreground.

We will study these properly in **Day 13**.

---

# 25. `top`

`top` is one of the most important Linux monitoring commands.

Run:

```bash
top
```

It continuously displays running processes.

You can see:

* CPU usage
* Memory usage
* PID
* User
* Process state
* Running processes

Example:

```text
PID USER  PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
1500 user 20  0  ... ... ... S  2.5  4.2  java
```

---

# 26. Understanding `top`

Important columns:

### PID

Process ID.

### USER

Process owner.

### PR

Priority.

### NI

Nice value.

### VIRT

Virtual memory.

### RES

Resident memory.

### SHR

Shared memory.

### S

Process state.

### %CPU

CPU usage.

### %MEM

Memory usage.

### COMMAND

Process/application name.

---

# 27. Exit `top`

Press:

```text
q
```

to exit.

---

# 28. `htop`

`htop` is an interactive alternative to `top`.

Run:

```bash
htop
```

If installed, it provides an easier interface for process monitoring.

You may need to install it depending on your Linux distribution.

Example:

```bash
sudo apt install htop
```

Do not install packages unnecessarily on production systems.

---

# 29. Process Monitoring with `watch`

You can repeatedly run a command.

Example:

```bash
watch ps aux
```

This refreshes the output periodically.

Another useful example:

```bash
watch "pgrep -a java"
```

This is useful for watching Java applications.

---

# 30. The `/proc` Filesystem

Linux provides process information through:

```text
/proc
```

`/proc` is a virtual filesystem.

It contains information about:

* processes
* CPU
* memory
* kernel
* system configuration

Check it:

```bash
ls /proc
```

You will see many numbers:

```text
1
2
10
500
1200
1500
...
```

Numeric directories usually represent process IDs.

---

# 31. `/proc/PID`

Suppose Java process PID is:

```text
1500
```

Check:

```bash
ls /proc/1500
```

You can inspect information about that process.

---

# 32. `/proc/PID/status`

Run:

```bash
cat /proc/1500/status
```

You can find information such as:

* Name
* State
* PID
* PPID
* UID
* GID
* memory information
* thread information

---

# 33. `/proc/PID/cmdline`

To see how a process was started:

```bash
cat /proc/1500/cmdline
```

For example:

```text
java-jarapp.jar
```

The arguments may appear without normal spaces because `/proc` represents arguments internally.

---

# 34. `/proc/PID/exe`

You can inspect the executable:

```bash
ls -l /proc/1500/exe
```

This can show what executable the process is running.

---

# 35. Java Developer Example

Suppose your application is:

```text
smart-campus.jar
```

Start it:

```bash
java -jar smart-campus.jar
```

Linux creates a process.

Find it:

```bash
pgrep -a java
```

Example:

```text
2450 java -jar smart-campus.jar
```

Now inspect it:

```bash
ps -p 2450 -f
```

Check CPU and memory:

```bash
ps -p 2450 -o pid,ppid,user,%cpu,%mem,stat,cmd
```

Check detailed information:

```bash
cat /proc/2450/status
```

This is a practical Linux skill for Java developers.

---

# 36. Find High CPU Processes

Use:

```bash
ps aux --sort=-%cpu | head
```

This displays processes using the most CPU.

---

# 37. Find High Memory Processes

Use:

```bash
ps aux --sort=-%mem | head
```

This displays processes using the most memory.

---

# 38. Count Running Processes

You can use:

```bash
ps -e | wc -l
```

This counts process entries.

---

# 39. Search Processes with Pipes

Day 7 becomes useful here.

Example:

```bash
ps aux | grep java
```

Another:

```bash
ps aux | grep nginx
```

Or:

```bash
ps -ef | grep ssh
```

Remember that:

```bash
|
```

passes output from one command to another.

---

# 40. Avoid the `grep` Self-Match

When you run:

```bash
ps aux | grep java
```

you may sometimes see the `grep java` command itself.

A common alternative is:

```bash
pgrep -a java
```

This is cleaner.

---

# 41. Process Tree

Run:

```bash
pstree
```

With PIDs:

```bash
pstree -p
```

For a specific user:

```bash
pstree username
```

Example structure:

```text
systemd
 ├── sshd
 │    └── bash
 │         └── java
 ├── cron
 └── other-services
```

---

# 42. Mini Practice Lab

Create a test process:

```bash
sleep 300 &
```

You should get something similar to:

```text
[1] 3000
```

Your PID will be different.

Find it:

```bash
pgrep sleep
```

Check it:

```bash
ps -p <PID> -f
```

Replace `<PID>` with the actual PID.

Example:

```bash
ps -p 3000 -f
```

---

# 43. Inspect the Process

Run:

```bash
cat /proc/<PID>/status
```

Example:

```bash
cat /proc/3000/status
```

Look for:

```text
Name:
State:
Pid:
PPid:
Uid:
Gid:
VmSize:
VmRSS:
Threads:
```

---

# 44. View the Process Tree

Run:

```bash
pstree -p
```

Find your `sleep` process.

You should see it somewhere under your shell.

---

# 45. Monitor with `top`

Run:

```bash
top
```

Find the PID of your process.

Press:

```text
q
```

to exit.

---

# 46. Check Process State

Run:

```bash
ps -p <PID> -o pid,ppid,stat,cmd
```

You may see:

```text
PID  PPID STAT CMD
3000 2500 S    sleep 300
```

Here:

```text
S = Sleeping
```

---

# 47. Java Process Monitoring Lab

If Java is installed, check:

```bash
java -version
```

Start a simple Java application or JAR.

For example:

```bash
java -jar app.jar &
```

Find it:

```bash
pgrep -a java
```

Monitor:

```bash
ps aux | grep java
```

Detailed:

```bash
ps -ef | grep java
```

CPU/memory:

```bash
ps -p <PID> -o pid,ppid,%cpu,%mem,stat,cmd
```

---

# 48. Real-World Java Troubleshooting

Suppose a Java application is slow.

First find the process:

```bash
pgrep -a java
```

Check CPU:

```bash
ps aux --sort=-%cpu | head
```

Check memory:

```bash
ps aux --sort=-%mem | head
```

Inspect the Java process:

```bash
ps -p <PID> -f
```

Then monitor it:

```bash
top
```

This gives you an initial picture of whether the application is consuming unusual CPU or memory.

---

# 49. Process vs Thread

A **process** is an independent running program.

A process can contain multiple **threads**.

Example:

```text
Java Process
     |
     ├── Main Thread
     ├── Worker Thread
     ├── HTTP Thread
     └── Database Thread
```

You can see threads using:

```bash
ps -T -p <PID>
```

Threads will be covered more deeply when we connect Linux concepts with Java.

---

# 50. Important Commands

| Command  | Purpose                        |
| -------- | ------------------------------ |
| `ps`     | Show processes                 |
| `ps -ef` | Detailed process list          |
| `ps aux` | Detailed resource/process view |
| `pgrep`  | Find process by name           |
| `pstree` | Process hierarchy              |
| `top`    | Live process monitoring        |
| `htop`   | Interactive monitoring         |
| `jobs`   | Shell background jobs          |
| `bg`     | Continue job in background     |
| `fg`     | Bring job to foreground        |
| `sleep`  | Create test process            |
| `/proc`  | Process/system information     |
| `watch`  | Repeatedly run command         |

---

# 51. Important Difference

### `ps`

Snapshot:

```text
ps
```

Shows information at that moment.

### `top`

Live monitoring:

```text
top
```

Continuously updates process information.

---

# 52. Practice Challenge 🚀

Complete these without looking at the answers.

### Challenge 1

Show all processes.

```text
?
```

### Challenge 2

Show processes with CPU and memory usage.

```text
?
```

### Challenge 3

Find Java processes.

```text
?
```

### Challenge 4

Show a process tree with PIDs.

```text
?
```

### Challenge 5

Start a background test process.

```text
?
```

### Challenge 6

Find the PID of `sleep`.

```text
?
```

### Challenge 7

Display detailed information about a PID.

```text
?
```

### Challenge 8

Display process information from `/proc`.

```text
?
```

### Challenge 9

Find the processes using the most CPU.

```text
?
```

### Challenge 10

Find the processes using the most memory.

```text
?
```

---

# 53. Challenge Answers

### 1

```bash
ps -ef
```

### 2

```bash
ps aux
```

### 3

```bash
pgrep -a java
```

### 4

```bash
pstree -p
```

### 5

```bash
sleep 300 &
```

### 6

```bash
pgrep sleep
```

### 7

```bash
ps -p <PID> -f
```

### 8

```bash
cat /proc/<PID>/status
```

### 9

```bash
ps aux --sort=-%cpu | head
```

### 10

```bash
ps aux --sort=-%mem | head
```

---

# 54. Interview Questions 🎯

### Q1. What is a process?

A process is a running instance of a program.

---

### Q2. What is PID?

PID stands for Process ID. Linux assigns a unique process ID to a process.

---

### Q3. What is PPID?

PPID stands for Parent Process ID. It identifies the process that created the current process.

---

### Q4. What is the difference between a program and a process?

A program is a passive file stored on disk, while a process is a running instance of that program.

---

### Q5. What does `ps` do?

`ps` displays information about running processes.

---

### Q6. Difference between `ps -ef` and `ps aux`?

Both display process information, but they use different output formats and emphasize different information. `ps aux` is especially useful for viewing CPU/memory usage, while `ps -ef` clearly shows PID, PPID and full command information.

---

### Q7. What is `top`?

`top` is a real-time process monitoring tool that displays CPU, memory and process information.

---

### Q8. What is a zombie process?

A zombie is a terminated process whose parent has not yet collected its exit status.

---

### Q9. What is `/proc`?

`/proc` is a virtual filesystem that exposes information about processes, the kernel and system resources.

---

### Q10. How do you find Java processes?

```bash
pgrep -a java
```

or:

```bash
ps aux | grep java
```

---

### Q11. How do you find a process using high CPU?

```bash
ps aux --sort=-%cpu | head
```

---

### Q12. How do you find a process using high memory?

```bash
ps aux --sort=-%mem | head
```

---

# 55. Quick Revision

Remember this flow:

```text
Program
   ↓
Execution
   ↓
Process
   ↓
PID
   ↓
CPU + Memory
   ↓
Monitoring
   ↓
ps / top / htop
```

Process hierarchy:

```text
Parent
  |
  └── Child
       |
       └── Another Child
```

Useful commands:

```bash
ps
ps -ef
ps aux
pgrep
pstree
top
htop
jobs
bg
fg
```

---

# 🧠 Day 12 Key Takeaways

You should now understand:

* A running program is a process.
* Every process has a PID.
* Processes have parent processes identified by PPID.
* Linux organizes processes as a tree.
* Processes have different states.
* `ps` gives a process snapshot.
* `top` provides live monitoring.
* `pgrep` helps find processes.
* `pstree` displays process hierarchy.
* `/proc` provides detailed process information.
* Java applications also run as Linux processes.
* CPU and memory usage can be monitored from Linux.



