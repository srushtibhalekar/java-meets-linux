# 🐧 Day 01 — Linux Basics & Essential Commands

> **30 Days of Linux — Day 01**

## 🎯 Mission

**Enter the terminal. Understand where you are. Learn how to communicate with the Linux system.**

---

# 1. What is Linux?

Linux is an **open-source operating system kernel**. Distributions such as Ubuntu, Debian, Fedora, Arch, and Kali combine the Linux kernel with other software to provide a complete operating system.

Linux is heavily used in:

* ☁️ Cloud computing
* 🖥️ Servers
* 🔐 Cybersecurity
* ⚙️ DevOps
* 🌐 Networking
* 🐳 Containers
* 💻 Software development

---

# 2. Linux Architecture

A simplified Linux system looks like this:

```text
                USER
                  │
                  ▼
          ┌───────────────┐
          │   Terminal    │
          └───────┬───────┘
                  │
                  ▼
          ┌───────────────┐
          │     Shell     │
          │ Bash / Zsh    │
          └───────┬───────┘
                  │
                  ▼
          ┌───────────────┐
          │ Linux Kernel  │
          └───────┬───────┘
                  │
                  ▼
          ┌───────────────┐
          │    Hardware   │
          └───────────────┘
```

### Kernel

The **kernel** is the core of Linux.

It manages:

* CPU
* Memory
* Processes
* Devices
* Filesystems
* Networking

### Shell

The shell allows you to communicate with the operating system using commands.

Examples:

```bash
bash
zsh
fish
```

For this challenge, most examples will use **Bash**.

---

# 3. Terminal vs Shell

These two terms are related but different.

### Terminal

The terminal is the application/interface where you type commands.

Examples:

* GNOME Terminal
* Konsole
* Windows Terminal

### Shell

The shell interprets the commands you type.

Example:

```bash
ls
```

The terminal receives your input and the shell interprets it.

---

# 4. The Linux Command Structure

Most commands follow this pattern:

```text
command + options + arguments
```

Example:

```bash
ls -l /home
```

Breakdown:

```text
ls      → command
-l      → option
/home   → argument
```

Another example:

```bash
echo "Hello Linux"
```

```text
echo          → command
"Hello Linux" → argument
```

---

# 5. `pwd` — Where Am I?

`pwd` means:

**Print Working Directory**

```bash
pwd
```

Example output:

```text
/home/user
```

This tells you your current location in the filesystem.

### Practice

```bash
pwd
```

Ask yourself:

> Where am I currently working?

---

# 6. `ls` — List Contents

Use:

```bash
ls
```

to see files and directories.

Example:

```text
Documents
Downloads
Music
Pictures
Projects
```

### Detailed listing

```bash
ls -l
```

### Show hidden files

```bash
ls -a
```

### Detailed + hidden files

```bash
ls -la
```

### Human-readable file sizes

```bash
ls -lh
```

You can combine options:

```bash
ls -lah
```

---

# 7. Understanding `ls -l`

Example:

```text
drwxr-xr-x 2 user user 4096 Aug 29 Documents
```

Basic interpretation:

```text
d           → directory
rwx         → owner permissions
r-x         → group permissions
r-x         → others permissions
2           → link count
user        → owner
user        → group
4096        → size
Aug 29      → modification date
Documents   → name
```

Permissions will be studied deeply on **Day 8**.

---

# 8. `cd` — Change Directory

`cd` means:

**Change Directory**

Go into a directory:

```bash
cd Documents
```

Go back one level:

```bash
cd ..
```

Go to your home directory:

```bash
cd ~
```

You can also use:

```bash
cd
```

to return to your home directory in Bash.

---

# 9. Absolute vs Relative Paths

Understanding paths is extremely important.

## Absolute Path

An absolute path starts from `/`.

Example:

```bash
cd /home/user/Documents
```

It describes the complete location.

---

## Relative Path

A relative path starts from your current location.

Example:

```bash
cd Documents
```

If you are currently in:

```text
/home/user
```

then:

```bash
cd Documents
```

takes you to:

```text
/home/user/Documents
```

---

# 10. Special Path Symbols

Linux uses special symbols for navigation.

| Symbol | Meaning           |
| ------ | ----------------- |
| `/`    | Root directory    |
| `~`    | Home directory    |
| `.`    | Current directory |
| `..`   | Parent directory  |

Examples:

```bash
cd /
```

```bash
cd ~
```

```bash
cd .
```

```bash
cd ..
```

---

# 11. `/` — Root Directory

Linux has a single filesystem hierarchy beginning at:

```text
/
```

This is called the **root directory**.

View it:

```bash
cd /
ls
```

You may see:

```text
bin
boot
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

You will learn these directories throughout the challenge.

---

# 12. `clear` — Clean the Terminal

To clear the terminal:

```bash
clear
```

Keyboard shortcut:

```text
Ctrl + L
```

---

# 13. `whoami` — Current User

Run:

```bash
whoami
```

Example:

```text
srushti
```

This tells you which user account is currently active.

---

# 14. `id` — User Identity Information

Run:

```bash
id
```

Example:

```text
uid=1000(user) gid=1000(user) groups=1000(user)
```

It provides information about:

* User ID
* Group ID
* Groups

Users and groups will be covered later.

---

# 15. `date` — System Date & Time

Run:

```bash
date
```

Example:

```text
Sat Aug 29 20:30:00 IST 2026
```

This displays the system's current date and time.

---

# 16. `echo` — Display Text

Basic example:

```bash
echo "Hello Linux"
```

Output:

```text
Hello Linux
```

You can also display values:

```bash
echo $HOME
```

We will study environment variables on **Day 19**.

---

# 17. `uname` — System Information

Run:

```bash
uname
```

Example:

```text
Linux
```

Get more information:

```bash
uname -a
```

This can display information about:

* Kernel
* Hostname
* Kernel release
* Architecture

---

# 18. `hostname` — System Name

Run:

```bash
hostname
```

This displays the system's hostname.

Example:

```text
ubuntu
```

---

# 19. `history` — Previous Commands

Linux remembers commands you previously executed.

Run:

```bash
history
```

Example:

```text
1  pwd
2  ls
3  cd Documents
4  ls -la
5  whoami
```

Run a previous command using:

```bash
!5
```

This executes command number 5.

### Useful shortcut

Press:

```text
↑
```

to move through previous commands.

---

# 20. `man` — Manual Pages

Linux provides built-in documentation.

Try:

```bash
man ls
```

You will see documentation for `ls`.

Other examples:

```bash
man pwd
```

```bash
man cd
```

```bash
man echo
```

To exit a manual page:

```text
q
```

---

# 21. `command --help`

Many commands provide quick help.

Example:

```bash
ls --help
```

Another:

```bash
uname --help
```

This is useful when you want a quick reminder instead of reading the complete manual.

---

# 22. `which` — Locate a Command

Run:

```bash
which ls
```

Example:

```text
/usr/bin/ls
```

This tells you where the executable being selected by your shell is located.

Try:

```bash
which bash
```

---

# 23. Command Types

Not every shell command is simply an external executable.

Bash can have:

* Built-in commands
* External commands
* Aliases
* Functions

Check how Bash interprets a command:

```bash
type cd
```

Example:

```text
cd is a shell builtin
```

Try:

```bash
type ls
```

---

# 24. Getting System Information

Useful commands:

```bash
uname -a
```

```bash
hostname
```

```bash
whoami
```

```bash
id
```

```bash
date
```

Together, these give you a quick overview of your environment.

---

# 25. Command Line Shortcuts

Learn these early.

| Shortcut   | Action                |
| ---------- | --------------------- |
| `Ctrl + L` | Clear terminal        |
| `Ctrl + C` | Stop current command  |
| `Ctrl + D` | Exit shell / send EOF |
| `Ctrl + A` | Beginning of line     |
| `Ctrl + E` | End of line           |
| `↑`        | Previous command      |
| `↓`        | Next command          |
| `Tab`      | Auto-completion       |

### ⭐ Most important

```text
Tab
```

Use Tab to complete filenames, directories, and commands.

---

# 26. First Linux Navigation Exercise

Start:

```bash
pwd
```

Then:

```bash
ls
```

Go to the root:

```bash
cd /
```

Check:

```bash
pwd
```

List its contents:

```bash
ls
```

Return home:

```bash
cd ~
```

Check:

```bash
pwd
```

---

# 27. Information Challenge

Run:

```bash
whoami
```

```bash
hostname
```

```bash
uname -a
```

```bash
date
```

```bash
id
```

Then:

```bash
history
```

---

# 28. Mini Mission 🚀

Without copying the complete sequence blindly, try to:

### Mission 1

Find your current location.

```bash
pwd
```

### Mission 2

List everything, including hidden files.

```bash
ls -la
```

### Mission 3

Go to `/`.

```bash
cd /
```

### Mission 4

Explore its directories.

```bash
ls
```

### Mission 5

Return to your home directory.

```bash
cd ~
```

### Mission 6

Find your username.

```bash
whoami
```

### Mission 7

Check your Linux kernel.

```bash
uname -a
```

### Mission 8

Read the manual for `ls`.

```bash
man ls
```

Exit with:

```text
q
```

---

# 29. Quick Reference

```bash
pwd             # Current directory
ls              # List contents
ls -l           # Detailed listing
ls -a           # Show hidden files
ls -la          # Detailed + hidden
cd folder       # Enter directory
cd ..           # Parent directory
cd ~            # Home directory
cd /            # Root directory
clear           # Clear terminal
whoami          # Current user
id              # User/group information
date            # Date and time
echo            # Display text
uname -a        # System/kernel information
hostname        # System hostname
history         # Command history
man ls          # Manual page
ls --help       # Quick command help
which ls        # Locate command
type cd         # Identify command type
```

---

# 🧠 What I Learned Today

* Linux filesystem starts at `/`
* `~` represents the home directory
* `.` represents the current directory
* `..` represents the parent directory
* Absolute paths start from `/`
* Relative paths start from the current directory
* The shell interprets commands
* `pwd` tells me where I am
* `ls` lets me inspect directories
* `cd` lets me navigate
* `man` gives me documentation
* `history` shows previous commands
* `Tab` makes terminal navigation faster

---


