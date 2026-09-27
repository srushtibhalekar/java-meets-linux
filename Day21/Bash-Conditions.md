# 🐚 Day 21 — Bash Conditions

Bash conditions allow a script to **make decisions**.

For example:

```text
If Java is installed
    ↓
Run Java application

Otherwise
    ↓
Show an error message
```

Conditions are essential for:

* Automation
* Error handling
* File checking
* User input validation
* Server administration
* Deployment scripts
* Java application management

---

# 🎯 Learning Objectives

By the end of Day 21, you will understand:

* `if`
* `else`
* `elif`
* `fi`
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
* `!`
* Exit status
* Java installation checks
* Practical Bash condition scripts

---

# 1. Why Do We Need Conditions?

Without conditions, a script simply executes commands one after another.

With conditions, it can make decisions.

Example:

```text
User enters age
       ↓
Is age >= 18?
   ↙       ↘
 Yes        No
  ↓          ↓
Adult      Minor
```

---

# 2. Basic `if` Syntax

The basic Bash syntax is:

```bash
if [ condition ]
then
    commands
fi
```

Example:

```bash
#!/bin/bash

if [ "$USER" = "root" ]
then
    echo "You are root."
fi
```

Notice:

```text
if
condition
then
commands
fi
```

`fi` marks the end of the `if` block.

---

# 3. Important Spaces in `[ ]`

This is correct:

```bash
if [ "$name" = "Srushti" ]
```

This is incorrect:

```bash
if ["$name" = "Srushti"]
```

There must be spaces:

```text
[ condition ]
```

because `[` is treated as a command in traditional Bash test syntax.

---

# 4. Simple String Condition

Create:

```bash
nano check-name.sh
```

Add:

```bash
#!/bin/bash

name="Srushti"

if [ "$name" = "Srushti" ]
then
    echo "Name matched."
fi
```

Run:

```bash
bash check-name.sh
```

Output:

```text
Name matched.
```

---

# 5. `else`

Use `else` when you need an alternative.

Syntax:

```bash
if [ condition ]
then
    commands
else
    commands
fi
```

Example:

```bash
#!/bin/bash

name="Rahul"

if [ "$name" = "Srushti" ]
then
    echo "Name matched."
else
    echo "Name did not match."
fi
```

Output:

```text
Name did not match.
```

---

# 6. User Input With `if`

Create:

```bash
nano name-check.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter your name: " name

if [ "$name" = "Srushti" ]
then
    echo "Welcome Srushti!"
else
    echo "Welcome $name!"
fi
```

Run:

```bash
chmod +x name-check.sh
./name-check.sh
```

---

# 7. `elif`

`elif` means:

```text
else if
```

Syntax:

```bash
if [ condition1 ]
then
    commands
elif [ condition2 ]
then
    commands
else
    commands
fi
```

Example:

```bash
#!/bin/bash

read -p "Enter your score: " score

if [ "$score" -ge 90 ]
then
    echo "Grade A"
elif [ "$score" -ge 75 ]
then
    echo "Grade B"
elif [ "$score" -ge 60 ]
then
    echo "Grade C"
else
    echo "Needs improvement"
fi
```

---

# 8. Number Comparisons

For numbers, Bash uses special operators.

| Operator | Meaning               |
| -------- | --------------------- |
| `-eq`    | Equal                 |
| `-ne`    | Not equal             |
| `-gt`    | Greater than          |
| `-ge`    | Greater than or equal |
| `-lt`    | Less than             |
| `-le`    | Less than or equal    |

---

# 9. `-eq`

Equal to:

```bash
if [ "$a" -eq "$b" ]
```

Example:

```bash
a=10
b=10

if [ "$a" -eq "$b" ]
then
    echo "Numbers are equal."
fi
```

---

# 10. `-ne`

Not equal:

```bash
if [ "$a" -ne "$b" ]
then
    echo "Numbers are different."
fi
```

---

# 11. `-gt`

Greater than:

```bash
if [ "$age" -gt 18 ]
then
    echo "Age is greater than 18."
fi
```

---

# 12. `-ge`

Greater than or equal:

```bash
if [ "$age" -ge 18 ]
then
    echo "Adult."
fi
```

---

# 13. `-lt`

Less than:

```bash
if [ "$age" -lt 18 ]
then
    echo "Minor."
fi
```

---

# 14. `-le`

Less than or equal:

```bash
if [ "$age" -le 18 ]
then
    echo "Age is 18 or below."
fi
```

---

# 15. Complete Age Checker

Create:

```bash
nano age-check.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter your age: " age

if [ "$age" -ge 18 ]
then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

Run:

```bash
chmod +x age-check.sh
./age-check.sh
```

---

# 16. String Comparisons

Common string operators:

| Operator | Meaning             |
| -------- | ------------------- |
| `=`      | Equal               |
| `!=`     | Not equal           |
| `-z`     | String is empty     |
| `-n`     | String is not empty |

Example:

```bash
if [ "$name" = "Srushti" ]
then
    echo "Matched"
fi
```

---

# 17. String Not Equal

```bash
if [ "$name" != "Srushti" ]
then
    echo "Different name."
fi
```

---

# 18. Check Empty String

Use:

```bash
-z
```

Example:

```bash
if [ -z "$name" ]
then
    echo "Name is empty."
fi
```

---

# 19. Check Non-Empty String

Use:

```bash
-n
```

Example:

```bash
if [ -n "$name" ]
then
    echo "Name was entered."
fi
```

---

# 20. Input Validation

A useful example:

```bash
#!/bin/bash

read -p "Enter your name: " name

if [ -z "$name" ]
then
    echo "Name cannot be empty."
else
    echo "Hello $name"
fi
```

This is one of the most common uses of conditions.

---

# 21. File Conditions

Bash can check whether files and directories exist.

Important operators:

| Operator | Meaning                      |
| -------- | ---------------------------- |
| `-e`     | Exists                       |
| `-f`     | Regular file                 |
| `-d`     | Directory                    |
| `-r`     | Readable                     |
| `-w`     | Writable                     |
| `-x`     | Executable                   |
| `-s`     | File exists and is not empty |
| `-L`     | Symbolic link                |

---

# 22. Check Whether a File Exists

```bash
if [ -e "test.txt" ]
then
    echo "File exists."
else
    echo "File does not exist."
fi
```

---

# 23. Check Regular File

Use:

```bash
-f
```

Example:

```bash
if [ -f "test.txt" ]
then
    echo "This is a regular file."
fi
```

---

# 24. Check Directory

Use:

```bash
-d
```

Example:

```bash
if [ -d "Day21" ]
then
    echo "Directory exists."
fi
```

---

# 25. Check Read Permission

```bash
if [ -r "test.txt" ]
then
    echo "File is readable."
fi
```

---

# 26. Check Write Permission

```bash
if [ -w "test.txt" ]
then
    echo "File is writable."
fi
```

---

# 27. Check Execute Permission

```bash
if [ -x "script.sh" ]
then
    echo "Script is executable."
fi
```

---

# 28. Check Whether File Is Empty

Use:

```bash
-s
```

Example:

```bash
if [ -s "application.log" ]
then
    echo "Log file contains data."
else
    echo "Log file is empty or does not exist."
fi
```

---

# 29. File Validation Script

Create:

```bash
nano file-check.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter file name: " file

if [ -f "$file" ]
then
    echo "File exists."
else
    echo "File does not exist."
fi
```

Run:

```bash
chmod +x file-check.sh
./file-check.sh
```

---

# 30. Directory Validation

Create:

```bash
nano directory-check.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter directory name: " directory

if [ -d "$directory" ]
then
    echo "Directory exists."
else
    echo "Directory does not exist."
fi
```

---

# 31. Check Multiple Conditions

You can use logical operators.

### AND

```text
condition1 AND condition2
```

Both conditions must be true.

### OR

```text
condition1 OR condition2
```

At least one condition must be true.

---

# 32. Using `&&`

Example:

```bash
if [ "$age" -ge 18 ] && [ "$age" -le 60 ]
then
    echo "Age is between 18 and 60."
fi
```

Both conditions must be true.

---

# 33. Using `||`

Example:

```bash
if [ "$role" = "developer" ] || [ "$role" = "admin" ]
then
    echo "Authorized role."
fi
```

At least one condition must be true.

---

# 34. Using `!`

`!` means NOT.

Example:

```bash
if [ ! -f "test.txt" ]
then
    echo "File does not exist."
fi
```

---

# 35. `[[ ]]`

Bash provides an enhanced conditional syntax:

```bash
[[ condition ]]
```

Example:

```bash
if [[ "$name" == "Srushti" ]]
then
    echo "Matched"
fi
```

For Bash scripts, `[[ ]]` is generally safer and more flexible than `[ ]`.

---

# 36. `[ ]` vs `[[ ]]`

Traditional:

```bash
if [ "$name" = "Srushti" ]
```

Bash-specific:

```bash
if [[ "$name" == "Srushti" ]]
```

For this roadmap, learn both.

A useful beginner rule:

```text
[ ]    → traditional test syntax

[[ ]]  → Bash conditional syntax
```

---

# 37. `==` With `[[ ]]`

Inside `[[ ]]`, you can use:

```bash
if [[ "$name" == "Srushti" ]]
then
    echo "Matched"
fi
```

With `[ ]`, prefer:

```bash
if [ "$name" = "Srushti" ]
```

---

# 38. Pattern Matching With `[[ ]]`

`[[ ]]` supports pattern matching.

Example:

```bash
name="Srushti"

if [[ "$name" == Sru* ]]
then
    echo "Name starts with Sru."
fi
```

Here:

```text
*
```

matches additional characters.

---

# 39. Case-Insensitive Comparison

Bash's `[[ ]]` can also use pattern techniques, but for beginners a simple approach is to normalize input.

For example:

```bash
name="JAVA"
name=${name,,}

if [[ "$name" == "java" ]]
then
    echo "Java selected."
fi
```

Output:

```text
Java selected.
```

`${name,,}` converts the string to lowercase in Bash.

---

# 40. Numeric Conditions With `(( ))`

For arithmetic comparisons, Bash also supports:

```bash
(( ))
```

Example:

```bash
age=22

if (( age >= 18 ))
then
    echo "Adult"
fi
```

This is convenient for numeric expressions.

---

# 41. Example: Even or Odd

Create:

```bash
nano even-odd.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter a number: " number

if (( number % 2 == 0 ))
then
    echo "Even number."
else
    echo "Odd number."
fi
```

Run:

```bash
chmod +x even-odd.sh
./even-odd.sh
```

---

# 42. Example: Positive, Negative, Zero

```bash
#!/bin/bash

read -p "Enter a number: " number

if (( number > 0 ))
then
    echo "Positive number."
elif (( number < 0 ))
then
    echo "Negative number."
else
    echo "Zero."
fi
```

---

# 43. Check Java Installation

This is very useful for Java developers.

Create:

```bash
nano java-check.sh
```

Add:

```bash
#!/bin/bash

if command -v java > /dev/null 2>&1
then
    echo "Java is installed."
    java --version
else
    echo "Java is not installed."
fi
```

Run:

```bash
chmod +x java-check.sh
./java-check.sh
```

---

# 44. Understanding `> /dev/null 2>&1`

This part:

```bash
command -v java > /dev/null 2>&1
```

means:

```text
stdout → /dev/null
stderr → stdout
```

So the command's output is hidden.

We only care whether the command succeeds.

Then the `if` checks its exit status.

---

# 45. Check Java Compiler

```bash
if command -v javac > /dev/null 2>&1
then
    echo "Java compiler is available."
else
    echo "javac is not available."
fi
```

---

# 46. Check Git

```bash
if command -v git > /dev/null 2>&1
then
    echo "Git is installed."
else
    echo "Git is not installed."
fi
```

---

# 47. Check Multiple Developer Tools

Create:

```bash
nano tools-check.sh
```

Add:

```bash
#!/bin/bash

echo "===== Developer Tools Check ====="

if command -v java > /dev/null 2>&1
then
    echo "Java: Installed"
else
    echo "Java: Not Installed"
fi

if command -v git > /dev/null 2>&1
then
    echo "Git: Installed"
else
    echo "Git: Not Installed"
fi

if command -v curl > /dev/null 2>&1
then
    echo "cURL: Installed"
else
    echo "cURL: Not Installed"
fi

if command -v bash > /dev/null 2>&1
then
    echo "Bash: Installed"
else
    echo "Bash: Not Installed"
fi
```

Run:

```bash
chmod +x tools-check.sh
./tools-check.sh
```

---

# 48. Check Java Version

You can capture Java version:

```bash
java_version=$(java --version 2>&1 | head -n 1)

echo "$java_version"
```

Then:

```bash
if command -v java > /dev/null 2>&1
then
    echo "Java detected."
    java --version
else
    echo "Java not detected."
fi
```

---

# 49. Check File Before Reading

Suppose your Java application has a log file:

```text
application.log
```

Use:

```bash
if [ -f "application.log" ]
then
    echo "Log file found."
    cat application.log
else
    echo "Log file not found."
fi
```

This prevents the script from blindly trying to read a missing file.

---

# 50. Check Directory Before Creating

```bash
if [ -d "logs" ]
then
    echo "Logs directory already exists."
else
    mkdir logs
    echo "Logs directory created."
fi
```

---

# 51. Practical Log Backup Example

```bash
#!/bin/bash

if [ -f "application.log" ]
then
    cp application.log application.log.backup
    echo "Log backup created."
else
    echo "application.log not found."
fi
```

This is a simple example of real-world automation.

---

# 52. Nested Conditions

Conditions can be placed inside other conditions.

Example:

```bash
#!/bin/bash

if [ -f "application.log" ]
then

    if [ -s "application.log" ]
    then
        echo "Log file exists and contains data."
    else
        echo "Log file exists but is empty."
    fi

else
    echo "Log file does not exist."
fi
```

You do not need to use nested conditions everywhere, but they are useful when decisions depend on previous checks.

---

# 53. Combining File and Command Checks

Example:

```bash
#!/bin/bash

if command -v java > /dev/null 2>&1
then

    if [ -f "Main.java" ]
    then
        echo "Java and Main.java are available."
    else
        echo "Java is installed, but Main.java is missing."
    fi

else
    echo "Java is not installed."
fi
```

---

# 54. Exit Status With Conditions

Remember from Day 20:

```bash
echo $?
```

A successful command normally returns:

```text
0
```

A failed command normally returns:

```text
non-zero
```

Bash `if` can directly evaluate command success.

Example:

```bash
if command -v java > /dev/null 2>&1
then
    echo "Java exists."
else
    echo "Java does not exist."
fi
```

The `if` is checking the command's exit status.

---

# 55. `&&` Outside `if`

You can also write:

```bash
command -v java > /dev/null 2>&1 && echo "Java installed"
```

Meaning:

```text
If command succeeds
        ↓
run echo
```

---

# 56. `||` Outside `if`

```bash
command -v java > /dev/null 2>&1 || echo "Java missing"
```

Meaning:

```text
If command fails
        ↓
run echo
```

This is useful for short checks.

For complex logic, `if` is easier to read.

---

# 57. 🧪 Practice Lab — Student Grade Checker

Create:

```bash
mkdir -p ~/Day21-Lab
cd ~/Day21-Lab
nano grade.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter your marks: " marks

if [ "$marks" -ge 90 ]
then
    echo "Grade A"
elif [ "$marks" -ge 75 ]
then
    echo "Grade B"
elif [ "$marks" -ge 60 ]
then
    echo "Grade C"
elif [ "$marks" -ge 40 ]
then
    echo "Grade D"
else
    echo "Fail"
fi
```

Run:

```bash
chmod +x grade.sh
./grade.sh
```

---

# 58. 🧪 Practice Lab — File Manager Check

Create:

```bash
nano file-manager.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter file or directory name: " path

if [ -f "$path" ]
then
    echo "It is a file."

    if [ -r "$path" ]
    then
        echo "File is readable."
    fi

    if [ -w "$path" ]
    then
        echo "File is writable."
    fi

elif [ -d "$path" ]
then
    echo "It is a directory."
else
    echo "Path does not exist."
fi
```

Run:

```bash
chmod +x file-manager.sh
./file-manager.sh
```

---

# 59. 🛠 Mini Project — Java Environment Validator

Create:

```bash
mkdir -p ~/Day21-JavaValidator
cd ~/Day21-JavaValidator
```

Create:

```bash
nano validate-java.sh
```

Add:

```bash
#!/bin/bash

echo "================================="
echo "      Java Environment Check"
echo "================================="

if command -v java > /dev/null 2>&1
then
    echo "Java: Installed"
    java --version
else
    echo "Java: NOT Installed"
    exit 1
fi

echo ""

if command -v javac > /dev/null 2>&1
then
    echo "Javac: Installed"
    javac --version
else
    echo "Javac: NOT Installed"
    exit 1
fi

echo ""

if [ -n "$JAVA_HOME" ]
then
    echo "JAVA_HOME: $JAVA_HOME"
else
    echo "JAVA_HOME: Not configured"
fi

echo ""
echo "Java environment check completed."
```

Make executable:

```bash
chmod +x validate-java.sh
```

Run:

```bash
./validate-java.sh
```

---

# 60. Expected Output

If Java is configured:

```text
=================================
      Java Environment Check
=================================
Java: Installed
java version "..."

Javac: Installed
javac ...

JAVA_HOME: ...

Java environment check completed.
```

If Java is missing, the script reports the problem and exits with a non-zero status.

---

# 61. 🔥 Day 21 Challenge

Try these without looking at the solutions above.

### Challenge 1 — Number Checker

Ask the user for a number.

Display:

```text
Positive
Negative
Zero
```

---

### Challenge 2 — Even/Odd

Ask for a number and display:

```text
Even
```

or:

```text
Odd
```

---

### Challenge 3 — File Checker

Ask for a path and determine whether it is:

```text
File
Directory
Does not exist
```

---

### Challenge 4 — File Permission Checker

Ask for a filename and check whether it is:

```text
Readable
Writable
Executable
```

---

### Challenge 5 — Java Checker

Check:

```text
java
javac
```

and display whether each is installed.

---

### Challenge 6 — Java Project Checker

Create a script that checks:

```text
Main.java exists?
javac exists?
```

If both are available, compile the Java program.

---

### Challenge 7 — Marks Calculator

Ask for marks and display:

```text
90–100 → A
75–89  → B
60–74  → C
40–59  → D
Below 40 → Fail
```

---

# 62. 💼 Real-World Bash Conditions

Conditions are heavily used in automation.

Example:

```text
Check Java
    ↓
Installed?
  ↙    ↘
Yes     No
 ↓       ↓
Compile  Stop
 ↓
Run
```

Another example:

```text
Check log file
      ↓
Does it exist?
   ↙       ↘
 Yes       No
  ↓         ↓
Backup    Create/Error
```

Another:

```text
Check disk usage
       ↓
Above threshold?
    ↙       ↘
  Yes        No
   ↓          ↓
Alert       Continue
```

These concepts become especially useful when we learn loops and complete Bash automation.

---

# 63. Common Mistakes

### Mistake 1 — Missing spaces

Wrong:

```bash
if ["$name" = "Srushti"]
```

Correct:

```bash
if [ "$name" = "Srushti" ]
```

---

### Mistake 2 — Using `=` for numeric comparison

Avoid:

```bash
if [ "$age" = 18 ]
```

For numeric comparison use:

```bash
if [ "$age" -eq 18 ]
```

---

### Mistake 3 — Forgetting `fi`

Every basic `if` block must end with:

```bash
fi
```

---

### Mistake 4 — Forgetting quotes

Prefer:

```bash
if [ "$name" = "Srushti" ]
```

rather than:

```bash
if [ $name = Srushti ]
```

Quoting helps when variables are empty or contain spaces.

---

### Mistake 5 — Confusing `-f` and `-d`

```text
-f → regular file
-d → directory
```

---

# 64. 🎤 Interview Questions

### 1. How do you write an `if` condition in Bash?

```bash
if [ condition ]
then
    command
fi
```

### 2. What is `fi`?

`fi` marks the end of an `if` statement.

### 3. What is `elif`?

`elif` means "else if" and allows additional conditions.

### 4. What is the difference between `-eq` and `=`?

`-eq` is generally used for numeric equality, while `=` is used for string equality with `[ ]`.

### 5. What does `-f` check?

It checks whether a path is a regular file.

### 6. What does `-d` check?

It checks whether a path is a directory.

### 7. What does `-e` check?

It checks whether a path exists.

### 8. What does `-x` check?

It checks whether a path is executable.

### 9. What is `[[ ]]`?

It is Bash's enhanced conditional expression syntax.

### 10. What does `$?` represent?

It contains the exit status of the previous command.

### 11. What does `&&` mean?

The command on the right runs if the command on the left succeeds.

### 12. What does `||` mean?

The command on the right runs if the command on the left fails.

---

# 65. ⚡ Quick Cheat Sheet

```bash
# Basic if
if [ condition ]
then
    command
fi

# if/else
if [ condition ]
then
    command
else
    command
fi

# if/elif/else
if [ condition ]
then
    command
elif [ condition ]
then
    command
else
    command
fi

# String
[ "$a" = "$b" ]
[ "$a" != "$b" ]
[ -z "$a" ]
[ -n "$a" ]

# Numbers
[ "$a" -eq "$b" ]
[ "$a" -ne "$b" ]
[ "$a" -gt "$b" ]
[ "$a" -ge "$b" ]
[ "$a" -lt "$b" ]
[ "$a" -le "$b" ]

# Files
[ -e "$file" ]
[ -f "$file" ]
[ -d "$path" ]
[ -r "$file" ]
[ -w "$file" ]
[ -x "$file" ]
[ -s "$file" ]

# AND
[ condition1 ] && [ condition2 ]

# OR
[ condition1 ] || [ condition2 ]

# NOT
[ ! -f "$file" ]

# Bash conditional
[[ "$name" == "Srushti" ]]

# Arithmetic condition
(( age >= 18 ))

# Command condition
if command -v java > /dev/null 2>&1
then
    echo "Java installed"
fi
```

---
