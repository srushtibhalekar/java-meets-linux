# 🐚 Day 20 — Bash Fundamentals

Bash scripting is one of the most useful Linux skills for **automation, DevOps, server management, Java deployment, system administration, and repetitive tasks**.

A Bash script is simply a text file containing Linux commands that can be executed together.

---

# 🎯 Learning Objectives

By the end of Day 20, you will understand:

* What Bash is
* What a shell is
* What a Bash script is
* Shebang
* Creating a `.sh` file
* Executing Bash scripts
* Variables
* `echo`
* `read`
* Command substitution
* User input
* Positional arguments
* `$0`, `$1`, `$2`
* `$#`
* `$@`
* `$?`
* Exit status
* Comments
* Basic Bash automation

---

# 1. What Is a Shell?

A shell is a program that allows you to interact with the operating system using commands.

Example:

```bash
ls
```

```bash
pwd
```

```bash
cd /home
```

The shell receives the command and asks the operating system to execute it.

Common shells include:

```text
Bash
Zsh
Fish
sh
Ksh
```

---

# 2. What Is Bash?

Bash means:

```text
Bourne Again SHell
```

Bash is one of the most commonly used shells on Linux.

Check your shell:

```bash
echo $SHELL
```

Example:

```text
/bin/bash
```

Check Bash version:

```bash
bash --version
```

---

# 3. What Is a Bash Script?

A Bash script is a file containing multiple commands.

Example:

```bash
#!/bin/bash

echo "Hello Linux"
pwd
whoami
date
```

Instead of manually running:

```bash
pwd
whoami
date
```

you can put them inside one script.

---

# 4. Why Use Bash Scripts?

Bash scripts are useful for:

* Automation
* Backups
* Log management
* Server administration
* Application deployment
* Monitoring
* File management
* Running multiple commands
* Scheduled tasks
* DevOps workflows

For a Java developer, Bash can be used to:

```text
Compile Java
     ↓
Run Java
     ↓
Create logs
     ↓
Backup files
     ↓
Start application
```

---

# 5. Your First Bash Script

Create a directory:

```bash
mkdir -p ~/day20
cd ~/day20
```

Create a file:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello from Bash!"
echo "Welcome to Linux."
```

Save the file.

---

# 6. Understanding the Shebang

The first line:

```bash
#!/bin/bash
```

is called the **shebang**.

It tells the system which interpreter should execute the script.

For Bash:

```bash
#!/bin/bash
```

Another common form is:

```bash
#!/usr/bin/env bash
```

For this course, use:

```bash
#!/bin/bash
```

---

# 7. Execute the Script Using Bash

You can directly run:

```bash
bash hello.sh
```

Output:

```text
Hello from Bash!
Welcome to Linux.
```

This does not require the script itself to have execute permission.

---

# 8. Make the Script Executable

Run:

```bash
chmod +x hello.sh
```

Check:

```bash
ls -l hello.sh
```

You should see an `x` permission.

Example:

```text
-rwxr-xr-x
```

Now run:

```bash
./hello.sh
```

---

# 9. Difference Between `bash script.sh` and `./script.sh`

### Method 1

```bash
bash hello.sh
```

You explicitly tell Bash to execute the file.

### Method 2

```bash
./hello.sh
```

The system uses the shebang and execute permission.

For:

```bash
./hello.sh
```

you generally need:

```bash
chmod +x hello.sh
```

---

# 10. Comments

Comments are ignored by Bash.

Example:

```bash
#!/bin/bash

# This is a comment

echo "Hello Linux"
```

Single-line comments begin with:

```bash
#
```

Comments are useful for explaining your scripts.

---

# 11. Bash Variables

Create a variable:

```bash
name="Srushti"
```

Print it:

```bash
echo "$name"
```

Output:

```text
Srushti
```

---

# 12. Important Variable Rule

Do not put spaces around `=`.

Correct:

```bash
name="Srushti"
```

Wrong:

```bash
name = "Srushti"
```

Bash interprets the second form as a command.

---

# 13. Multiple Variables

Example:

```bash
name="Srushti"
role="Java Developer"
language="Java"
```

Print:

```bash
echo "$name"
echo "$role"
echo "$language"
```

---

# 14. Combining Variables

You can write:

```bash
name="Srushti"
language="Java"

echo "My name is $name"
echo "I am learning $language"
```

Output:

```text
My name is Srushti
I am learning Java
```

---

# 15. Curly Braces With Variables

Use:

```bash
name="Java"

echo "${name}Developer"
```

Output:

```text
JavaDeveloper
```

Curly braces clearly identify the variable name.

---

# 16. `echo`

`echo` displays text.

Example:

```bash
echo "Hello"
```

Variable:

```bash
name="Srushti"
echo "$name"
```

Multiple values:

```bash
echo "Name: $name"
```

---

# 17. User Input With `read`

Bash can ask the user for input.

Create:

```bash
nano input.sh
```

Add:

```bash
#!/bin/bash

echo "Enter your name:"
read name

echo "Hello $name"
```

Run:

```bash
bash input.sh
```

Example:

```text
Enter your name:
Srushti
Hello Srushti
```

---

# 18. `read -p`

Instead of two lines:

```bash
echo "Enter your name:"
read name
```

you can use:

```bash
read -p "Enter your name: " name
```

Example:

```bash
#!/bin/bash

read -p "Enter your name: " name

echo "Hello $name"
```

---

# 19. Reading Multiple Values

You can read multiple values:

```bash
read -p "Enter first name: " first
read -p "Enter city: " city

echo "Name: $first"
echo "City: $city"
```

---

# 20. Command Substitution

Bash can store the output of a command inside a variable.

Use:

```bash
$(command)
```

Example:

```bash
current_date=$(date)
```

Print:

```bash
echo "$current_date"
```

---

# 21. More Command Substitution Examples

```bash
current_user=$(whoami)
current_directory=$(pwd)
hostname_name=$(hostname)
```

Then:

```bash
echo "User: $current_user"
echo "Directory: $current_directory"
echo "Hostname: $hostname_name"
```

---

# 22. Example System Information Script

Create:

```bash
nano system-info.sh
```

Add:

```bash
#!/bin/bash

user=$(whoami)
directory=$(pwd)
hostname_name=$(hostname)
date_now=$(date)

echo "===== System Information ====="
echo "User: $user"
echo "Directory: $directory"
echo "Hostname: $hostname_name"
echo "Date: $date_now"
```

Run:

```bash
bash system-info.sh
```

---

# 23. Positional Arguments

Bash scripts can accept arguments from the command line.

Example:

```bash
./hello.sh Srushti
```

Inside the script:

```bash
$1
```

contains:

```text
Srushti
```

---

# 24. `$0`

`$0` represents the name/path used to run the script.

Example:

```bash
#!/bin/bash

echo "Script name: $0"
```

Run:

```bash
bash hello.sh
```

Output may look like:

```text
Script name: hello.sh
```

---

# 25. `$1`

`$1` represents the first argument.

Script:

```bash
#!/bin/bash

echo "First argument: $1"
```

Run:

```bash
bash hello.sh Srushti
```

Output:

```text
First argument: Srushti
```

---

# 26. `$2`

`$2` represents the second argument.

Script:

```bash
#!/bin/bash

echo "First: $1"
echo "Second: $2"
```

Run:

```bash
bash hello.sh Srushti Java
```

Output:

```text
First: Srushti
Second: Java
```

---

# 27. `$3`

Similarly:

```bash
#!/bin/bash

echo "Name: $1"
echo "Language: $2"
echo "City: $3"
```

Run:

```bash
bash hello.sh Srushti Java Pune
```

Output:

```text
Name: Srushti
Language: Java
City: Pune
```

---

# 28. `$#`

`$#` tells you how many arguments were provided.

Script:

```bash
#!/bin/bash

echo "Number of arguments: $#"
```

Run:

```bash
bash test.sh one two three
```

Output:

```text
Number of arguments: 3
```

---

# 29. `$@`

`$@` represents all positional arguments.

Example:

```bash
#!/bin/bash

echo "Arguments: $@"
```

Run:

```bash
bash test.sh Java Linux Git
```

Output:

```text
Arguments: Java Linux Git
```

---

# 30. `$?`

`$?` contains the exit status of the previous command.

Example:

```bash
ls
echo $?
```

If `ls` succeeds:

```text
0
```

Usually:

```text
0 → success
non-zero → error/failure
```

Example:

```bash
ls /does-not-exist
echo $?
```

The value will normally be non-zero.

---

# 31. Why Exit Status Matters

Scripts can use exit status to determine whether a command succeeded.

Example:

```bash
mkdir test
echo $?
```

If successful:

```text
0
```

This becomes very important when we learn:

* Conditions
* Loops
* Automation
* Error handling

---

# 32. `exit`

You can manually specify a script's exit status.

Example:

```bash
#!/bin/bash

echo "Script completed."

exit 0
```

Run:

```bash
bash test.sh
```

Then:

```bash
echo $?
```

Output:

```text
0
```

---

# 33. Exit With an Error

Example:

```bash
#!/bin/bash

echo "Something went wrong."

exit 1
```

Then:

```bash
echo $?
```

Output:

```text
1
```

---

# 34. Script Arguments Example

Create:

```bash
nano developer.sh
```

Add:

```bash
#!/bin/bash

echo "===== Developer Information ====="

echo "Script: $0"
echo "Name: $1"
echo "Primary Skill: $2"
echo "City: $3"
echo "Number of Arguments: $#"
```

Make executable:

```bash
chmod +x developer.sh
```

Run:

```bash
./developer.sh Srushti Java Pune
```

Output:

```text
===== Developer Information =====
Script: ./developer.sh
Name: Srushti
Primary Skill: Java
City: Pune
Number of Arguments: 3
```

---

# 35. Special Bash Variables

| Variable | Meaning                      |
| -------- | ---------------------------- |
| `$0`     | Script name                  |
| `$1`     | First argument               |
| `$2`     | Second argument              |
| `$3`     | Third argument               |
| `$#`     | Number of arguments          |
| `$@`     | All arguments                |
| `$?`     | Previous command exit status |
| `$$`     | Current shell process ID     |
| `$HOME`  | User home directory          |
| `$USER`  | Current username             |
| `$PATH`  | Executable search path       |

---

# 36. Process ID With `$$`

Example:

```bash
#!/bin/bash

echo "Current script PID: $$"
```

Run:

```bash
bash process.sh
```

You will get a process ID.

This is useful when working with:

* Processes
* Logs
* Temporary files
* Process management

---

# 37. Quoting in Bash

### Double quotes

```bash
name="Srushti"
echo "Hello $name"
```

Variable is expanded.

### Single quotes

```bash
echo 'Hello $name'
```

Output:

```text
Hello $name
```

### No quotes

Simple values may work:

```bash
name=Srushti
```

But quotes are safer when values contain:

* Spaces
* Special characters
* Shell metacharacters

Prefer:

```bash
name="Srushti Bhalekar"
```

---

# 38. Bash Arithmetic

Bash supports arithmetic using:

```bash
$(( ))
```

Example:

```bash
a=10
b=20

sum=$((a + b))

echo "Sum: $sum"
```

Output:

```text
Sum: 30
```

Other operations:

```bash
echo $((10 - 5))
echo $((10 * 5))
echo $((10 / 5))
echo $((10 % 3))
```

---

# 39. Mini Calculator Script

Create:

```bash
nano calculator.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter first number: " a
read -p "Enter second number: " b

echo "Addition: $((a + b))"
echo "Subtraction: $((a - b))"
echo "Multiplication: $((a * b))"
echo "Division: $((a / b))"
```

Run:

```bash
bash calculator.sh
```

Example:

```text
Enter first number: 20
Enter second number: 5
Addition: 25
Subtraction: 15
Multiplication: 100
Division: 4
```

---

# 40. Java Developer Example

Bash can automate Java compilation.

Create:

```bash
nano run-java.sh
```

Add:

```bash
#!/bin/bash

echo "Compiling Java program..."

javac Main.java

echo "Running Java program..."

java Main
```

Make executable:

```bash
chmod +x run-java.sh
```

Run:

```bash
./run-java.sh
```

This is a simple example of how Linux scripting can support Java development.

Later we will improve this with:

* Conditions
* Error handling
* Functions
* Loops
* Arguments
* Logging

---

# 41. Bash Script With Arguments

Create:

```bash
nano run-java.sh
```

Example:

```bash
#!/bin/bash

echo "Java file: $1"

javac "$1.java"

java "$1"
```

Run:

```bash
./run-java.sh Main
```

This expects:

```text
Main.java
```

and then runs:

```text
Main
```

---

# 42. Check Whether a Command Exists

You can use:

```bash
command -v java
```

Example script:

```bash
#!/bin/bash

if command -v java > /dev/null
then
    echo "Java is installed."
else
    echo "Java is not installed."
fi
```

Don't worry about the `if` syntax yet.

We will study Bash conditions in **Day 21**.

---

# 43. Script Execution Methods

There are several ways to execute a script.

### Method 1

```bash
bash script.sh
```

### Method 2

```bash
chmod +x script.sh
./script.sh
```

### Method 3

```bash
source script.sh
```

Be careful with `source`.

It runs the commands in your **current shell**, so variable changes can affect your current environment.

For ordinary scripts, prefer:

```bash
bash script.sh
```

or:

```bash
./script.sh
```

---

# 44. 🧪 Practice Lab 1 — Hello Script

Create:

```bash
mkdir -p ~/Day20-Lab
cd ~/Day20-Lab
nano hello.sh
```

Write:

```bash
#!/bin/bash

echo "Hello from Linux"
echo "I am learning Bash"
echo "Today is Day 20"
```

Run:

```bash
bash hello.sh
```

Then:

```bash
chmod +x hello.sh
./hello.sh
```

---

# 45. 🧪 Practice Lab 2 — User Information

Create:

```bash
nano user-info.sh
```

Add:

```bash
#!/bin/bash

echo "===== User Information ====="

echo "User: $USER"
echo "Home: $HOME"
echo "Shell: $SHELL"
echo "Current Directory: $PWD"
```

Run:

```bash
chmod +x user-info.sh
./user-info.sh
```

---

# 46. 🧪 Practice Lab 3 — Interactive Script

Create:

```bash
nano student.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter your name: " name
read -p "Enter your course: " course
read -p "Enter your skill: " skill

echo ""
echo "===== Student Information ====="
echo "Name: $name"
echo "Course: $course"
echo "Skill: $skill"
```

Run:

```bash
chmod +x student.sh
./student.sh
```

---

# 47. 🧪 Practice Lab 4 — Arguments

Create:

```bash
nano arguments.sh
```

Add:

```bash
#!/bin/bash

echo "Script Name: $0"
echo "First Argument: $1"
echo "Second Argument: $2"
echo "Third Argument: $3"
echo "Total Arguments: $#"
echo "All Arguments: $@"
```

Run:

```bash
chmod +x arguments.sh
./arguments.sh Java Linux Git
```

---

# 48. 🛠 Mini Project — Java Project Runner

Create:

```bash
mkdir -p ~/Day20-JavaRunner
cd ~/Day20-JavaRunner
```

Create Java program:

```bash
nano Main.java
```

Add:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Java application started.");
        System.out.println("Running from Linux Bash script.");
    }
}
```

Create Bash script:

```bash
nano run.sh
```

Add:

```bash
#!/bin/bash

echo "================================"
echo "       Java Project Runner"
echo "================================"

echo "Current Directory: $PWD"

echo ""
echo "Compiling Main.java..."

javac Main.java

echo ""
echo "Running Java application..."

java Main

echo ""
echo "Script finished."
```

Make executable:

```bash
chmod +x run.sh
```

Run:

```bash
./run.sh
```

Expected output:

```text
================================
       Java Project Runner
================================

Current Directory: /home/user/Day20-JavaRunner

Compiling Main.java...

Running Java application...

Java application started.
Running from Linux Bash script.

Script finished.
```

---

# 49. 🔥 Day 20 Challenge

Try these without looking at the previous examples.

### Challenge 1

Create:

```text
welcome.sh
```

It should print:

```text
Welcome to Linux
Welcome to Bash
Welcome to Java
```

---

### Challenge 2

Create a script that asks for:

```text
Name
Age
City
```

and displays them.

---

### Challenge 3

Create a script that accepts:

```text
Name
Skill
Experience
```

as command-line arguments.

Example:

```bash
./developer.sh Srushti Java Fresher
```

---

### Challenge 4

Display:

```text
Script name
Number of arguments
All arguments
```

using:

```text
$0
$#
$@
```

---

### Challenge 5

Create a calculator that accepts two numbers from the user and displays:

```text
Addition
Subtraction
Multiplication
Division
```

---

### Challenge 6

Create a script that displays:

```text
Current User
Current Directory
Hostname
Date
Java Version
```

---

# 50. 💼 Real-World Bash + Java Workflow

A simple Java developer workflow can look like:

```text
Bash Script
    ↓
Check Java
    ↓
Compile Java
    ↓
Run Application
    ↓
Generate Logs
    ↓
Backup / Deploy
```

This is why Bash is useful even when your main programming language is Java.

---

# 51. Common Beginner Mistakes

### Mistake 1 — Forgetting the shebang

Use:

```bash
#!/bin/bash
```

at the beginning of executable Bash scripts.

---

### Mistake 2 — Forgetting execute permission

If:

```bash
./script.sh
```

doesn't work because it isn't executable:

```bash
chmod +x script.sh
```

---

### Mistake 3 — Spaces around `=`

Wrong:

```bash
name = "Srushti"
```

Correct:

```bash
name="Srushti"
```

---

### Mistake 4 — Forgetting `$`

Wrong:

```bash
echo name
```

This prints:

```text
name
```

Correct:

```bash
echo "$name"
```

---

### Mistake 5 — Forgetting quotes

Prefer:

```bash
echo "$name"
```

instead of:

```bash
echo $name
```

especially when values may contain spaces or special characters.

---

# 52. 🎤 Interview Questions

### 1. What is Bash?

Bash is a Unix shell and command interpreter commonly used on Linux.

### 2. What is a Bash script?

A Bash script is a file containing Bash commands that can be executed together.

### 3. What is a shebang?

The shebang specifies the interpreter used to execute a script.

Example:

```bash
#!/bin/bash
```

### 4. How do you make a Bash script executable?

```bash
chmod +x script.sh
```

### 5. What is `$1`?

`$1` contains the first command-line argument passed to the script.

### 6. What is `$#`?

`$#` contains the number of positional arguments.

### 7. What is `$@`?

`$@` represents all positional arguments.

### 8. What is `$0`?

`$0` represents the script name/path used to invoke the script.

### 9. What is `$?`?

`$?` contains the exit status of the previous command.

### 10. What does exit status `0` normally mean?

It normally indicates successful execution.

### 11. How do you read user input?

Using:

```bash
read
```

Example:

```bash
read -p "Enter name: " name
```

### 12. How do you perform arithmetic in Bash?

Using:

```bash
$((expression))
```

Example:

```bash
sum=$((a + b))
```

---

# 53. ⚡ Quick Cheat Sheet

```bash
# Check Bash
bash --version

# Current shell
echo $SHELL

# Create script
nano script.sh

# Execute with Bash
bash script.sh

# Make executable
chmod +x script.sh

# Execute
./script.sh

# Variable
name="Srushti"

# Print variable
echo "$name"

# User input
read -p "Enter name: " name

# Command substitution
today=$(date)

# First argument
echo "$1"

# Second argument
echo "$2"

# Number of arguments
echo "$#"

# All arguments
echo "$@"

# Script name
echo "$0"

# Previous command status
echo "$?"

# Current process ID
echo "$$"

# Arithmetic
sum=$((10 + 20))

# Exit
exit 0
```

---

# 📁 Day 20 Repository Structure

```text
Java-Meets-Linux/
├── DAY19/
│   └── Environment-Variables.md
│
└── DAY20/
    └── Bash-Fundamentals.md
```

---

# 🚀 GitHub Push

From PowerShell:

```powershell
cd C:\Desktop\Java-Meets-Linux
```

Check:

```powershell
git status
```

Add Day 20:

```powershell
git add Day20\Bash-Fundamentals.md
```

Check:

```powershell
git status
```

Commit:

```powershell
git commit -m "docs: add day 20 bash fundamentals"
```

Push:

```powershell
git push
```

Finally:

```powershell
git status
```

You should see:

```text
Your branch is up to date with 'origin/main'.
```

---

# 🎯 Day 20 Complete

Today you learned:

```text
Bash
 ↓
Shell
 ↓
Bash Script
 ↓
Shebang
 ↓
Variables
 ↓
echo
 ↓
read
 ↓
Command Substitution
 ↓
Arguments
 ↓
$0 $1 $2
 ↓
$# $@
 ↓
$?
 ↓
Exit Status
 ↓
Bash + Java Automation
```

## 🔜 Day 21 — Bash Conditions

Next you will learn:

* `if`
* `else`
* `elif`
* `[ ]`
* `[[ ]]`
* String comparisons
* Number comparisons
* File conditions
* `-f`
* `-d`
* `-e`
* `-r`
* `-w`
* `-x`
* Logical operators
* `&&`
* `||`
* Practical Bash projects
* Java + Bash condition checks
