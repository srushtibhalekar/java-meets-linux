# 🐧 Day 24 — Bash Scripting

## 🎯 Learning Objectives

By the end of Day 24, you will understand:

* What Bash scripting is
* Bash script structure
* Shebang
* Variables
* User input
* Command-line arguments
* Conditions
* Loops
* Functions
* File handling
* Exit codes
* Error handling
* Debugging
* Script permissions
* Real-world automation
* Building a complete Bash automation project

---

# 1. What Is Bash Scripting?

A Bash script is a text file containing Linux commands that Bash executes in sequence.

Instead of manually running:

```bash
pwd
whoami
date
df -h
free -h
```

you can put them into one script:

```bash
#!/bin/bash

pwd
whoami
date
df -h
free -h
```

Then run:

```bash
./script.sh
```

This is **automation**.

---

# 2. Why Use Bash Scripts?

Bash scripting is useful for:

* System administration
* Server automation
* Backup
* Log analysis
* Deployment
* Monitoring
* File management
* Application startup
* Application shutdown
* Scheduled tasks
* DevOps
* Linux administration

For a Java developer, Bash can automate:

```text
Compile Java
    ↓
Run tests
    ↓
Build application
    ↓
Create backup
    ↓
Check logs
    ↓
Start application
```

---

# 3. Creating Your First Bash Script

Create:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello from Bash!"
echo "Welcome to Linux scripting."
```

Save the file.

Make it executable:

```bash
chmod +x hello.sh
```

Run:

```bash
./hello.sh
```

Output:

```text
Hello from Bash!
Welcome to Linux scripting.
```

---

# 4. The Shebang

The first line:

```bash
#!/bin/bash
```

is called the **shebang**.

It tells the operating system to use Bash to execute the script.

Example:

```bash
#!/bin/bash

echo "Hello"
```

The shebang should normally be the first line.

---

# 5. Running a Script With Bash

You can run:

```bash
bash hello.sh
```

even if the file is not executable.

You can also run:

```bash
./hello.sh
```

after:

```bash
chmod +x hello.sh
```

### Difference

```bash
bash hello.sh
```

Explicitly starts Bash to execute the file.

```bash
./hello.sh
```

executes the script directly using the interpreter specified by the shebang.

---

# 6. Comments

Comments are ignored by Bash.

Single-line comment:

```bash
# This is a comment
```

Example:

```bash
#!/bin/bash

# Display current user
whoami

# Display current directory
pwd
```

Comments make scripts easier to understand.

---

# 7. Variables

Create a variable:

```bash
name="Srushti"
```

Use it:

```bash
echo "$name"
```

Output:

```text
Srushti
```

### Important

Do not put spaces around `=`.

Wrong:

```bash
name = "Srushti"
```

Correct:

```bash
name="Srushti"
```

---

# 8. Multiple Variables

```bash
name="Srushti"
role="Java Developer"
language="Java"

echo "Name: $name"
echo "Role: $role"
echo "Language: $language"
```

Output:

```text
Name: Srushti
Role: Java Developer
Language: Java
```

---

# 9. Command Substitution

You can store command output in a variable.

```bash
current_user=$(whoami)
current_directory=$(pwd)
current_date=$(date)
```

Display:

```bash
echo "User: $current_user"
echo "Directory: $current_directory"
echo "Date: $current_date"
```

---

# 10. Environment Variables

Some variables already exist.

Check:

```bash
echo "$HOME"
echo "$USER"
echo "$SHELL"
echo "$PATH"
```

You can also use:

```bash
printenv
```

---

# 11. User Input

Use:

```bash
read
```

Example:

```bash
#!/bin/bash

read -p "Enter your name: " name

echo "Hello $name"
```

Run:

```bash
bash hello.sh
```

Example:

```text
Enter your name: Srushti
Hello Srushti
```

---

# 12. Reading Multiple Values

```bash
read -p "Enter first name: " first
read -p "Enter city: " city

echo "Name: $first"
echo "City: $city"
```

---

# 13. Read Password

Use:

```bash
read -s -p "Enter password: " password
echo
```

The `-s` option hides the input while typing.

Do not store real passwords in scripts or commit them to Git.

---

# 14. Arithmetic

Bash supports integer arithmetic.

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

Operations:

```text
+   Addition
-   Subtraction
*   Multiplication
/   Division
%   Modulus
```

---

# 15. Conditions

Basic structure:

```bash
if [ condition ]; then
    commands
fi
```

Example:

```bash
age=22

if [ "$age" -ge 18 ]; then
    echo "Adult"
fi
```

---

# 16. `if-else`

```bash
age=17

if [ "$age" -ge 18 ]; then
    echo "Adult"
else
    echo "Minor"
fi
```

---

# 17. `if-elif-else`

```bash
marks=75

if [ "$marks" -ge 90 ]; then
    echo "Grade A+"
elif [ "$marks" -ge 75 ]; then
    echo "Grade A"
elif [ "$marks" -ge 60 ]; then
    echo "Grade B"
else
    echo "Needs Improvement"
fi
```

---

# 18. Numeric Comparison Operators

| Operator | Meaning               |
| -------- | --------------------- |
| `-eq`    | Equal                 |
| `-ne`    | Not equal             |
| `-gt`    | Greater than          |
| `-ge`    | Greater than or equal |
| `-lt`    | Less than             |
| `-le`    | Less than or equal    |

Example:

```bash
if [ "$a" -gt "$b" ]; then
    echo "A is greater"
fi
```

---

# 19. String Comparison

Example:

```bash
name="Srushti"

if [ "$name" = "Srushti" ]; then
    echo "Name matched"
fi
```

Not equal:

```bash
if [ "$name" != "Rahul" ]; then
    echo "Names are different"
fi
```

---

# 20. File Conditions

Bash can check files and directories.

| Test | Meaning           |
| ---- | ----------------- |
| `-f` | Regular file      |
| `-d` | Directory         |
| `-e` | Exists            |
| `-r` | Readable          |
| `-w` | Writable          |
| `-x` | Executable        |
| `-s` | File is not empty |

Example:

```bash
if [ -f "data.txt" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

---

# 21. Checking a Directory

```bash
if [ -d "project" ]; then
    echo "Project directory exists."
else
    echo "Project directory does not exist."
fi
```

---

# 22. Logical Operators

### AND

```bash
if [ "$age" -ge 18 ] && [ "$age" -le 60 ]; then
    echo "Working age range"
fi
```

### OR

```bash
if [ "$name" = "Java" ] || [ "$name" = "Bash" ]; then
    echo "Known technology"
fi
```

### NOT

```bash
if [ ! -f "test.txt" ]; then
    echo "File does not exist"
fi
```

---

# 23. `case` Statement

`case` is useful for menus.

```bash
choice=2

case "$choice" in
    1)
        echo "Java"
        ;;
    2)
        echo "Linux"
        ;;
    3)
        echo "Git"
        ;;
    *)
        echo "Invalid choice"
        ;;
esac
```

---

# 24. Loops

Bash supports:

* `for`
* `while`
* `until`

### For loop

```bash
for i in {1..5}
do
    echo "$i"
done
```

Output:

```text
1
2
3
4
5
```

---

# 25. Loop Through Files

```bash
for file in *.java
do
    echo "Java file: $file"
done
```

This is useful for Java project automation.

---

# 26. While Loop

```bash
count=1

while [ "$count" -le 5 ]
do
    echo "$count"
    ((count++))
done
```

---

# 27. Reading a File Line by Line

A safe common pattern is:

```bash
while IFS= read -r line
do
    echo "$line"
done < names.txt
```

This is useful for:

* Log files
* Configuration files
* Lists of servers
* User lists
* Java source files

---

# 28. Functions

A script can be divided into functions.

```bash
show_info() {
    echo "User: $(whoami)"
    echo "Directory: $(pwd)"
}

show_info
```

Functions make large scripts easier to maintain.

---

# 29. Function With Arguments

```bash
greet() {

    local name=$1

    echo "Hello $name"
}

greet "Srushti"
```

Output:

```text
Hello Srushti
```

---

# 30. Command-Line Arguments

Suppose the script is:

```bash
#!/bin/bash

echo "First argument: $1"
echo "Second argument: $2"
echo "Total arguments: $#"
```

Run:

```bash
bash args.sh Java Linux
```

Output:

```text
First argument: Java
Second argument: Linux
Total arguments: 2
```

---

# 31. Important Script Variables

| Variable | Meaning              |
| -------- | -------------------- |
| `$0`     | Script name          |
| `$1`     | First argument       |
| `$2`     | Second argument      |
| `$#`     | Number of arguments  |
| `$@`     | All arguments        |
| `$?`     | Previous exit status |
| `$$`     | Current process PID  |

Example:

```bash
echo "Script: $0"
echo "Arguments: $#"
```

---

# 32. Exit Codes

Every command returns an exit status.

Usually:

```text
0 = Success
Non-zero = Failure
```

Example:

```bash
ls
echo $?
```

If successful:

```text
0
```

Test a failed command:

```bash
ls /directory-that-does-not-exist
echo $?
```

The result will normally be non-zero.

---

# 33. Using `exit`

You can stop a script with:

```bash
exit 0
```

Success:

```bash
exit 0
```

Failure:

```bash
exit 1
```

Example:

```bash
if [ ! -f "$1" ]; then
    echo "File not found."
    exit 1
fi
```

---

# 34. Error Handling

A basic script can check every important operation.

```bash
mkdir project

if [ $? -eq 0 ]; then
    echo "Directory created."
else
    echo "Failed to create directory."
    exit 1
fi
```

A shorter approach:

```bash
if mkdir project; then
    echo "Directory created."
else
    echo "Failed to create directory."
fi
```

---

# 35. `set -e`

You can make Bash stop when a command fails:

```bash
#!/bin/bash

set -e

echo "Starting..."
mkdir project
echo "Continuing..."
```

If an important command fails, the script can stop instead of continuing blindly.

Use this carefully because some commands intentionally return non-zero statuses during normal logic.

---

# 36. `set -u`

```bash
set -u
```

This helps catch references to unset variables.

Example:

```bash
#!/bin/bash

set -u

echo "$undefined_variable"
```

This can reveal variable mistakes early.

---

# 37. `set -x`

```bash
set -x
```

It displays commands as Bash executes them.

Example:

```bash
#!/bin/bash

set -x

name="Srushti"
echo "$name"
```

Useful for debugging.

You can also run:

```bash
bash -x script.sh
```

without modifying the script.

---

# 38. Safe Script Starting Point

A common robust starting point is:

```bash
#!/bin/bash

set -euo pipefail
```

Meaning:

```text
-e → stop on many command failures
-u → detect unset variables
-o pipefail → detect failures inside pipelines
```

For beginner scripts, understand each option before using it in complex automation.

---

# 39. File Creation

Create a file:

```bash
touch output.txt
```

Write:

```bash
echo "Hello Linux" > output.txt
```

Append:

```bash
echo "Second line" >> output.txt
```

Read:

```bash
cat output.txt
```

---

# 40. Creating a Backup

Example:

```bash
#!/bin/bash

source="project"
backup="project-backup.tar.gz"

if [ ! -d "$source" ]; then
    echo "Source directory not found."
    exit 1
fi

tar -czf "$backup" "$source"

echo "Backup created: $backup"
```

---

# 41. Log Analysis Script

Example:

```bash
#!/bin/bash

log_file="$1"

if [ ! -f "$log_file" ]; then
    echo "Log file not found."
    exit 1
fi

echo "===== LOG REPORT ====="

echo "Total lines:"
wc -l "$log_file"

echo
echo "ERROR count:"
grep -ic "error" "$log_file"

echo
echo "WARNING count:"
grep -ic "warning" "$log_file"
```

Run:

```bash
bash log-report.sh application.log
```

---

# 42. Java Project Automation

Suppose your project contains:

```text
project/
├── src/
│   └── Main.java
└── out/
```

A simple script can compile Java.

```bash
#!/bin/bash

mkdir -p out

javac -d out src/Main.java

if [ $? -eq 0 ]; then
    echo "Compilation successful."
else
    echo "Compilation failed."
    exit 1
fi
```

---

# 43. Compile and Run Java

```bash
#!/bin/bash

mkdir -p out

javac -d out src/Main.java

if [ $? -ne 0 ]; then
    echo "Compilation failed."
    exit 1
fi

echo "Compilation successful."

java -cp out Main
```

This is a simple example of Bash automating a Java workflow.

---

# 44. Bash Script With Menu

Example:

```bash
#!/bin/bash

show_menu() {
    echo
    echo "=============================="
    echo "       LINUX UTILITY"
    echo "=============================="
    echo "1. System Information"
    echo "2. Disk Usage"
    echo "3. Memory Usage"
    echo "4. Current User"
    echo "5. Exit"
    echo "=============================="
}

system_info() {
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Architecture: $(uname -m)"
}

disk_usage() {
    df -h /
}

memory_usage() {
    free -h
}

while true
do

    show_menu

    read -p "Enter choice: " choice

    case "$choice" in

        1)
            system_info
            ;;

        2)
            disk_usage
            ;;

        3)
            memory_usage
            ;;

        4)
            echo "Current user: $(whoami)"
            ;;

        5)
            echo "Goodbye!"
            exit 0
            ;;

        *)
            echo "Invalid choice."
            ;;

    esac

done
```

This combines almost everything learned so far.

---

# 🧪 45. Practice Lab 1 — System Information Script

Create:

```text
system-info.sh
```

Display:

```text
User
Hostname
Kernel
Architecture
Current directory
Date
```

Commands you can use:

```bash
whoami
hostname
uname -r
uname -m
pwd
date
```

---

# 🧪 46. Practice Lab 2 — File Checker

Create:

```text
file-checker.sh
```

Accept a filename:

```bash
bash file-checker.sh test.txt
```

The script should display:

```text
File exists
```

or:

```text
File does not exist
```

If the file exists, also display:

* File size
* Number of lines
* Read permission
* Write permission

---

# 🧪 47. Practice Lab 3 — Java Project Checker

Create:

```text
java-checker.sh
```

Check:

```text
Java
Javac
Git
Maven
```

Use:

```bash
command -v java
command -v javac
command -v git
command -v mvn
```

Display whether each tool is installed.

---

# 🧪 48. Practice Lab 4 — Backup Script

Create:

```text
backup.sh
```

Accept:

```bash
bash backup.sh project
```

The script should:

1. Check whether the directory exists.
2. Create a timestamp.
3. Create a `.tar.gz` backup.
4. Display the backup filename.
5. Return an appropriate exit code.

Example timestamp:

```bash
timestamp=$(date +"%Y%m%d_%H%M%S")
```

---

# 🚀 49. Mini Project — Linux Developer Automation Tool

Create:

```text
linux-dev-tool.sh
```

Menu:

```text
========================================
       LINUX DEVELOPER TOOL
========================================

1. System Information
2. Check Java
3. Check Git
4. Check Maven
5. Check Disk
6. Check Memory
7. Find Java Files
8. Search Errors in Log
9. Backup Project
10. Exit

Enter your choice:
```

Create separate functions:

```text
show_system_info()
check_java()
check_git()
check_maven()
check_disk()
check_memory()
find_java_files()
search_errors()
backup_project()
```

---

# 50. Suggested Project Structure

```text
Day24-Project/
├── linux-dev-tool.sh
├── README.md
└── sample.log
```

---

# 51. Example `check_java()` Function

```bash
check_java() {

    echo "===== JAVA ====="

    if command -v java >/dev/null 2>&1; then
        echo "Java is installed."
        java -version
    else
        echo "Java is not installed."
        return 1
    fi
}
```

---

# 52. Example `check_maven()` Function

```bash
check_maven() {

    echo "===== MAVEN ====="

    if command -v mvn >/dev/null 2>&1; then
        echo "Maven is installed."
        mvn -version
    else
        echo "Maven is not installed."
        return 1
    fi
}
```

---

# 53. Example Java File Search

```bash
find_java_files() {

    echo "===== JAVA FILES ====="

    find . -type f -name "*.java"
}
```

---

# 54. Example Error Search

```bash
search_errors() {

    local log_file=$1

    if [ ! -f "$log_file" ]; then
        echo "Log file not found."
        return 1
    fi

    grep -in "error" "$log_file"
}
```

Call:

```bash
search_errors application.log
```

---

# 55. Example Backup Function

```bash
backup_project() {

    local project=$1

    if [ ! -d "$project" ]; then
        echo "Project directory not found."
        return 1
    fi

    local timestamp
    timestamp=$(date +"%Y%m%d_%H%M%S")

    local backup="${project}_${timestamp}.tar.gz"

    tar -czf "$backup" "$project"

    echo "Backup created: $backup"
}
```

---

# 56. Debugging Bash Scripts

### Syntax check

```bash
bash -n script.sh
```

This checks syntax without running the script.

### Debug execution

```bash
bash -x script.sh
```

### Show Bash version

```bash
bash --version
```

---

# 57. Common Bash Errors

### Permission denied

```text
Permission denied
```

Fix:

```bash
chmod +x script.sh
```

---

### Command not found

Example:

```text
java: command not found
```

Check:

```bash
command -v java
```

---

### Variable mistake

Wrong:

```bash
echo "$username"
```

when the variable was actually:

```bash
user_name="Srushti"
```

Correct:

```bash
echo "$user_name"
```

---

### Missing argument

Always validate important arguments.

```bash
if [ "$#" -lt 1 ]; then
    echo "Usage: $0 filename"
    exit 1
fi
```

---

# 58. Bash Script Best Practices

Follow these habits:

### 1. Use a shebang

```bash
#!/bin/bash
```

### 2. Quote variables

Prefer:

```bash
"$file"
```

### 3. Validate input

```bash
if [ "$#" -lt 1 ]; then
    ...
fi
```

### 4. Use functions

Keep related logic together.

### 5. Use meaningful names

Good:

```bash
backup_project
check_disk
search_errors
```

Avoid unclear names such as:

```bash
x
abc
temp1
```

### 6. Never hard-code passwords

Do not commit secrets to GitHub.

### 7. Test before automating important operations

Especially:

```bash
rm
mv
chown
chmod
```

---

# 🎯 59. Day 24 Challenge

Build a script:

```text
java-linux-manager.sh
```

It should provide:

```text
1. Show system information
2. Check Java
3. Check Git
4. Check Maven
5. Find Java files
6. Count Java files
7. Search errors
8. Show disk usage
9. Backup project
10. Exit
```

Requirements:

* Use functions.
* Use a menu.
* Use `case`.
* Use at least one loop.
* Validate user input.
* Use command-line arguments for the project path.
* Return meaningful exit codes.
* Handle missing files/directories.
* Do not use hard-coded passwords.
* Keep the code organized.

---

# ❓ 60. Interview Questions

### Q1. What is Bash scripting?

Bash scripting is writing a sequence of Linux commands in a script file so they can be executed and automated.

### Q2. What is a shebang?

The shebang specifies the interpreter used to execute a script.

Example:

```bash
#!/bin/bash
```

### Q3. How do you make a Bash script executable?

```bash
chmod +x script.sh
```

### Q4. How do you run a Bash script?

```bash
./script.sh
```

or:

```bash
bash script.sh
```

### Q5. What is `$1`?

The first command-line argument or positional parameter.

### Q6. What is `$#`?

The number of positional arguments.

### Q7. What is `$@`?

All positional arguments.

### Q8. What does `$?` represent?

The exit status of the previously executed command.

### Q9. What does exit code `0` normally mean?

Successful execution.

### Q10. What does `exit 1` do?

Terminates the script with exit status `1`, commonly indicating an error.

### Q11. How do you debug a Bash script?

```bash
bash -x script.sh
```

### Q12. How do you check Bash syntax without executing the script?

```bash
bash -n script.sh
```

### Q13. What is command substitution?

It allows command output to be stored or used as a value.

Example:

```bash
current_date=$(date)
```

### Q14. Why are functions useful?

They make scripts reusable, modular, easier to read, and easier to maintain.

### Q15. What is the difference between `echo` and `return` in a function?

`echo` produces output that can be captured.

`return` sets the function's exit status.

---

# 📌 61. Day 24 Cheat Sheet

| Syntax / Command   | Purpose                  |
| ------------------ | ------------------------ |
| `#!/bin/bash`      | Bash interpreter         |
| `chmod +x file.sh` | Make executable          |
| `./file.sh`        | Run executable script    |
| `bash file.sh`     | Run with Bash            |
| `read`             | Read user input          |
| `$0`               | Script name              |
| `$1`               | First argument           |
| `$#`               | Argument count           |
| `"$@"`             | All arguments            |
| `$?`               | Exit status              |
| `exit 0`           | Successful exit          |
| `exit 1`           | Error exit               |
| `if`               | Conditional logic        |
| `case`             | Multiple-choice logic    |
| `for`              | Loop                     |
| `while`            | Loop                     |
| `function()`       | Define function          |
| `$(command)`       | Command substitution     |
| `> file`           | Overwrite output         |
| `>> file`          | Append output            |
| `bash -n`          | Syntax check             |
| `bash -x`          | Debug                    |
| `set -e`           | Stop on many errors      |
| `set -u`           | Detect unset variables   |
| `set -o pipefail`  | Detect pipeline failures |

---



