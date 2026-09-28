# 🔁 Day 22 — Bash Loops

Loops allow a Bash script to **repeat commands automatically**.

Instead of writing:

```bash
echo "Java"
echo "Java"
echo "Java"
echo "Java"
echo "Java"
```

you can use a loop:

```bash
for i in 1 2 3 4 5
do
    echo "Java"
done
```

Loops are extremely useful for:

* Processing multiple files
* Reading logs
* Automating repetitive tasks
* Working with servers
* Creating backups
* Processing command-line arguments
* Running repeated commands
* Java project automation

---

# 🎯 Learning Objectives

By the end of Day 22, you will understand:

* What loops are
* `for` loops
* `while` loops
* `until` loops
* Loop ranges
* Looping through files
* Looping through arguments
* `break`
* `continue`
* Nested loops
* Reading files with loops
* Practical automation
* Java + Bash loop automation

---

# 1. What Is a Loop?

A loop repeatedly executes a block of commands.

Example:

```text
Start
  ↓
Check condition
  ↓
Run commands
  ↓
Repeat
  ↓
Condition false?
  ↓
Exit
```

---

# 2. Why Do We Need Loops?

Imagine you have:

```text
file1.txt
file2.txt
file3.txt
file4.txt
file5.txt
```

Without a loop:

```bash
cat file1.txt
cat file2.txt
cat file3.txt
cat file4.txt
cat file5.txt
```

With a loop:

```bash
for file in *.txt
do
    cat "$file"
done
```

Much easier.

---

# 3. Types of Bash Loops

The three important Bash loops are:

```text
for
while
until
```

You will also use:

```text
break
continue
```

to control loops.

---

# 4. Basic `for` Loop

Syntax:

```bash
for variable in values
do
    commands
done
```

Example:

```bash
for name in Srushti Rahul Priya
do
    echo "Hello $name"
done
```

Output:

```text
Hello Srushti
Hello Rahul
Hello Priya
```

---

# 5. Understanding the Loop

This:

```bash
for name in Srushti Rahul Priya
```

means:

```text
name = Srushti
name = Rahul
name = Priya
```

For every value, Bash executes:

```bash
echo "Hello $name"
```

---

# 6. Simple Number Loop

```bash
for number in 1 2 3 4 5
do
    echo "Number: $number"
done
```

Output:

```text
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

---

# 7. Range With `{}`

Instead of:

```bash
for number in 1 2 3 4 5
```

you can write:

```bash
for number in {1..5}
do
    echo "$number"
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

# 8. Range From 1 to 10

```bash
for number in {1..10}
do
    echo "$number"
done
```

---

# 9. Range With a Step

You can use:

```bash
{start..end..step}
```

Example:

```bash
for number in {2..10..2}
do
    echo "$number"
done
```

Output:

```text
2
4
6
8
10
```

---

# 10. Countdown

```bash
for number in {10..1}
do
    echo "$number"
done
```

Output:

```text
10
9
8
7
6
5
4
3
2
1
```

---

# 11. `seq`

Another way to generate numbers is:

```bash
seq 1 5
```

Output:

```text
1
2
3
4
5
```

Use it in a loop:

```bash
for number in $(seq 1 5)
do
    echo "$number"
done
```

---

# 12. `seq` With a Step

```bash
seq 2 2 10
```

Output:

```text
2
4
6
8
10
```

Then:

```bash
for number in $(seq 2 2 10)
do
    echo "$number"
done
```

---

# 13. C-Style `for` Loop

Bash also supports C-style syntax:

```bash
for ((i=1; i<=5; i++))
do
    echo "$i"
done
```

This is very useful when you already know programming languages such as Java.

---

# 14. Understanding C-Style Loop

```bash
for ((i=1; i<=5; i++))
```

means:

```text
i=1
   ↓
check i<=5
   ↓
execute
   ↓
i++
   ↓
check again
```

This is similar to Java:

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

---

# 15. Bash `for` vs Java `for`

### Bash

```bash
for ((i=1; i<=5; i++))
do
    echo "$i"
done
```

### Java

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

The logic is very similar.

---

# 16. Loop Through Files

Suppose a directory contains:

```text
app.log
error.log
server.log
data.txt
```

You can process all `.log` files:

```bash
for file in *.log
do
    echo "Processing: $file"
done
```

Output:

```text
Processing: app.log
Processing: error.log
Processing: server.log
```

---

# 17. Display File Names

```bash
for file in *
do
    echo "$file"
done
```

This loops through entries in the current directory.

---

# 18. Check Whether Each Entry Is a File

```bash
for item in *
do
    if [ -f "$item" ]
    then
        echo "File: $item"
    fi
done
```

---

# 19. Check Whether Each Entry Is a Directory

```bash
for item in *
do
    if [ -d "$item" ]
    then
        echo "Directory: $item"
    fi
done
```

---

# 20. Loop Through Java Files

```bash
for file in *.java
do
    echo "Java file: $file"
done
```

This is useful for Java project automation.

---

# 21. Compile Multiple Java Files

Suppose you have:

```text
Main.java
Student.java
Teacher.java
```

You can compile:

```bash
for file in *.java
do
    echo "Compiling $file"
    javac "$file"
done
```

For many Java projects, compiling all source files together is usually preferable:

```bash
javac *.java
```

The loop example is mainly for learning automation.

---

# 22. Loop Through Command-Line Arguments

Remember from Day 20:

```bash
$@
```

represents all arguments.

Example:

```bash
for argument in "$@"
do
    echo "Argument: $argument"
done
```

Run:

```bash
./script.sh Java Linux Git Bash
```

Output:

```text
Argument: Java
Argument: Linux
Argument: Git
Argument: Bash
```

---

# 23. Why Quote `"$@"`?

Prefer:

```bash
for argument in "$@"
```

rather than:

```bash
for argument in $@
```

Quoting preserves each argument as a separate value, including arguments containing spaces.

---

# 24. Loop Through a List

```bash
languages=("Java" "Python" "C" "JavaScript")

for language in "${languages[@]}"
do
    echo "Language: $language"
done
```

Output:

```text
Language: Java
Language: Python
Language: C
Language: JavaScript
```

---

# 25. Bash Arrays

Arrays store multiple values.

Example:

```bash
skills=("Java" "Linux" "Git" "SQL")
```

Access first item:

```bash
echo "${skills[0]}"
```

Output:

```text
Java
```

Loop:

```bash
for skill in "${skills[@]}"
do
    echo "$skill"
done
```

---

# 26. `while` Loop

A `while` loop runs while a condition is true.

Syntax:

```bash
while [ condition ]
do
    commands
done
```

Example:

```bash
counter=1

while [ "$counter" -le 5 ]
do
    echo "$counter"
    counter=$((counter + 1))
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

# 27. Understanding `while`

The process is:

```text
counter=1
    ↓
counter <= 5?
    ↓
Yes → print
    ↓
counter + 1
    ↓
check again
```

When the condition becomes false, the loop stops.

---

# 28. Infinite Loop Warning

Be careful with:

```bash
while true
do
    echo "Running..."
done
```

This creates an infinite loop.

Stop it with:

```text
Ctrl + C
```

---

# 29. Useful `while` Example

```bash
counter=1

while [ "$counter" -le 10 ]
do
    echo "Running iteration $counter"
    counter=$((counter + 1))
done
```

---

# 30. User-Controlled While Loop

```bash
while true
do
    read -p "Enter a word (type exit to stop): " word

    if [ "$word" = "exit" ]
    then
        break
    fi

    echo "You entered: $word"
done
```

This creates an interactive loop.

---

# 31. `until` Loop

`until` is similar to `while`, but it continues **until the condition becomes true**.

Syntax:

```bash
until [ condition ]
do
    commands
done
```

Example:

```bash
counter=1

until [ "$counter" -gt 5 ]
do
    echo "$counter"
    counter=$((counter + 1))
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

# 32. `while` vs `until`

### While

Runs while condition is true:

```bash
while [ "$counter" -le 5 ]
```

### Until

Runs until condition becomes true:

```bash
until [ "$counter" -gt 5 ]
```

A simple way to remember:

```text
while → continue while TRUE

until → continue until TRUE
```

---

# 33. `break`

`break` immediately exits the loop.

Example:

```bash
for number in {1..10}
do
    if [ "$number" -eq 5 ]
    then
        break
    fi

    echo "$number"
done
```

Output:

```text
1
2
3
4
```

---

# 34. `continue`

`continue` skips the current iteration and moves to the next one.

Example:

```bash
for number in {1..5}
do
    if [ "$number" -eq 3 ]
    then
        continue
    fi

    echo "$number"
done
```

Output:

```text
1
2
4
5
```

---

# 35. `break` vs `continue`

```text
break
  ↓
Exit entire loop

continue
  ↓
Skip current iteration
  ↓
Continue next iteration
```

---

# 36. Skip Even Numbers

```bash
for number in {1..10}
do
    if (( number % 2 == 0 ))
    then
        continue
    fi

    echo "$number"
done
```

Output:

```text
1
3
5
7
9
```

---

# 37. Print Only Even Numbers

```bash
for number in {1..10}
do
    if (( number % 2 != 0 ))
    then
        continue
    fi

    echo "$number"
done
```

Output:

```text
2
4
6
8
10
```

---

# 38. Nested Loops

A loop inside another loop is called a nested loop.

Example:

```bash
for i in {1..3}
do
    for j in {1..3}
    do
        echo "i=$i j=$j"
    done
done
```

Output:

```text
i=1 j=1
i=1 j=2
i=1 j=3
i=2 j=1
i=2 j=2
i=2 j=3
i=3 j=1
i=3 j=2
i=3 j=3
```

---

# 39. Multiplication Table

Create:

```bash
nano table.sh
```

Add:

```bash
#!/bin/bash

read -p "Enter a number: " number

for ((i=1; i<=10; i++))
do
    result=$((number * i))
    echo "$number x $i = $result"
done
```

Run:

```bash
chmod +x table.sh
./table.sh
```

Example:

```text
Enter a number: 5
5 x 1 = 5
5 x 2 = 10
5 x 3 = 15
...
5 x 10 = 50
```

---

# 40. Read a File Line by Line

A common Linux automation task is reading a file.

Example:

```bash
while IFS= read -r line
do
    echo "Line: $line"
done < users.txt
```

Important:

```text
< users.txt
```

redirects the file into the loop.

---

# 41. Why `IFS=` and `-r`?

This:

```bash
while IFS= read -r line
```

is a safer pattern for reading text files.

* `IFS=` prevents unwanted trimming of leading/trailing whitespace.
* `-r` prevents backslash interpretation.

For simple files, beginners may also see:

```bash
while read line
do
    echo "$line"
done < users.txt
```

But the `IFS= read -r` version is preferred for robust scripts.

---

# 42. Create a Sample File

```bash
mkdir -p ~/Day22-Lab
cd ~/Day22-Lab
```

Create:

```bash
nano users.txt
```

Add:

```text
Srushti
Rahul
Priya
Amit
Sneha
```

Now:

```bash
while IFS= read -r user
do
    echo "User: $user"
done < users.txt
```

---

# 43. Process Log Files

Suppose:

```text
application.log
```

contains:

```text
INFO Application started
INFO Database connected
ERROR Database connection failed
INFO Application stopped
```

You can read each line:

```bash
while IFS= read -r line
do
    echo "LOG: $line"
done < application.log
```

Later, we can combine this with `grep`, conditions, and loops for powerful log automation.

---

# 44. Count Lines Using a Loop

```bash
count=0

while IFS= read -r line
do
    count=$((count + 1))
done < users.txt

echo "Total lines: $count"
```

If there are 5 users:

```text
Total lines: 5
```

---

# 45. Loop Through Files and Display Size

```bash
for file in *
do
    if [ -f "$file" ]
    then
        size=$(du -h "$file" | cut -f1)
        echo "$file → $size"
    fi
done
```

This is a simple file-reporting script.

---

# 46. Find Empty Files

```bash
for file in *
do
    if [ -f "$file" ] && [ ! -s "$file" ]
    then
        echo "Empty file: $file"
    fi
done
```

---

# 47. Find Java Files

```bash
for file in *.java
do
    if [ -f "$file" ]
    then
        echo "Java source: $file"
    fi
done
```

---

# 48. Count Java Files

```bash
count=0

for file in *.java
do
    if [ -f "$file" ]
    then
        count=$((count + 1))
    fi
done

echo "Java files: $count"
```

---

# 49. Bash Loop + Java Compilation

Suppose a project contains:

```text
Main.java
Student.java
Teacher.java
```

A simple learning example:

```bash
for file in *.java
do
    echo "Checking $file"
done
```

You can also compile:

```bash
for file in *.java
do
    echo "Compiling $file"
    javac "$file"
done
```

Again, for normal Java projects, compiling the project together is generally better:

```bash
javac *.java
```

The loop demonstrates how Bash can automate individual files.

---

# 50. Loop Through Git Files

You can process files returned by Git:

```bash
git ls-files
```

Example:

```bash
for file in $(git ls-files)
do
    echo "$file"
done
```

For filenames containing unusual whitespace, prefer null-delimited processing with appropriate tools rather than simple command substitution.

---

# 51. Loop With Command Output

Example:

```bash
for user in $(who | awk '{print $1}')
do
    echo "Logged in user: $user"
done
```

This demonstrates command substitution inside a loop.

---

# 52. Better Approach for Complex Data

Avoid relying on:

```bash
for item in $(command)
```

when output can contain spaces or special characters.

For simple whitespace-separated values, it is fine for learning.

For robust scripts, use:

* Arrays
* `while read`
* Null-delimited output
* Quoted variables

This becomes important in real-world automation.

---

# 53. Practical Backup Loop

Suppose:

```text
projects/
    app1/
    app2/
    app3/
```

You can process each directory:

```bash
for directory in projects/*
do
    if [ -d "$directory" ]
    then
        echo "Backing up: $directory"
    fi
done
```

Later this can be extended with `tar`.

---

# 54. Backup Java Projects

Example:

```bash
for project in ~/projects/*
do
    if [ -d "$project" ]
    then
        echo "Project found: $project"
    fi
done
```

A real backup script could then archive each project.

---

# 55. Loop Through Numbers and Calculate Squares

```bash
for number in {1..10}
do
    square=$((number * number))
    echo "$number → $square"
done
```

Output:

```text
1 → 1
2 → 4
3 → 9
...
10 → 100
```

---

# 56. Fibonacci Example

A simple Bash loop:

```bash
a=0
b=1

for ((i=1; i<=10; i++))
do
    echo -n "$a "
    next=$((a + b))
    a=$b
    b=$next
done

echo
```

Output:

```text
0 1 1 2 3 5 8 13 21 34
```

---

# 57. 🧪 Practice Lab — File Report

Create:

```bash
nano file-report.sh
```

Add:

```bash
#!/bin/bash

echo "===== File Report ====="

for file in *
do
    if [ -f "$file" ]
    then
        echo "File: $file"
    fi
done
```

Run:

```bash
chmod +x file-report.sh
./file-report.sh
```

---

# 58. 🧪 Practice Lab — Directory Report

```bash
nano directory-report.sh
```

Add:

```bash
#!/bin/bash

echo "===== Directory Report ====="

for item in *
do
    if [ -d "$item" ]
    then
        echo "Directory: $item"
    fi
done
```

Run:

```bash
chmod +x directory-report.sh
./directory-report.sh
```

---

# 59. 🧪 Practice Lab — Number Processor

Create:

```bash
nano number-loop.sh
```

Add:

```bash
#!/bin/bash

for ((i=1; i<=20; i++))
do
    if (( i % 2 == 0 ))
    then
        echo "$i is even"
    else
        echo "$i is odd"
    fi
done
```

Run:

```bash
chmod +x number-loop.sh
./number-loop.sh
```

---

# 60. 🧪 Practice Lab — Argument Processor

Create:

```bash
nano arguments.sh
```

Add:

```bash
#!/bin/bash

echo "===== Arguments ====="

for argument in "$@"
do
    echo "Argument: $argument"
done
```

Run:

```bash
chmod +x arguments.sh
./arguments.sh Java Linux Git Bash
```

---

# 61. 🛠 Mini Project — Linux File Scanner

Create:

```bash
mkdir -p ~/Day22-FileScanner
cd ~/Day22-FileScanner
```

Create:

```bash
nano scanner.sh
```

Add:

```bash
#!/bin/bash

echo "================================="
echo "       Linux File Scanner"
echo "================================="

file_count=0
directory_count=0

for item in *
do
    if [ -f "$item" ]
    then
        echo "FILE      : $item"
        file_count=$((file_count + 1))

    elif [ -d "$item" ]
    then
        echo "DIRECTORY : $item"
        directory_count=$((directory_count + 1))
    fi
done

echo ""
echo "================================="
echo "Files      : $file_count"
echo "Directories: $directory_count"
echo "================================="
```

Make executable:

```bash
chmod +x scanner.sh
```

Run:

```bash
./scanner.sh
```

---

# 62. Example Output

```text
=================================
       Linux File Scanner
=================================

FILE      : application.log
FILE      : scanner.sh
FILE      : users.txt
DIRECTORY : backup
DIRECTORY : projects

=================================
Files      : 3
Directories: 2
=================================
```

---

# 63. 🛠 Mini Project — Java Source Scanner

Create:

```bash
mkdir -p ~/Day22-JavaScanner
cd ~/Day22-JavaScanner
```

Create:

```bash
nano java-scanner.sh
```

Add:

```bash
#!/bin/bash

echo "================================="
echo "       Java Source Scanner"
echo "================================="

count=0

for file in *.java
do
    if [ -f "$file" ]
    then
        lines=$(wc -l < "$file")

        echo "Java File : $file"
        echo "Lines     : $lines"
        echo "-----------------------------"

        count=$((count + 1))
    fi
done

echo "Total Java files: $count"
```

Make executable:

```bash
chmod +x java-scanner.sh
```

Run:

```bash
./java-scanner.sh
```

---

# 64. What If There Are No `.java` Files?

Depending on Bash settings, a pattern such as:

```bash
*.java
```

may remain literally `*.java` when no files match.

For more robust scripts, enable:

```bash
shopt -s nullglob
```

Then:

```bash
files=( *.java )
```

If no Java files exist, the array is empty.

Example:

```bash
shopt -s nullglob

files=( *.java )

if [ "${#files[@]}" -eq 0 ]
then
    echo "No Java files found."
else
    for file in "${files[@]}"
    do
        echo "Java file: $file"
    done
fi
```

This is a useful real-world Bash technique.

---

# 65. Loop Control Summary

## `break`

Stops the loop completely.

```bash
for i in {1..10}
do
    if [ "$i" -eq 5 ]
    then
        break
    fi

    echo "$i"
done
```

---

## `continue`

Skips the current iteration.

```bash
for i in {1..5}
do
    if [ "$i" -eq 3 ]
    then
        continue
    fi

    echo "$i"
done
```

---

# 66. Nested Loop Example — Multiplication Tables

```bash
for i in {1..3}
do
    echo "Table of $i"

    for j in {1..5}
    do
        result=$((i * j))
        echo "$i x $j = $result"
    done

    echo ""
done
```

---

# 67. Infinite Loop

Example:

```bash
while true
do
    echo "Running..."
    sleep 2
done
```

Stop with:

```text
Ctrl + C
```

Infinite loops are useful for some monitoring programs, but they must have a clear exit mechanism in production scripts.

---

# 68. Loop + `sleep`

You can pause between iterations:

```bash
for i in {1..5}
do
    echo "Iteration $i"
    sleep 1
done
```

Output appears one second apart.

This is useful for:

* Monitoring
* Polling
* Retry mechanisms
* Demonstrations

---

# 69. Monitoring Example

```bash
for i in {1..5}
do
    echo "Checking system..."
    uptime
    sleep 2
done
```

This checks system uptime repeatedly.

---

# 70. Real-World Example — Check Java Process

```bash
for i in {1..5}
do
    if pgrep java > /dev/null
    then
        echo "Java process is running."
    else
        echo "Java process is not running."
    fi

    sleep 2
done
```

This combines:

```text
loop
+
condition
+
process checking
```

---

# 71. 🔥 Day 22 Challenge

Try these yourself.

### Challenge 1 — Print 1 to 20

Use a `for` loop.

---

### Challenge 2 — Print Even Numbers

Print:

```text
2
4
6
8
...
20
```

---

### Challenge 3 — Print Odd Numbers

Print:

```text
1
3
5
...
19
```

---

### Challenge 4 — Multiplication Table

Ask the user for a number and print its table from 1 to 10.

---

### Challenge 5 — Count Files

Count the regular files in the current directory.

---

### Challenge 6 — Count Directories

Count directories in the current directory.

---

### Challenge 7 — Java Files

Count `.java` files.

---

### Challenge 8 — Arguments

Run:

```bash
./script.sh Java Linux Git Bash
```

and print every argument on a separate line.

---

### Challenge 9 — Read a File

Create:

```text
names.txt
```

with 5 names.

Use a `while` loop to print:

```text
Hello Srushti
Hello Rahul
...
```

---

### Challenge 10 — Skip Number

Print 1–20 but skip:

```text
10
```

Use `continue`.

---

### Challenge 11 — Stop Number

Print 1–20 but stop when the number reaches:

```text
15
```

Use `break`.

---

### Challenge 12 — Java Process Monitor

Create a script that checks whether a Java process is running five times with a two-second delay.

---

# 72. 💼 Real-World Bash Automation

A common automation pattern is:

```text
           Bash Script
                ↓
        Find Files / Data
                ↓
             Loop
                ↓
           Check Condition
             ↙      ↘
           Yes       No
            ↓         ↓
        Process      Skip
            ↓
         Next Item
```

This pattern appears everywhere in Linux administration and DevOps.

---

# 73. Java Developer Use Cases

Bash loops can help with:

### Multiple Java files

```bash
for file in *.java
do
    echo "$file"
done
```

### Multiple Java projects

```bash
for project in ~/projects/*
do
    echo "Project: $project"
done
```

### Log processing

```bash
for log in *.log
do
    echo "Checking $log"
done
```

### Backup

```bash
for directory in ~/projects/*
do
    echo "Backup: $directory"
done
```

### Monitoring

```bash
for i in {1..10}
do
    pgrep java
    sleep 2
done
```

---

# 74. Common Mistakes

### Mistake 1 — Forgetting `do`

Wrong:

```bash
for file in *.txt
    echo "$file"
done
```

Correct:

```bash
for file in *.txt
do
    echo "$file"
done
```

---

### Mistake 2 — Forgetting `done`

Every loop must end with:

```bash
done
```

---

### Mistake 3 — Forgetting spaces in `[ ]`

Correct:

```bash
if [ -f "$file" ]
```

---

### Mistake 4 — Not quoting filenames

Prefer:

```bash
echo "$file"
```

instead of:

```bash
echo $file
```

Quoting helps with filenames containing spaces.

---

### Mistake 5 — Accidental infinite loop

Be careful with:

```bash
while true
```

Always provide an exit mechanism when appropriate.

---

# 75. 🎤 Interview Questions

### 1. What is a loop?

A loop repeatedly executes a block of commands until a specified condition or list is exhausted.

### 2. What are the main Bash loops?

```text
for
while
until
```

### 3. What is the syntax of a `for` loop?

```bash
for item in values
do
    commands
done
```

### 4. What does `while` do?

It repeatedly executes commands while its condition is true.

### 5. What does `until` do?

It repeatedly executes commands until its condition becomes true.

### 6. What does `break` do?

It immediately terminates the current loop.

### 7. What does `continue` do?

It skips the current iteration and continues with the next iteration.

### 8. How do you loop through all `.java` files?

```bash
for file in *.java
do
    echo "$file"
done
```

### 9. How do you loop through command-line arguments?

```bash
for argument in "$@"
do
    echo "$argument"
done
```

### 10. How do you create a C-style Bash loop?

```bash
for ((i=1; i<=10; i++))
do
    echo "$i"
done
```

### 11. How do you read a file line by line?

```bash
while IFS= read -r line
do
    echo "$line"
done < file.txt
```

### 12. How do you stop an infinite loop manually?

Press:

```text
Ctrl + C
```

---

# 76. ⚡ Quick Cheat Sheet

```bash
# Basic for
for item in one two three
do
    echo "$item"
done

# Range
for i in {1..10}
do
    echo "$i"
done

# Step
for i in {2..10..2}
do
    echo "$i"
done

# C-style
for ((i=1; i<=10; i++))
do
    echo "$i"
done

# Files
for file in *.txt
do
    echo "$file"
done

# Arguments
for arg in "$@"
do
    echo "$arg"
done

# While
counter=1

while [ "$counter" -le 5 ]
do
    echo "$counter"
    counter=$((counter + 1))
done

# Until
counter=1

until [ "$counter" -gt 5 ]
do
    echo "$counter"
    counter=$((counter + 1))
done

# Break
break

# Continue
continue

# Read file
while IFS= read -r line
do
    echo "$line"
done < file.txt

# Sleep
sleep 2



