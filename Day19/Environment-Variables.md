# 🌍 Day 19 — Linux Environment Variables

Environment variables are an important part of Linux and are heavily used in **Java development, Spring Boot, databases, DevOps, and server configuration**.

They allow applications and shell commands to access configuration values without hard-coding them directly into the program.

---

# 🎯 Learning Objectives

By the end of Day 19, you will understand:

* What environment variables are
* Shell variables vs environment variables
* `printenv`
* `env`
* `set`
* `echo`
* `export`
* `unset`
* `$PATH`
* `$HOME`
* `$USER`
* `$SHELL`
* `$JAVA_HOME`
* Temporary variables
* Persistent variables
* `.bashrc`
* `.profile`
* `source`
* Environment variables in Java applications
* Safe configuration practices

---

# 1. What Is an Environment Variable?

An environment variable is a **named value stored by the operating system/shell** that can be used by processes.

Example:

```bash
NAME="Srushti"
```

You can access it using:

```bash
echo $NAME
```

Output:

```text
Srushti
```

Environment variables are commonly used for:

* Application configuration
* Database configuration
* Java configuration
* PATH configuration
* API configuration
* Server configuration
* Development environments

---

# 2. Shell Variable vs Environment Variable

A normal shell variable:

```bash
NAME="Srushti"
```

is available in the current shell.

An exported environment variable:

```bash
export NAME="Srushti"
```

can also be inherited by child processes.

### Example

```bash
NAME="Srushti"
bash
echo $NAME
```

The child shell may not receive the normal shell variable.

Now try:

```bash
export NAME="Srushti"
bash
echo $NAME
```

Output:

```text
Srushti
```

### Important

```text
variable
   ↓
current shell

exported variable
   ↓
current shell + child processes
```

---

# 3. View Environment Variables

## `printenv`

Display environment variables:

```bash
printenv
```

You may see:

```text
HOME=/home/user
USER=user
SHELL=/bin/bash
PATH=/usr/local/bin:/usr/bin:/bin
```

---

# 4. Print One Variable

Use:

```bash
printenv HOME
```

Example:

```text
/home/user
```

You can also use:

```bash
printenv USER
```

---

# 5. Using `echo`

Another common method:

```bash
echo $HOME
```

```bash
echo $USER
```

```bash
echo $SHELL
```

```bash
echo $PATH
```

---

# 6. Important Linux Environment Variables

## `$HOME`

Home directory:

```bash
echo $HOME
```

Example:

```text
/home/srushti
```

---

## `$USER`

Current username:

```bash
echo $USER
```

Example:

```text
srushti
```

---

## `$SHELL`

Current/default shell:

```bash
echo $SHELL
```

Example:

```text
/bin/bash
```

---

## `$PWD`

Current working directory:

```bash
echo $PWD
```

---

## `$OLDPWD`

Previous working directory:

```bash
echo $OLDPWD
```

---

## `$PATH`

Contains directories where Linux searches for executable commands.

```bash
echo $PATH
```

Example:

```text
/usr/local/bin:/usr/bin:/bin
```

The directories are separated by:

```text
:
```

---

## `$LANG`

Language/locale configuration:

```bash
echo $LANG
```

---

## `$TERM`

Terminal type:

```bash
echo $TERM
```

---

# 7. The PATH Variable

`PATH` is one of the most important Linux environment variables.

Suppose you type:

```bash
java
```

Linux needs to find the `java` executable.

It searches directories listed in:

```bash
echo $PATH
```

Example:

```text
/usr/local/bin:/usr/bin:/bin:/usr/lib/jvm/bin
```

Linux searches these directories.

---

# 8. Check Where a Command Comes From

Use:

```bash
which java
```

or:

```bash
command -v java
```

Example:

```text
/usr/bin/java
```

This tells you which executable will be used.

---

# 9. Creating a Variable

Create a normal shell variable:

```bash
NAME="Srushti"
```

Check it:

```bash
echo $NAME
```

Output:

```text
Srushti
```

### Important

Do NOT put spaces around `=`.

Correct:

```bash
NAME="Srushti"
```

Wrong:

```bash
NAME = "Srushti"
```

---

# 10. Exporting a Variable

Use:

```bash
export NAME="Srushti"
```

Check:

```bash
echo $NAME
```

Check through:

```bash
printenv NAME
```

Output:

```text
Srushti
```

---

# 11. Export an Existing Variable

You can also do:

```bash
NAME="Srushti"
export NAME
```

Now `NAME` becomes available to child processes.

---

# 12. Variable Expansion

Linux replaces variables with their values.

Example:

```bash
NAME="Srushti"
echo "Hello $NAME"
```

Output:

```text
Hello Srushti
```

You can also use:

```bash
echo "User: ${NAME}"
```

---

# 13. Why `${VARIABLE}` Is Useful

Suppose:

```bash
NAME="Java"
```

Then:

```bash
echo "$NAMEDeveloper"
```

Linux may interpret the variable name as:

```text
NAMEDeveloper
```

Instead use:

```bash
echo "${NAME}Developer"
```

Output:

```text
JavaDeveloper
```

---

# 14. Single Quotes vs Double Quotes

This is important.

### Double quotes

Variables are expanded:

```bash
NAME="Srushti"
echo "Hello $NAME"
```

Output:

```text
Hello Srushti
```

### Single quotes

Variables are not expanded:

```bash
echo 'Hello $NAME'
```

Output:

```text
Hello $NAME
```

### Remember

```text
"..."  → variable expansion works

'...'  → variable expansion does not happen
```

---

# 15. Remove a Variable

Use:

```bash
unset NAME
```

Check:

```bash
echo $NAME
```

Output will normally be empty.

Check:

```bash
printenv NAME
```

If the variable doesn't exist, nothing is printed.

---

# 16. `env`

Display environment variables:

```bash
env
```

You can combine it with other commands.

Example:

```bash
env | sort
```

This displays variables alphabetically.

---

# 17. `printenv` vs `env`

Both can display environment variables.

### `printenv`

Useful for displaying environment variables:

```bash
printenv
```

or:

```bash
printenv HOME
```

### `env`

Useful for viewing variables and running a command with modified environment variables.

Example:

```bash
env NAME="Srushti" bash
```

---

# 18. What Is `set`?

Use:

```bash
set
```

It displays shell variables, functions, and other shell information.

It can produce much more output than:

```bash
printenv
```

### Basic difference

```text
printenv → environment variables

env      → environment variables + execute commands with modified environment

set      → shell variables + functions + shell information
```

---

# 19. Temporary Environment Variables

A variable created in the current terminal session can disappear when the shell closes.

Example:

```bash
export PROJECT_NAME="JavaApp"
```

Check:

```bash
echo $PROJECT_NAME
```

Close the terminal.

Open a new terminal.

Then:

```bash
echo $PROJECT_NAME
```

It may be empty.

This is a **session-level configuration**.

---

# 20. Persistent Environment Variables

If you want a variable to be available automatically whenever your Bash shell starts, you can put the configuration in shell startup files.

Common files include:

```text
~/.bashrc
~/.profile
```

---

# 21. `.bashrc`

For Bash interactive shell configuration:

```bash
nano ~/.bashrc
```

Add:

```bash
export PROJECT_NAME="JavaApp"
```

Save the file.

Then reload it:

```bash
source ~/.bashrc
```

Check:

```bash
echo $PROJECT_NAME
```

Output:

```text
JavaApp
```

---

# 22. What Does `source` Do?

`source` reads and executes commands from a file in the current shell.

Example:

```bash
source ~/.bashrc
```

Short form:

```bash
. ~/.bashrc
```

Both perform the same basic operation.

---

# 23. `.profile`

Another startup configuration file is:

```bash
~/.profile
```

You can edit it using:

```bash
nano ~/.profile
```

For example:

```bash
export PROJECT_NAME="JavaApp"
```

After appropriate shell/session reload or login, the variable can become available.

---

# 24. Important Difference: `.bashrc` vs `.profile`

A simple way to remember:

```text
.bashrc
    ↓
Bash interactive shell configuration

.profile
    ↓
Login/session environment configuration
```

The exact startup behavior can depend on how your shell/session is launched.

For beginner practice, focus first on:

```bash
~/.bashrc
```

---

# 25. Safely Modify PATH

Suppose you have your own executable directory:

```text
$HOME/bin
```

Add it to PATH:

```bash
export PATH="$HOME/bin:$PATH"
```

Now:

```bash
echo $PATH
```

You should see `$HOME/bin` at the beginning.

---

# ⚠️ Important PATH Warning

Never casually do:

```bash
export PATH="$HOME/bin"
```

This replaces your existing PATH.

You may then find that commands such as:

```bash
ls
java
git
```

are no longer found.

Prefer:

```bash
export PATH="$HOME/bin:$PATH"
```

This keeps the existing PATH.

---

# 26. JAVA_HOME

Java developers commonly use:

```bash
JAVA_HOME
```

It points to the JDK installation.

Example:

```bash
export JAVA_HOME="/path/to/jdk"
```

Then:

```bash
export PATH="$JAVA_HOME/bin:$PATH"
```

Check:

```bash
echo $JAVA_HOME
```

Check Java:

```bash
java --version
```

Check compiler:

```bash
javac --version
```

Check executable:

```bash
which java
```

---

# 27. Java Environment Example

Suppose your application requires:

```text
JAVA_HOME
DB_HOST
DB_PORT
DB_NAME
```

You can configure:

```bash
export JAVA_HOME="/path/to/jdk"
export DB_HOST="localhost"
export DB_PORT="5432"
export DB_NAME="studentdb"
```

Check:

```bash
echo $DB_HOST
echo $DB_PORT
echo $DB_NAME
```

This allows application configuration to remain outside the source code.

---

# 28. Environment Variables and Java

Java programs can read environment variables.

Example Java code:

```java
public class EnvironmentDemo {

    public static void main(String[] args) {

        String username = System.getenv("USER");
        String home = System.getenv("HOME");

        System.out.println("User: " + username);
        System.out.println("Home: " + home);
    }
}
```

Compile:

```bash
javac EnvironmentDemo.java
```

Run:

```bash
java EnvironmentDemo
```

---

# 29. Java Reading Custom Environment Variables

Linux:

```bash
export APP_NAME="StudentManagement"
```

Java:

```java
public class EnvironmentDemo {

    public static void main(String[] args) {

        String appName = System.getenv("APP_NAME");

        System.out.println("Application: " + appName);
    }
}
```

Run:

```bash
javac EnvironmentDemo.java
java EnvironmentDemo
```

Output:

```text
Application: StudentManagement
```

---

# 30. Why Developers Use Environment Variables

Environment variables are useful when the same application runs in different environments.

For example:

```text
Development
    DB_HOST=localhost

Testing
    DB_HOST=test-server

Production
    DB_HOST=production-server
```

The application code can remain the same while configuration changes.

---

# 31. Environment Variables for Spring Boot

A Spring Boot application can use environment variables for configuration.

For example:

```bash
export DB_HOST="localhost"
export DB_PORT="5432"
export DB_NAME="studentdb"
```

Application configuration can reference these values.

Conceptually:

```text
Linux Environment
       ↓
Environment Variables
       ↓
Spring Boot
       ↓
Database Connection
```

This is especially useful when deploying applications to servers.

---

# 32. Security Warning

Environment variables are useful, but they are **not a perfect secret-management system**.

Avoid exposing sensitive information unnecessarily.

For example, don't casually put:

```bash
export DB_PASSWORD="mypassword"
```

into files that might be committed to Git.

Never commit secrets such as:

```text
API keys
Database passwords
Private tokens
Cloud credentials
Access tokens
```

to GitHub.

Use proper secret-management mechanisms for production systems.

---

# 33. View PATH Clearly

Run:

```bash
echo $PATH
```

To display each directory on a separate line:

```bash
echo $PATH | tr ':' '\n'
```

Example:

```text
/usr/local/bin
/usr/bin
/bin
/home/user/bin
```

This is very useful when troubleshooting command-not-found problems.

---

# 34. Environment Variable Debugging

Suppose:

```bash
java
```

returns:

```text
command not found
```

Check:

```bash
echo $PATH
```

Then:

```bash
which java
```

Then:

```bash
echo $JAVA_HOME
```

Then:

```bash
ls "$JAVA_HOME/bin"
```

This helps determine whether the problem is related to Java installation or PATH configuration.

---

# 35. Useful Commands

| Command              | Purpose                           |
| -------------------- | --------------------------------- |
| `echo $VAR`          | Display variable                  |
| `printenv`           | Display environment variables     |
| `printenv VAR`       | Display specific variable         |
| `env`                | Display environment               |
| `set`                | Display shell variables/functions |
| `export VAR=value`   | Export variable                   |
| `unset VAR`          | Remove variable                   |
| `source file`        | Reload configuration              |
| `echo $PATH`         | Display PATH                      |
| `which command`      | Locate executable                 |
| `command -v command` | Locate command                    |

---

# 36. 🧪 Practice Lab

Create a test environment.

### Step 1 — Create variables

```bash
export STUDENT_NAME="Srushti"
export COURSE="Information Technology"
export SKILL="Java"
```

### Step 2 — Display them

```bash
echo $STUDENT_NAME
echo $COURSE
echo $SKILL
```

### Step 3 — Check with printenv

```bash
printenv STUDENT_NAME
printenv COURSE
printenv SKILL
```

### Step 4 — Display all variables

```bash
env | sort
```

### Step 5 — Remove one

```bash
unset SKILL
```

Check:

```bash
echo $SKILL
```

---

# 37. 🧪 PATH Practice

Check:

```bash
echo $PATH
```

Then:

```bash
echo $PATH | tr ':' '\n'
```

Check Java:

```bash
which java
```

Check:

```bash
echo $JAVA_HOME
```

If `JAVA_HOME` is not configured, do not randomly set it to a path that does not exist.

First locate Java:

```bash
readlink -f "$(which java)"
```

The result can help you identify the JDK/JRE location.

---

# 38. 🧪 Java Environment Lab

Create:

```bash
mkdir -p ~/day19-java
cd ~/day19-java
```

Create a Java file:

```bash
nano EnvironmentDemo.java
```

Use:

```java
public class EnvironmentDemo {

    public static void main(String[] args) {

        System.out.println("User: " + System.getenv("USER"));
        System.out.println("Home: " + System.getenv("HOME"));
        System.out.println("Shell: " + System.getenv("SHELL"));
        System.out.println("Java Home: " + System.getenv("JAVA_HOME"));
        System.out.println("Application: " + System.getenv("APP_NAME"));
    }
}
```

Set:

```bash
export APP_NAME="Java-Linux-Lab"
```

Compile:

```bash
javac EnvironmentDemo.java
```

Run:

```bash
java EnvironmentDemo
```

---

# 39. 🛠 Mini Project — Linux Environment Report

Create:

```bash
mkdir -p ~/Day19-Project
cd ~/Day19-Project
```

Create:

```bash
nano environment-report.sh
```

Add:

```bash
#!/bin/bash

echo "===== Linux Environment Report ====="

echo "User: $USER"
echo "Home: $HOME"
echo "Shell: $SHELL"
echo "Current Directory: $PWD"
echo "Hostname: $(hostname)"
echo "Java Version:"
java --version 2>&1 | head -n 1

echo ""
echo "===== PATH ====="
echo "$PATH" | tr ':' '\n'

echo ""
echo "===== Java Location ====="
command -v java
```

Make it executable:

```bash
chmod +x environment-report.sh
```

Run:

```bash
./environment-report.sh
```

---

# 40. 🔥 Day 19 Challenge

Complete these tasks without looking at the answers.

### Task 1

Create:

```text
MY_NAME
```

with your name.

---

### Task 2

Export:

```text
PROJECT=JavaLinux
```

---

### Task 3

Display:

```text
PROJECT
```

using `printenv`.

---

### Task 4

Remove the variable using:

```bash
unset
```

---

### Task 5

Display every directory in PATH on a separate line.

Hint:

```bash
tr
```

---

### Task 6

Find the location of:

```text
java
git
```

using:

```bash
command -v
```

---

### Task 7

Create a Java program that reads:

```text
APP_NAME
```

using:

```java
System.getenv()
```

---

# 41. 💼 Real-World Developer Workflow

A common development workflow looks like:

```text
Linux Server
     ↓
Environment Variables
     ↓
Application Configuration
     ↓
Java / Spring Boot
     ↓
Database / API / Services
```

Example:

```bash
export DB_HOST="localhost"
export DB_PORT="5432"
export DB_NAME="studentdb"
```

Your application can read these values instead of hard-coding configuration.

---

# 42. Common Mistakes

### Mistake 1 — Spaces around `=`

Wrong:

```bash
NAME = "Srushti"
```

Correct:

```bash
NAME="Srushti"
```

---

### Mistake 2 — Forgetting `export`

```bash
NAME="Srushti"
```

is not automatically exported.

Use:

```bash
export NAME="Srushti"
```

when child processes need it.

---

### Mistake 3 — Overwriting PATH

Avoid:

```bash
export PATH="/my-folder"
```

Prefer:

```bash
export PATH="/my-folder:$PATH"
```

---

### Mistake 4 — Forgetting to reload `.bashrc`

After editing:

```bash
~/.bashrc
```

run:

```bash
source ~/.bashrc
```

---

# 43. 🎤 Interview Questions

### 1. What is an environment variable?

A named value provided to processes by the shell/environment for configuration and runtime information.

### 2. What is the difference between a shell variable and an environment variable?

A shell variable normally belongs to the current shell, while an exported environment variable is inherited by child processes.

### 3. What does `export` do?

It marks a shell variable for inheritance by child processes.

### 4. What is PATH?

`PATH` is a list of directories where the shell searches for executable commands.

### 5. What is JAVA_HOME?

It is commonly used to specify the location of a Java installation/JDK.

### 6. What does `unset` do?

It removes a variable from the current shell environment.

### 7. What does `source ~/.bashrc` do?

It reads the `.bashrc` file and applies its commands to the current shell.

### 8. How can Java read an environment variable?

Using:

```java
System.getenv("VARIABLE_NAME")
```

### 9. Why are environment variables useful?

They separate application configuration from source code and make it easier to run the same application in different environments.

### 10. Are environment variables a secure secret store?

No. They can expose sensitive values depending on the system, process inspection, logging, debugging, and configuration practices. Production secrets should use appropriate secret-management solutions.

---

# 44. ⚡ Quick Cheat Sheet

```bash
# Display variable
echo $HOME

# Display all environment variables
printenv

# Display one variable
printenv HOME

# Environment
env

# Shell variables/functions
set

# Create variable
NAME="Srushti"

# Export variable
export NAME="Srushti"

# Export existing variable
export NAME

# Remove variable
unset NAME

# PATH
echo $PATH

# PATH line by line
echo $PATH | tr ':' '\n'

# Find command
which java

# Find command
command -v java

# Reload Bash configuration
source ~/.bashrc

# Java
echo $JAVA_HOME
java --version
javac --version
```

---

# 📁 Day 19 Repository Structure

```text
Java-Meets-Linux/
└── DAY19/
    └── Environment-Variables.md
```

---

# 🚀 GitHub Push

From PowerShell:

```powershell
cd C:\Desktop\Java-Meets-Linux
```

Check status:

```powershell
git status
```

Add the file:

```powershell
git add Day19\Environment-Variables.md
```

Check:

```powershell
git status
```

Commit:

```powershell
git commit -m "docs: add day 19 environment variables"
```

Push:

```powershell
git push
```

Verify:

```powershell
git status
```

You should see:

```text
Your branch is up to date with 'origin/main'.
```

---

# 🎯 Day 19 Complete

Today you learned:

```text
Environment Variables
        ↓
printenv / env / set
        ↓
export / unset
        ↓
PATH
        ↓
JAVA_HOME
        ↓
.bashrc / .profile
        ↓
source
        ↓
Java System.getenv()
        ↓
Application Configuration
```


