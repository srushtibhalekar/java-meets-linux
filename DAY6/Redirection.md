# 🐧 Day 06 — Input & Output Redirection

> **30 Days of Linux — Day 06**

## 🎯 Mission

Today you will learn how Linux handles **input and output** and how to redirect command results into files.

By the end of Day 06, you should understand:

* Standard Input — `stdin`
* Standard Output — `stdout`
* Standard Error — `stderr`
* File descriptors `0`, `1`, `2`
* `>`
* `>>`
* `<`
* `2>`
* `2>>`
* `2>&1`
* `&>`
* `/dev/null`
* `tee`
* `tee -a`
* Saving command output
* Saving errors into log files
* Combining output and errors
* Practical Linux logging

---

# 1. What is Redirection?

Normally, when you execute a Linux command, the output appears on the terminal.

Example:

```bash
ls
```

Output:

```text
Documents
Downloads
Pictures
Projects
```

But Linux allows us to change where the output goes.

Instead of:

```text
Command → Terminal
```

we can do:

```text
Command → File
```

This process is called **redirection**.

For example:

```bash
ls > files.txt
```

Now the output of `ls` is stored inside:

```text
files.txt
```

The output will not normally be displayed on the terminal.

---

# 2. Standard Input, Output and Error

Linux commands generally work with three standard streams.

| Stream | Name            | File Descriptor | Purpose               |
| ------ | --------------- | --------------: | --------------------- |
| stdin  | Standard Input  |             `0` | Input to command      |
| stdout | Standard Output |             `1` | Normal command output |
| stderr | Standard Error  |             `2` | Error messages        |

Think of it like this:

```text
             ┌──────────────┐
stdin (0) →  │    COMMAND   │ → stdout (1)
             │              │
             └──────────────┘
                    │
                    ↓
                stderr (2)
```

---

# 3. Standard Input — stdin

`stdin` means **Standard Input**.

The default input source is usually your keyboard.

Example:

```bash
cat
```

Now type:

```text
Hello Linux
```

Press Enter.

The command receives your keyboard input.

Press:

```text
Ctrl + D
```

to indicate the end of input.

---

# 4. Standard Output — stdout

`stdout` means **Standard Output**.

Normally, stdout is displayed on your terminal.

Example:

```bash
echo "Hello Linux"
```

Output:

```text
Hello Linux
```

Another example:

```bash
pwd
```

Output:

```text
/home/user
```

This is normal command output.

---

# 5. Standard Error — stderr

`stderr` is used for error messages.

For example:

```bash
ls /folder-that-does-not-exist
```

You may see:

```text
ls: cannot access '/folder-that-does-not-exist': No such file or directory
```

This message is sent through **stderr**, not normal stdout.

This distinction becomes very important when saving command output.

---

# 6. File Descriptors

Linux represents the three standard streams using numbers.

```text
0 → stdin
1 → stdout
2 → stderr
```

Remember:

```text
0 = Input
1 = Output
2 = Error
```

### Easy memory trick

```text
0 → Input
1 → Output
2 → Error
```

---

# 7. `>` — Redirect Output

The `>` operator redirects stdout into a file.

Syntax:

```bash
command > filename
```

Example:

```bash
echo "Hello Linux" > hello.txt
```

Check the file:

```bash
cat hello.txt
```

Output:

```text
Hello Linux
```

---

# 8. Important: `>` Overwrites the File

Suppose:

```bash
echo "First line" > test.txt
```

Then:

```bash
cat test.txt
```

Output:

```text
First line
```

Now execute:

```bash
echo "Second line" > test.txt
```

Check again:

```bash
cat test.txt
```

Output:

```text
Second line
```

The previous content was replaced.

### Remember

```text
> = overwrite
```

Be careful when using it with important files.

---

# 9. `>>` — Append Output

The `>>` operator adds output to the end of a file.

Example:

```bash
echo "First line" > test.txt
```

Then:

```bash
echo "Second line" >> test.txt
```

Check:

```bash
cat test.txt
```

Output:

```text
First line
Second line
```

### Remember

```text
>  = overwrite
>> = append
```

---

# 10. Practical Example

Create a file:

```bash
echo "Linux" > subjects.txt
```

Add another line:

```bash
echo "Java" >> subjects.txt
```

Add another:

```bash
echo "Git" >> subjects.txt
```

View it:

```bash
cat subjects.txt
```

Output:

```text
Linux
Java
Git
```

---

# 11. Redirecting `ls`

Normally:

```bash
ls
```

displays files on the terminal.

Instead:

```bash
ls > files.txt
```

View the saved result:

```bash
cat files.txt
```

You have captured the command's output.

---

# 12. Redirecting `pwd`

Run:

```bash
pwd > location.txt
```

Then:

```bash
cat location.txt
```

The current directory path is stored in the file.

---

# 13. Redirecting `date`

Run:

```bash
date > current-date.txt
```

Then:

```bash
cat current-date.txt
```

You have saved the date output.

---

# 14. `<` — Redirect Input

The `<` operator takes input from a file.

Syntax:

```bash
command < filename
```

For example:

```bash
cat < test.txt
```

Here:

```text
test.txt → stdin → cat
```

Instead of getting input from the keyboard, `cat` gets it from the file.

---

# 15. Understanding `<`

Without redirection:

```bash
cat
```

Input comes from:

```text
Keyboard
   ↓
stdin
   ↓
cat
```

With:

```bash
cat < test.txt
```

Input comes from:

```text
test.txt
   ↓
stdin
   ↓
cat
```

---

# 16. `2>` — Redirect Errors

Remember:

```text
2 = stderr
```

Therefore:

```bash
command 2> errors.txt
```

redirects error messages into a file.

Example:

```bash
ls /wrong-directory 2> errors.txt
```

The terminal may not display the error.

Check:

```bash
cat errors.txt
```

You will see the error message stored there.

---

# 17. Why Use `2>`?

Suppose you are running a script or command that may produce errors.

You can save errors separately:

```bash
./script.sh 2> errors.log
```

Now:

```text
Normal output → terminal
Errors → errors.log
```

This is useful for troubleshooting.

---

# 18. `2>>` — Append Errors

Just like `>>`, `2>>` appends errors.

Example:

```bash
ls /wrong1 2> errors.log
```

Then:

```bash
ls /wrong2 2>> errors.log
```

The second error is added to the existing file.

Remember:

```text
2>  = overwrite error file
2>> = append errors
```

---

# 19. Redirect stdout and stderr Separately

Suppose you want normal output in one file and errors in another.

Use:

```bash
command > output.txt 2> errors.txt
```

Example:

```bash
find /etc -name "*.conf" > results.txt 2> errors.txt
```

Now:

```text
Normal results → results.txt
Errors          → errors.txt
```

This is extremely useful for scripts and system administration.

---

# 20. Redirect Both stdout and stderr

Sometimes you want everything in one file.

One common Bash syntax is:

```bash
command > output.log 2>&1
```

Meaning:

```text
stdout → output.log
stderr → stdout
stdout → output.log
```

Therefore both streams end up in:

```text
output.log
```

Example:

```bash
ls /etc /wrong-folder > result.log 2>&1
```

Now check:

```bash
cat result.log
```

The file can contain both successful output and error messages.

---

# 21. Why Does `2>&1` Look Strange?

Break it down:

```text
2 = stderr
1 = stdout
& = file descriptor
```

Therefore:

```bash
2>&1
```

means:

> Send stderr to the same destination as stdout.

---

# 22. Order Matters

These two commands are not always equivalent:

```bash
command > output.txt 2>&1
```

and:

```bash
command 2>&1 > output.txt
```

The first is the common form when you want both stdout and stderr in the same file.

Why?

Because redirections are processed from left to right.

Correct:

```bash
command > output.txt 2>&1
```

First:

```text
stdout → output.txt
```

Then:

```text
stderr → wherever stdout currently goes
```

Therefore both go to the file.

---

# 23. `&>` — Redirect Everything

In Bash, you can also use:

```bash
command &> output.log
```

This redirects both:

```text
stdout
stderr
```

to the same file.

Example:

```bash
ls /etc /wrong-folder &> result.log
```

Then:

```bash
cat result.log
```

---

# 24. `/dev/null`

Linux provides a special device:

```text
/dev/null
```

Anything sent there is discarded.

Think of it as:

> Linux's digital trash can.

Example:

```bash
echo "Hello" > /dev/null
```

Nothing appears.

---

# 25. Ignore Errors

You can discard errors:

```bash
command 2> /dev/null
```

Example:

```bash
ls /wrong-folder 2> /dev/null
```

The error is discarded.

This is useful when you intentionally don't care about errors.

---

# 26. Discard All Output

You can discard stdout:

```bash
command > /dev/null
```

Or both stdout and stderr:

```bash
command > /dev/null 2>&1
```

In Bash, another short form is:

```bash
command &> /dev/null
```

---

# 27. `tee` Command

`tee` is extremely useful.

It allows you to:

```text
Display output on terminal
+
Save the same output to a file
```

Example:

```bash
ls | tee files.txt
```

You see the output on the terminal.

At the same time, the output is saved in:

```text
files.txt
```

---

# 28. Why `tee` Is Useful

Compare:

```bash
ls > files.txt
```

Output:

```text
Not displayed
```

But:

```bash
ls | tee files.txt
```

Output:

```text
Displayed on terminal
+
Saved to files.txt
```

This is very useful when monitoring commands.

---

# 29. `tee -a`

Normal:

```bash
tee files.txt
```

overwrites the file.

Use:

```bash
tee -a files.txt
```

to append.

Example:

```bash
echo "Linux" | tee -a subjects.txt
```

Then:

```bash
echo "Java" | tee -a subjects.txt
```

Check:

```bash
cat subjects.txt
```

---

# 30. `echo` + Redirection

Example:

```bash
echo "Student Name: Srushti" > student.txt
```

Append:

```bash
echo "Course: Engineering" >> student.txt
```

Append:

```bash
echo "Skill: Java" >> student.txt
```

View:

```bash
cat student.txt
```

---

# 31. `printf` + Redirection

`printf` gives more control over formatting.

Example:

```bash
printf "Name: Srushti\n" > student.txt
```

Then:

```bash
printf "Skill: Java\n" >> student.txt
```

Then:

```bash
printf "Skill: Linux\n" >> student.txt
```

View:

```bash
cat student.txt
```

---

# 32. Saving Command Results

You can save almost any command's output.

Examples:

```bash
pwd > location.txt
```

```bash
date > date.txt
```

```bash
whoami > user.txt
```

```bash
ls -la > directory.txt
```

```bash
df -h > disk.txt
```

```bash
ps aux > processes.txt
```

This is useful when creating reports or logs.

---

# 33. Saving Search Results

You can combine previous topics with redirection.

Example:

```bash
find . -type f > files.txt
```

Now all matching files are saved in:

```text
files.txt
```

You can inspect them:

```bash
cat files.txt
```

---

# 34. Saving `grep` Results

Example:

```bash
grep "ERROR" application.log > errors.txt
```

Now matching lines are saved into:

```text
errors.txt
```

Append more results:

```bash
grep "WARNING" application.log >> errors.txt
```

---

# 35. Save Successful Results and Errors Separately

Example:

```bash
find /etc -name "*.conf" > config-files.txt 2> search-errors.txt
```

You now have:

```text
config-files.txt
search-errors.txt
```

This is a very practical Linux administration technique.

---

# 36. Creating a Log File

Suppose you want to create a simple system log.

```bash
echo "System Check Started" > system.log
date >> system.log
whoami >> system.log
pwd >> system.log
```

View:

```bash
cat system.log
```

Possible output:

```text
System Check Started
Thu Sep 3 10:30:00 IST 2026
srushti
/home/srushti
```

---

# 37. Using `tee` for Logs

You can display and save output simultaneously.

Example:

```bash
date | tee system.log
```

Then:

```bash
whoami | tee -a system.log
```

Then:

```bash
pwd | tee -a system.log
```

View:

```bash
cat system.log
```

---

# 38. Real-World Example — Application Logs

Suppose a Java application produces output.

You can save it:

```bash
java Main > application.log
```

Save errors separately:

```bash
java Main > application.log 2> application-error.log
```

Save everything together:

```bash
java Main > application.log 2>&1
```

This is commonly useful when running applications on Linux servers.

---

# 39. Command Output Pipeline

Redirection becomes even more powerful when combined with pipes.

Example:

```bash
ls | tee files.txt
```

Conceptually:

```text
ls
 ↓
stdout
 ↓
pipe
 ↓
tee
 ├──→ terminal
 └──→ files.txt
```

You will learn pipes deeply on **Day 07**.

---

# 40. Important Difference: `>` vs `>>`

| Operator    | Meaning                           |
| ----------- | --------------------------------- |
| `>`         | Create/overwrite                  |
| `>>`        | Create/append                     |
| `2>`        | Redirect errors                   |
| `2>>`       | Append errors                     |
| `<`         | Read input from file              |
| `2>&1`      | Send stderr to stdout destination |
| `&>`        | Redirect stdout + stderr          |
| `/dev/null` | Discard output                    |
| `tee`       | Display + save                    |
| `tee -a`    | Display + append                  |

---

# 41. Practice Environment

Create a directory:

```bash
mkdir Day6-Practice
cd Day6-Practice
```

Create some files:

```bash
touch file1.txt file2.txt file3.txt
```

Add content:

```bash
echo "Linux Redirection" > file1.txt
echo "Standard Output" > file2.txt
echo "Standard Error" > file3.txt
```

Check:

```bash
ls
```

---

# 42. Practice 1 — Save Directory Listing

Run:

```bash
ls > directory.txt
```

Check:

```bash
cat directory.txt
```

---

# 43. Practice 2 — Append Data

Run:

```bash
echo "Day 6 Linux Practice" >> directory.txt
```

Check:

```bash
cat directory.txt
```

---

# 44. Practice 3 — Redirect Error

Run:

```bash
ls does-not-exist 2> error.txt
```

Check:

```bash
cat error.txt
```

---

# 45. Practice 4 — Combine Output and Error

Run:

```bash
ls file1.txt does-not-exist > result.txt 2>&1
```

Then:

```bash
cat result.txt
```

You should see both successful output and the error.

---

# 46. Practice 5 — Discard Error

Run:

```bash
ls does-not-exist 2> /dev/null
```

The error will be discarded.

---

# 47. Practice 6 — Use `tee`

Run:

```bash
ls | tee listing.txt
```

Check:

```bash
cat listing.txt
```

---

# 48. Practice 7 — Append Using `tee`

Run:

```bash
echo "First entry" | tee -a log.txt
```

Then:

```bash
echo "Second entry" | tee -a log.txt
```

Check:

```bash
cat log.txt
```

---

# 49. Mini Project — Linux System Report

Create a project:

```bash
mkdir Day6-System-Report
cd Day6-System-Report
```

Create a report:

```bash
echo "===== LINUX SYSTEM REPORT =====" > system-report.txt
```

Add username:

```bash
whoami >> system-report.txt
```

Add current directory:

```bash
pwd >> system-report.txt
```

Add date:

```bash
date >> system-report.txt
```

Add files:

```bash
ls -la >> system-report.txt
```

Add disk information:

```bash
df -h >> system-report.txt
```

View the report:

```bash
cat system-report.txt
```

---

# 50. Mini Project — Error Logger

Create:

```bash
mkdir ErrorLogger
cd ErrorLogger
```

Run commands that may fail:

```bash
ls /etc > output.log 2> errors.log
ls /wrong-directory >> output.log 2>> errors.log
```

View normal output:

```bash
cat output.log
```

View errors:

```bash
cat errors.log
```

You now have two separate logs:

```text
output.log → successful output
errors.log → errors
```

---

# 51. Common Mistakes

### Mistake 1 — Confusing `>` and `>>`

Wrong assumption:

```text
> adds content
```

Actually:

```text
> overwrites
```

Use:

```bash
>> 
```

when you want to append.

---

### Mistake 2 — Forgetting stderr

This:

```bash
command > output.txt
```

does **not** redirect stderr.

Errors can still appear on the terminal.

To redirect both:

```bash
command > output.txt 2>&1
```

---

### Mistake 3 — Dangerous overwrite

Be careful with:

```bash
> important-file.txt
```

It can replace the file's contents.

---

### Mistake 4 — Wrong order

Remember:

```bash
command > output.txt 2>&1
```

is the standard form for putting both stdout and stderr into the same file.

---

# 52. Important Mental Model

Always imagine:

```text
              ┌──────────────┐
stdin (0) ──→ │              │ ──→ stdout (1)
              │    COMMAND   │
              │              │ ──→ stderr (2)
              └──────────────┘
```

Redirection changes where these streams go.

For example:

```bash
command > output.txt
```

means:

```text
stdout → output.txt
```

And:

```bash
command 2> error.txt
```

means:

```text
stderr → error.txt
```

And:

```bash
command > result.txt 2>&1
```

means:

```text
stdout ──┐
         ├──→ result.txt
stderr ──┘
```

---

# 53. Day 06 Cheat Sheet

```bash
# Standard streams
0 = stdin
1 = stdout
2 = stderr

# Output redirection
command > file

# Append output
command >> file

# Input redirection
command < file

# Error redirection
command 2> file

# Append errors
command 2>> file

# Redirect stdout + stderr
command > file 2>&1

# Bash shorthand
command &> file

# Discard output
command > /dev/null

# Discard errors
command 2> /dev/null

# Discard everything
command > /dev/null 2>&1

# Display + save
command | tee file

# Display + append
command | tee -a file
```

---


By completing Day 06, you learned:

```text
stdin
stdout
stderr
file descriptors
>
>>
<
2>
2>>
2>&1
&>
/dev/null
tee
tee -a
```

You can now control where Linux commands send their input, output, and errors.

