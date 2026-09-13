# 🐧 Day 13 — Linux Process Control

> **30 Days of Linux — Java Meets Linux**

Today we learn how to **control running processes** in Linux.

---

## 🎯 Today's Goals

By the end of Day 13, you should understand:

* Foreground and background processes
* Job control
* `&`
* `jobs`
* `fg`
* `bg`
* `Ctrl + C`
* `Ctrl + Z`
* `kill`
* Linux signals
* `kill -9`
* `pkill`
* `killall`
* Process priority
* `nice`
* `renice`
* Graceful vs forceful termination
* Java process control

---

# 1. What Is Process Control?

Process control means managing running processes.

You may need to:

* Start a process
* Stop a process
* Pause a process
* Resume a process
* Move a process to background
* Bring a process to foreground
* Change its priority
* Terminate an application

Example:

```text
Start
  ↓
Running
  ↓
Pause
  ↓
Resume
  ↓
Terminate
```

---

# 2. Foreground Process

Run:

```bash
sleep 30
```

The command runs in the foreground.

Your terminal waits until it finishes.

You cannot normally use that terminal for another command while it is running.

---

# 3. Background Process

Add `&`:

```bash
sleep 30 &
```

Example:

```text
[1] 2450
```

Here:

```text
1    = job number
2450 = process ID
```

The terminal becomes available immediately.

---

# 4. The `&` Operator

The `&` operator tells the shell:

> Start this command as a background job.

Example:

```bash
java -jar app.jar &
```

This is useful for applications that should continue running while you use the terminal.

---

# 5. View Background Jobs

Use:

```bash
jobs
```

Example:

```text
[1]+  Running    sleep 300 &
[2]-  Running    java -jar app.jar &
```

The numbers are **job IDs**, not PIDs.

---

# 6. Job ID vs PID

This distinction is important.

Example:

```text
[1] 2500
```

Here:

```text
1    = Job ID
2500 = PID
```

Job IDs are managed by the shell.

PIDs are assigned by Linux to processes.

---

# 7. Bring a Job to Foreground

Use:

```bash
fg
```

Example:

```bash
fg
```

This brings the most recent suitable background/stopped job to the foreground.

You can also specify a job:

```bash
fg %1
```

Here:

```text
%1 = Job number 1
```

---

# 8. Send a Job to Background

Suppose a process is stopped.

Use:

```bash
bg
```

It continues running in the background.

For a specific job:

```bash
bg %1
```

---

# 9. `Ctrl + C`

Press:

```text
Ctrl + C
```

This normally sends:

```text
SIGINT
```

to the foreground process.

It requests the process to stop.

Example:

```bash
sleep 300
```

Press:

```text
Ctrl + C
```

The command usually terminates.

---

# 10. `Ctrl + Z`

Press:

```text
Ctrl + Z
```

This normally sends:

```text
SIGTSTP
```

to the foreground process.

It suspends the process.

Example:

```bash
sleep 300
```

Press:

```text
Ctrl + Z
```

You may see:

```text
^Z
[1]+  Stopped    sleep 300
```

The process has been stopped, not terminated.

---

# 11. Resume a Stopped Job

After:

```text
Ctrl + Z
```

you can run:

```bash
bg
```

The process continues in the background.

Or:

```bash
fg
```

to continue it in the foreground.

---

# 12. Process Control Flow

A common workflow:

```text
Foreground
    |
    | Ctrl + Z
    ↓
Stopped
    |
    | bg
    ↓
Background
    |
    | fg
    ↓
Foreground
```

---

# 13. Practice Job Control

Run:

```bash
sleep 300
```

Press:

```text
Ctrl + Z
```

Check:

```bash
jobs
```

You should see something like:

```text
[1]+  Stopped    sleep 300
```

Resume in background:

```bash
bg %1
```

Check:

```bash
jobs
```

Bring it back:

```bash
fg %1
```

Then press:

```text
Ctrl + C
```

---

# 14. What Is a Signal?

A signal is a notification sent to a process.

Linux uses signals to communicate with processes.

Examples:

```text
SIGTERM
SIGKILL
SIGINT
SIGSTOP
SIGCONT
SIGHUP
```

Signals can request:

* termination
* stopping
* continuing
* interruption
* reloading/reconfiguration

---

# 15. View Common Signals

Run:

```bash
kill -l
```

You will see the available signals.

Common ones include:

| Signal  | Number | Purpose             |
| ------- | -----: | ------------------- |
| SIGHUP  |      1 | Hangup              |
| SIGINT  |      2 | Interrupt           |
| SIGQUIT |      3 | Quit                |
| SIGKILL |      9 | Force termination   |
| SIGTERM |     15 | Termination request |
| SIGSTOP |     19 | Stop                |
| SIGCONT |     18 | Continue            |

Signal numbers can vary on some architectures, so names are generally clearer.

---

# 16. `kill`

Despite its name, `kill` does not always mean forcefully terminate.

It means:

> Send a signal to a process.

Basic syntax:

```bash
kill PID
```

By default, this sends:

```text
SIGTERM
```

---

# 17. Graceful Termination

Suppose a Java application has:

```text
PID = 2500
```

Run:

```bash
kill 2500
```

This sends `SIGTERM`.

The application gets an opportunity to shut down cleanly.

This is preferred over immediately using `SIGKILL`.

---

# 18. SIGTERM

`SIGTERM` means:

> Please terminate gracefully.

Command:

```bash
kill -TERM 2500
```

Equivalent commonly:

```bash
kill 2500
```

Applications can sometimes handle `SIGTERM` and perform cleanup.

For example:

```text
Save data
   ↓
Close connections
   ↓
Write logs
   ↓
Exit
```

---

# 19. SIGKILL

`SIGKILL` is signal 9.

```bash
kill -9 2500
```

It requests immediate termination by the kernel.

The process cannot catch or ignore `SIGKILL`.

Use it carefully.

### Preferred order

```text
SIGTERM
   ↓
Wait
   ↓
SIGKILL if necessary
```

Do not make `kill -9` your first choice.

---

# 20. SIGSTOP

`SIGSTOP` pauses a process.

Example:

```bash
kill -STOP 2500
```

The process stops.

It does not terminate.

---

# 21. SIGCONT

Resume a stopped process:

```bash
kill -CONT 2500
```

The process continues.

Flow:

```text
Running
   |
   | SIGSTOP
   ↓
Stopped
   |
   | SIGCONT
   ↓
Running
```

---

# 22. `kill -0`

A special technique:

```bash
kill -0 2500
```

This does not actually terminate the process.

It can be used to check whether the process exists and whether you have permission to signal it.

For example:

```bash
if kill -0 2500 2>/dev/null; then
    echo "Process exists"
else
    echo "Process not available"
fi
```

---

# 23. `pkill`

`pkill` can send a signal based on process name or other matching criteria.

Example:

```bash
pkill java
```

This can affect **multiple Java processes**.

⚠️ Be careful.

Before using it, inspect the processes:

```bash
pgrep -a java
```

Do not blindly terminate production applications.

---

# 24. `pkill -TERM`

Graceful termination:

```bash
pkill -TERM java
```

Again, this may affect multiple matching processes.

---

# 25. `killall`

`killall` can terminate processes by name.

Example:

```bash
killall sleep
```

This can affect every matching process.

Always understand what will be matched before running it.

---

# 26. `kill` vs `pkill` vs `killall`

| Command        | Usually targets              |
| -------------- | ---------------------------- |
| `kill PID`     | Specific PID                 |
| `pkill name`   | Matching processes           |
| `killall name` | Processes with matching name |

For safety, targeting a specific PID is often clearer.

---

# 27. Process Priority

Linux processes have priorities.

The `nice` value influences scheduling priority.

You can see it with:

```bash
ps -o pid,ni,pri,cmd
```

Example:

```text
PID   NI   PRI   CMD
2500   0   20   java
```

---

# 28. Nice Value

The normal nice value is:

```text
0
```

Nice values commonly range from:

```text
-20 to 19
```

Generally:

```text
Lower nice value
    ↓
Higher scheduling priority

Higher nice value
    ↓
Lower scheduling priority
```

A process with a higher nice value is being more "nice" to other processes.

---

# 29. Start a Process with `nice`

Example:

```bash
nice -n 10 sleep 300 &
```

The process starts with a nice value of approximately:

```text
10
```

Check:

```bash
ps -o pid,ni,pri,cmd -C sleep
```

---

# 30. `renice`

`renice` changes the nice value of an existing process.

Example:

```bash
renice 10 -p 2500
```

This changes the nice value of PID `2500`.

Check:

```bash
ps -p 2500 -o pid,ni,pri,cmd
```

---

# 31. Permissions and Priority

Changing nice values to give a process **higher priority** may require elevated privileges.

For example, moving toward a negative nice value often requires appropriate privileges.

Do not use `sudo` unless necessary.

---

# 32. Java Process Control

Suppose you start:

```bash
java -jar smart-campus.jar &
```

Find the process:

```bash
pgrep -a java
```

Example:

```text
2500 java -jar smart-campus.jar
```

Inspect it:

```bash
ps -p 2500 -f
```

Gracefully terminate:

```bash
kill 2500
```

Check whether it still exists:

```bash
pgrep -a java
```

If the application refuses to terminate and you have confirmed it is safe to force it:

```bash
kill -9 2500
```

---

# 33. Java Application Shutdown

A well-designed Java application should respond appropriately to termination.

For example, Java applications can use shutdown hooks:

```java
Runtime.getRuntime().addShutdownHook(
    new Thread(() -> {
        System.out.println("Application shutting down...");
    })
);
```

This allows cleanup work during normal JVM shutdown.

However, `SIGKILL` cannot be handled by the application.

---

# 34. Graceful vs Forceful Shutdown

### Graceful

```bash
kill PID
```

Usually sends:

```text
SIGTERM
```

Application gets a chance to clean up.

### Forceful

```bash
kill -9 PID
```

Sends:

```text
SIGKILL
```

The kernel immediately terminates the process.

Think:

```text
SIGTERM = "Please stop."
SIGKILL = "Stop immediately."
```

---

# 35. Background Java Application

Run:

```bash
java -jar app.jar &
```

Check jobs:

```bash
jobs
```

Find PID:

```bash
pgrep -a java
```

Monitor:

```bash
top
```

Stop gracefully:

```bash
kill <PID>
```

Verify:

```bash
pgrep -a java
```

---

# 36. Process Control Using `ps`

Find a process:

```bash
ps aux | grep java
```

Then:

```bash
ps -p <PID> -f
```

Check state:

```bash
ps -p <PID> -o pid,ppid,stat,cmd
```

Check priority:

```bash
ps -p <PID> -o pid,ni,pri,cmd
```

---

# 37. Process Control Using `/proc`

For PID `2500`:

```bash
cat /proc/2500/status
```

Check command:

```bash
cat /proc/2500/cmdline
```

Check executable:

```bash
ls -l /proc/2500/exe
```

These are useful for troubleshooting.

---

# 38. Practice Lab 🚀

## Step 1 — Start a Process

```bash
sleep 300 &
```

---

## Step 2 — Check Jobs

```bash
jobs
```

---

## Step 3 — Find PID

```bash
pgrep sleep
```

---

## Step 4 — Inspect Process

```bash
ps -p <PID> -f
```

---

## Step 5 — Stop Process

```bash
kill <PID>
```

---

## Step 6 — Verify

```bash
pgrep sleep
```

If nothing is returned, the process has ended.

---

# 39. Second Practice Lab — Stop and Resume

Start:

```bash
sleep 300
```

Press:

```text
Ctrl + Z
```

Check:

```bash
jobs
```

Resume:

```bash
bg
```

Check:

```bash
jobs
```

Bring it foreground:

```bash
fg
```

Terminate:

```text
Ctrl + C
```

---

# 40. Third Practice Lab — Signals

Start:

```bash
sleep 300 &
```

Find PID:

```bash
pgrep sleep
```

Suppose:

```text
3000
```

Stop:

```bash
kill -STOP 3000
```

Check:

```bash
ps -p 3000 -o pid,stat,cmd
```

Resume:

```bash
kill -CONT 3000
```

Finally:

```bash
kill 3000
```

---

# 41. Process Priority Lab

Start:

```bash
nice -n 10 sleep 300 &
```

Find it:

```bash
pgrep sleep
```

Check priority:

```bash
ps -C sleep -o pid,ni,pri,cmd
```

You should see the nice value.

---

# 42. Troubleshooting a Java Application

Imagine:

```text
Application is running slowly.
```

First:

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

Inspect:

```bash
ps -p <PID> -o pid,ppid,%cpu,%mem,stat,ni,cmd
```

Monitor:

```bash
top
```

If the application needs to be stopped:

```bash
kill <PID>
```

Only use:

```bash
kill -9 <PID>
```

if graceful termination has failed and forceful termination is appropriate.

---

# 43. Common Mistakes

## Mistake 1

Using:

```bash
kill -9
```

for everything.

### Better:

Try:

```bash
kill PID
```

first.

---

## Mistake 2

Confusing Job ID with PID.

```text
[1] 2500
```

Remember:

```text
1    = Job ID
2500 = PID
```

---

## Mistake 3

Using `pkill java` without checking.

First:

```bash
pgrep -a java
```

Then decide what should be stopped.

---

## Mistake 4

Stopping the wrong process.

Always verify:

```bash
ps -p <PID> -f
```

before sending important signals.

---

# 44. Command Summary

| Command          | Purpose                            |
| ---------------- | ---------------------------------- |
| `command &`      | Run in background                  |
| `jobs`           | Show shell jobs                    |
| `fg`             | Foreground job                     |
| `bg`             | Background job                     |
| `Ctrl + C`       | Interrupt foreground process       |
| `Ctrl + Z`       | Suspend foreground process         |
| `kill PID`       | Send SIGTERM                       |
| `kill -TERM PID` | Graceful termination request       |
| `kill -KILL PID` | Force termination                  |
| `kill -9 PID`    | SIGKILL                            |
| `kill -STOP PID` | Stop process                       |
| `kill -CONT PID` | Continue process                   |
| `kill -0 PID`    | Check process/signaling permission |
| `pkill name`     | Signal matching processes          |
| `killall name`   | Signal processes by name           |
| `nice`           | Start with modified priority       |
| `renice`         | Change priority                    |
| `ps`             | Process information                |
| `top`            | Live monitoring                    |
| `pgrep`          | Find processes                     |

---

# 45. Interview Questions 🎯

### Q1. What is process control?

Process control means managing processes by starting, stopping, suspending, resuming, monitoring, or changing their priority.

---

### Q2. What does `&` do?

It starts a command as a background job.

Example:

```bash
sleep 100 &
```

---

### Q3. What is the difference between `bg` and `fg`?

`bg` resumes a stopped job in the background.

`fg` brings a job to the foreground.

---

### Q4. What does `Ctrl + C` do?

It normally sends `SIGINT` to the foreground process.

---

### Q5. What does `Ctrl + Z` do?

It normally sends `SIGTSTP`, suspending the foreground process.

---

### Q6. What does `kill` do?

`kill` sends a signal to a process.

---

### Q7. What is SIGTERM?

SIGTERM is a request for a process to terminate gracefully.

---

### Q8. What is SIGKILL?

SIGKILL forces a process to terminate and cannot be caught or ignored by the target process.

---

### Q9. Difference between SIGTERM and SIGKILL?

```text
SIGTERM → graceful termination request
SIGKILL → immediate forced termination
```

---

### Q10. What is `pkill`?

`pkill` sends signals to processes based on matching criteria such as process name.

---

### Q11. What is `nice`?

`nice` starts a process with a specified scheduling niceness value.

---

### Q12. What is `renice`?

`renice` changes the nice value of an existing process.

---

### Q13. How do you stop a Java process gracefully?

Find its PID:

```bash
pgrep -a java
```

Then:

```bash
kill <PID>
```

---

### Q14. When would you use `kill -9`?

When a process does not terminate normally after a graceful termination request and forceful termination is appropriate.

---

# 🧠 Day 13 Quick Revision

Remember:

```text
&       → Background
jobs    → Show jobs
fg      → Foreground
bg      → Background
Ctrl+C  → Interrupt
Ctrl+Z  → Suspend
kill    → Send signal
SIGTERM → Graceful termination
SIGKILL → Force termination
STOP    → Stop
CONT    → Continue
nice    → Start with priority adjustment
renice  → Change priority
```

---

# 🔥 Process Control Flow

```text
              ┌──────────────┐
              │  Foreground  │
              └──────┬───────┘
                     │
                  Ctrl + Z
                     ↓
              ┌──────────────┐
              │    Stopped   │
              └──────┬───────┘
                     │
                    bg
                     ↓
              ┌──────────────┐
              │  Background  │
              └──────┬───────┘
                     │
                    fg
                     ↓
              ┌──────────────┐
              │  Foreground  │
              └──────┬───────┘
                     │
                  Ctrl + C
                     ↓
              ┌──────────────┐
              │  Terminated  │
              └──────────────┘
```

---

# 🐧 Day 13 Complete



