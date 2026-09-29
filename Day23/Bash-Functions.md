# 🔧 Day 23 — Bash Functions

## 🎯 Learning Objectives

By the end of Day 23, you will understand:

* What Bash functions are
* Why functions are useful
* Function syntax
* Calling functions
* Function parameters
* `$1`, `$2`, `$#`, `$@`, `$*`
* Local variables
* Function output
* `return` and exit status
* Functions with conditions
* Functions with loops
* Default arguments
* Argument validation
* Reusing functions
* Loading functions from another file
* Practical Linux automation using functions
* Building a Linux System Utility

---

# 1. What Is a Bash Function?

A function is a reusable block of commands.

Instead of writing the same commands multiple times, we put them inside a function and call the function whenever required.

### Without a function

```bash
echo "Checking Java..."
java -version

echo "Checking Java again..."
java -version
```

### With a function

```bash
check_java() {
    java -version
}

check_java
check_java
```

The function can be reused multiple times.

---

# 2. Why Use Functions?

Functions make scripts:

* Easier to read
* Easier to maintain
* Reusable
* Shorter
* Easier to debug
* More organized

For example, a system administration script might contain:

```text
show_system_info()
check_disk()
check_memory()
check_java()
check_network()
backup_files()
```

Each function performs one specific task.

---

# 3. Basic Function Syntax

The most common syntax is:

```bash
function_name() {
    commands
}
```

Example:

```bash
hello() {
    echo "Hello Linux!"
}
```

Call the function:

```bash
hello
```

Output:

```text
Hello Linux!
```

---

# 4. Function Keyword Syntax

You can also write:

```bash
function hello {
    echo "Hello Linux!"
}
```

Both styles work in Bash.

For beginner scripts, this style is very common:

```bash
hello() {
    echo "Hello Linux!"
}
```

---

# 5. Creating Your First Function

Create a file:

```bash
nano functions-demo.sh
```

Add:

```bash
#!/bin/bash

hello() {
    echo "Hello from Bash Function!"
}

hello
```

Make it executable:

```bash
chmod +x functions-demo.sh
```

Run:

```bash
./functions-demo.sh
```

Output:

```text
Hello from Bash Function!
```

---

# 6. Calling a Function Multiple Times

A function can be called multiple times.

```bash
greet() {
    echo "Welcome to Linux!"
}

greet
greet
greet
```

Output:

```text
Welcome to Linux!
Welcome to Linux!
Welcome to Linux!
```

---

# 7. Functions With Arguments

Functions can accept arguments.

Example:

```bash
greet() {
    echo "Hello $1"
}

greet "Srushti"
```

Output:

```text
Hello Srushti
```

Here:

```text
$1
```

means the first argument passed to the function.

---

# 8. Multiple Function Arguments

Example:

```bash
add_numbers() {
    echo "First number: $1"
    echo "Second number: $2"
}

add_numbers 10 20
```

Output:

```text
First number: 10
Second number: 20
```

---

# 9. Function Argument Variables

Important variables:

| Variable | Meaning                         |
| -------- | ------------------------------- |
| `$1`     | First argument                  |
| `$2`     | Second argument                 |
| `$3`     | Third argument                  |
| `$#`     | Number of arguments             |
| `$@`     | All arguments                   |
| `$*`     | All arguments as one expansion  |
| `$?`     | Exit status of previous command |

Example:

```bash
show_arguments() {
    echo "First: $1"
    echo "Second: $2"
    echo "Total arguments: $#"
}

show_arguments Java Linux Bash
```

Output:

```text
First: Java
Second: Linux
Total arguments: 3
```

---

# 10. `$@` — All Arguments

Example:

```bash
show_all() {
    for item in "$@"
    do
        echo "$item"
    done
}

show_all Java Linux Git Bash
```

Output:

```text
Java
Linux
Git
Bash
```

Using:

```bash
"$@"
```

is usually the safer choice when processing arguments individually.

---

# 11. `$*` — All Arguments

Example:

```bash
show_all() {
    echo "$*"
}

show_all Java Linux Git Bash
```

Output:

```text
Java Linux Git Bash
```

For most scripts where arguments should remain separate, prefer:

```bash
"$@"
```

---

# 12. Checking Number of Arguments

Use:

```bash
$#
```

Example:

```bash
greet() {

    if [ "$#" -eq 0 ]; then
        echo "Please provide a name."
        return 1
    fi

    echo "Hello $1"
}

greet
```

Output:

```text
Please provide a name.
```

---

# 13. Function With Two Numbers

Example:

```bash
calculate_sum() {

    local a=$1
    local b=$2

    local sum=$((a + b))

    echo "$sum"
}

calculate_sum 10 20
```

Output:

```text
30
```

---

# 14. Using `local`

Variables inside functions can be declared using:

```bash
local
```

Example:

```bash
calculate_sum() {

    local a=$1
    local b=$2
    local sum=$((a + b))

    echo "$sum"
}

calculate_sum 10 20
```

Using `local` helps prevent accidental changes to variables outside the function.

---

# 15. Why `local` Is Important

Consider:

```bash
name="Linux"

change_name() {
    name="Bash"
}

change_name

echo "$name"
```

Output:

```text
Bash
```

The function changed the outer variable.

Now use `local`:

```bash
name="Linux"

change_name() {
    local name="Bash"
    echo "$name"
}

change_name

echo "$name"
```

Output:

```text
Bash
Linux
```

The function's local variable does not overwrite the outer variable.

---

# 16. Function Output Using `echo`

A common pattern is:

```bash
calculate_sum() {
    local result=$(( $1 + $2 ))
    echo "$result"
}
```

Then:

```bash
result=$(calculate_sum 10 20)

echo "Result: $result"
```

Output:

```text
Result: 30
```

This is called **command substitution**.

---

# 17. `return` in Functions

`return` is mainly used to return an **exit status**.

Example:

```bash
check_number() {

    if [ "$1" -gt 0 ]; then
        return 0
    else
        return 1
    fi
}
```

Then:

```bash
check_number 10

echo "$?"
```

Output:

```text
0
```

Usually:

```text
0 = success
non-zero = failure
```

---

# 18. `return` vs `echo`

This is very important.

### Use `echo` for data/output

```bash
get_name() {
    echo "Srushti"
}
```

Use:

```bash
name=$(get_name)
```

### Use `return` for success/failure

```bash
is_even() {

    if (( $1 % 2 == 0 )); then
        return 0
    else
        return 1
    fi
}
```

Then:

```bash
if is_even 10; then
    echo "Even number"
else
    echo "Odd number"
fi
```

---

# 19. Function With Conditions

Example:

```bash
check_file() {

    if [ -f "$1" ]; then
        echo "File exists."
        return 0
    else
        echo "File does not exist."
        return 1
    fi
}

check_file "test.txt"
```

---

# 20. Function With Directory Check

```bash
check_directory() {

    if [ -d "$1" ]; then
        echo "$1 exists."
    else
        echo "$1 does not exist."
    fi
}

check_directory "/tmp"
```

---

# 21. Function With Loops

Functions can contain loops.

```bash
print_numbers() {

    for i in {1..5}
    do
        echo "$i"
    done
}

print_numbers
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

# 22. Function + Loop + Arguments

```bash
print_numbers() {

    local limit=$1

    for ((i=1; i<=limit; i++))
    do
        echo "$i"
    done
}

print_numbers 10
```

Output:

```text
1
2
3
4
5
6
7
8
9
10
```

---

# 23. Default Arguments

You can provide a default value.

```bash
greet() {

    local name=${1:-"Guest"}

    echo "Hello $name"
}
```

Run:

```bash
greet
```

Output:

```text
Hello Guest
```

Run:

```bash
greet Srushti
```

Output:

```text
Hello Srushti
```

---

# 24. Validate Function Arguments

Example:

```bash
calculate_sum() {

    if [ "$#" -ne 2 ]; then
        echo "Usage: calculate_sum number1 number2"
        return 1
    fi

    local result=$(( $1 + $2 ))

    echo "Sum: $result"
}

calculate_sum 10 20
```

Correct output:

```text
Sum: 30
```

Try:

```bash
calculate_sum 10
```

Output:

```text
Usage: calculate_sum number1 number2
```

---

# 25. Checking Java Installation

A useful function for a Java developer:

```bash
check_java() {

    if command -v java >/dev/null 2>&1; then
        echo "Java is installed."
        java -version
        return 0
    else
        echo "Java is not installed."
        return 1
    fi
}

check_java
```

This is useful in deployment and setup scripts.

---

# 26. Checking Git Installation

```bash
check_git() {

    if command -v git >/dev/null 2>&1; then
        echo "Git is installed."
        git --version
    else
        echo "Git is not installed."
    fi
}

check_git
```

---

# 27. Checking Disk Usage

```bash
check_disk() {

    echo "Disk Usage:"
    df -h /
}

check_disk
```

---

# 28. Checking Memory

```bash
check_memory() {

    echo "Memory Usage:"
    free -h
}

check_memory
```

---

# 29. Checking Network

```bash
check_network() {

    if ping -c 1 8.8.8.8 >/dev/null 2>&1; then
        echo "Network is available."
    else
        echo "Network is unavailable."
    fi
}

check_network
```

---

# 30. Checking a Port

Using `ss`:

```bash
check_port() {

    local port=$1

    if ss -ltn | grep -q ":$port "; then
        echo "Port $port is listening."
        return 0
    else
        echo "Port $port is not listening."
        return 1
    fi
}

check_port 8080
```

This is useful for checking a Spring Boot application.

---

# 31. Java + Linux Example

Suppose your Spring Boot application normally runs on:

```text
localhost:8080
```

You can create:

```bash
check_spring_boot() {

    local port=8080

    if ss -ltn | grep -q ":$port "; then
        echo "Spring Boot application appears to be running."
    else
        echo "Spring Boot application is not listening on port $port."
    fi
}

check_spring_boot
```

---

# 32. Backup Function

Example:

```bash
backup_directory() {

    local source=$1
    local destination=$2

    if [ ! -d "$source" ]; then
        echo "Source directory does not exist."
        return 1
    fi

    tar -czf "$destination" "$source"

    echo "Backup created: $destination"
}
```

Call:

```bash
backup_directory "project" "project-backup.tar.gz"
```

---

# 33. Functions From Another File

Functions can be stored separately.

Create:

```text
functions.sh
```

Example:

```bash
hello() {
    echo "Hello from external function!"
}

check_java() {
    java -version
}
```

Create:

```text
main.sh
```

Add:

```bash
#!/bin/bash

source functions.sh

hello
check_java
```

Run:

```bash
chmod +x main.sh
./main.sh
```

---

# 34. `source` Command

Instead of:

```bash
source functions.sh
```

you can use:

```bash
. functions.sh
```

Both mean:

> Load and execute the commands from this file in the current shell.

---

# 35. Function Menu

Functions can make menus cleaner.

```bash
show_menu() {

    echo "======================"
    echo " Linux Utility"
    echo "======================"
    echo "1. System Information"
    echo "2. Disk Usage"
    echo "3. Memory Usage"
    echo "4. Exit"
}
```

---

# 36. Practical Function-Based Menu

```bash
show_system_info() {
    echo "Hostname: $(hostname)"
    echo "User: $(whoami)"
    echo "Kernel:"
    uname -r
}

check_disk() {
    df -h /
}

check_memory() {
    free -h
}

show_menu() {

    echo "1. System Information"
    echo "2. Disk Usage"
    echo "3. Memory Usage"
    echo "4. Exit"
}

while true
do

    show_menu

    read -p "Enter choice: " choice

    case $choice in

        1)
            show_system_info
            ;;

        2)
            check_disk
            ;;

        3)
            check_memory
            ;;

        4)
            echo "Exiting..."
            break
            ;;

        *)
            echo "Invalid choice."
            ;;

    esac

done
```

This combines:

* Functions
* Variables
* `read`
* `case`
* loops
* commands

---

# 🧪 37. Practice Lab 1 — Basic Functions

Create:

```bash
nano practice-functions.sh
```

Write functions:

```text
hello()
show_date()
show_user()
show_directory()
```

Call all four.

Expected output should contain:

```text
Hello Linux
Current date
Current user
Current directory
```

---

# 🧪 38. Practice Lab 2 — Calculator Functions

Create:

```text
add()
subtract()
multiply()
divide()
```

Example:

```bash
add() {
    echo $(( $1 + $2 ))
}
```

Test:

```bash
add 10 5
subtract 10 5
multiply 10 5
divide 10 5
```

Expected:

```text
15
5
50
2
```

---

# 🧪 39. Practice Lab 3 — File Utility

Create functions:

```text
check_file()
check_directory()
count_lines()
show_file_size()
```

Example:

```bash
count_lines() {

    if [ -f "$1" ]; then
        wc -l "$1"
    else
        echo "File not found."
        return 1
    fi
}
```

---

# 🧪 40. Practice Lab 4 — Java Environment Checker

Create functions:

```text
check_java()
check_javac()
check_git()
check_maven()
```

Your script should display:

```text
===== Java Environment =====

Java:
Installed

Javac:
Installed

Git:
Installed

Maven:
Installed
```

If something is missing, display:

```text
Not Installed
```

---

# 🚀 41. Mini Project — Linux System Utility

Create:

```text
linux-system-utility.sh
```

The application should contain functions:

```text
show_system_info()
check_disk()
check_memory()
check_java()
check_git()
check_network()
check_ports()
```

Menu:

```text
====================================
       LINUX SYSTEM UTILITY
====================================

1. System Information
2. Disk Usage
3. Memory Usage
4. Java Check
5. Git Check
6. Network Check
7. Port Check
8. Run All Checks
9. Exit

Enter your choice:
```

---

# 42. Suggested Project Structure

```text
Day23-Project/
├── linux-system-utility.sh
└── README.md
```

---

# 43. Example System Information Function

```bash
show_system_info() {

    echo "===== SYSTEM INFORMATION ====="

    echo "User: $(whoami)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Architecture: $(uname -m)"
    echo "Current Directory: $(pwd)"
}
```

---

# 44. Example Run-All Function

```bash
run_all_checks() {

    show_system_info

    echo
    check_disk

    echo
    check_memory

    echo
    check_java

    echo
    check_git

    echo
    check_network
}
```

This demonstrates one of the biggest benefits of functions:

```text
Small reusable blocks
        ↓
Combined into larger operations
        ↓
Complete automation script
```

---

# ⚠️ 45. Common Mistakes

### Mistake 1 — Defining but not calling

```bash
hello() {
    echo "Hello"
}
```

Nothing happens until:

```bash
hello
```

---

### Mistake 2 — Forgetting `$`

Wrong:

```bash
echo "Hello 1"
```

Correct:

```bash
echo "Hello $1"
```

---

### Mistake 3 — Using `return` for strings

Avoid:

```bash
return "Hello"
```

Use:

```bash
echo "Hello"
```

and capture it:

```bash
result=$(function_name)
```

---

### Mistake 4 — Not quoting arguments

Prefer:

```bash
"$1"
```

instead of:

```bash
$1
```

Especially when filenames contain spaces.

---

### Mistake 5 — Forgetting `local`

For function-specific variables, prefer:

```bash
local result=...
```

---

# 🔍 46. Troubleshooting

### Check function definition

```bash
declare -f function_name
```

Example:

```bash
declare -f check_java
```

---

### Check whether a command exists

```bash
command -v java
```

---

### Check exit status

```bash
echo $?
```

---

### Debug a script

```bash
bash -x script.sh
```

This shows commands as Bash executes them.

---

# 💻 47. Java Developer Connection

Bash functions are useful when working with Java applications.

For example, you can create functions to:

```text
compile Java
run Java
check Java version
check Maven
check Spring Boot port
backup logs
search logs
restart an application
check disk space
```

Example:

```bash
run_java() {

    local file=$1

    if [ ! -f "$file" ]; then
        echo "Java file not found."
        return 1
    fi

    javac "$file" && java "${file%.java}"
}
```

Run:

```bash
run_java Main.java
```

This combines:

* Bash functions
* arguments
* file checking
* `javac`
* command chaining
* Java execution

---

# 🎯 48. Day 23 Challenge

Build a function-based script called:

```text
java-project-manager.sh
```

It should have:

```text
1. Check Java
2. Compile Java
3. Run Java
4. Check Git
5. Show Disk Usage
6. Search Java Files
7. Exit
```

Create separate functions:

```text
check_java()
compile_java()
run_java()
check_git()
show_disk()
find_java_files()
```

Do not put everything into one large block.

The goal is to practice **modular Bash scripting**.

---

# ❓ 49. Interview Questions

### Q1. What is a Bash function?

A reusable block of commands that can be called multiple times.

---

### Q2. How do you define a Bash function?

```bash
function_name() {
    commands
}
```

---

### Q3. How do you call a function?

```bash
function_name
```

---

### Q4. What does `$1` mean?

The first argument passed to the function or script.

---

### Q5. What does `$#` mean?

The number of arguments.

---

### Q6. What does `$@` represent?

All positional arguments, commonly used as:

```bash
"$@"
```

to process arguments individually.

---

### Q7. What is `local`?

It creates a variable whose scope is limited to the current function.

---

### Q8. What does `return 0` mean?

It indicates successful execution.

---

### Q9. What is the difference between `echo` and `return`?

`echo` produces output/data.

`return` sets a function's exit status.

---

### Q10. How can you load functions from another file?

Using:

```bash
source functions.sh
```

---

### Q11. How do you check the exit status of a function?

```bash
echo $?
```

---

### Q12. How do you debug a Bash script?

```bash
bash -x script.sh
```

---

# 📌 50. Day 23 Cheat Sheet

| Command / Syntax    | Purpose                     |
| ------------------- | --------------------------- |
| `name() { }`        | Define function             |
| `name`              | Call function               |
| `$1`                | First argument              |
| `$2`                | Second argument             |
| `$#`                | Number of arguments         |
| `"$@"`              | All arguments separately    |
| `"$*"`              | All arguments together      |
| `local x=value`     | Local variable              |
| `echo`              | Produce output              |
| `return 0`          | Success                     |
| `return 1`          | Failure                     |
| `$?`                | Previous exit status        |
| `source file.sh`    | Load another script         |
| `declare -f`        | Display function definition |
| `bash -x script.sh` | Debug script                |

---

