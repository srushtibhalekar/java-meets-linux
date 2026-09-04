# 🐧 Day 07 — Pipes & Command Chaining

> **30 Days of Linux — Day 07**

## 🎯 Mission

Today you will learn how to connect Linux commands together.

By the end of Day 07, you will understand:

* Pipe `|`
* `&&`
* `||`
* `;`
* Command chaining
* Combining multiple commands
* Pipes with `grep`
* Pipes with `sort`
* Pipes with `head` and `tail`
* Pipes with `wc`
* Pipes with `tee`
* Exit status
* Conditional command execution
* `xargs`
* Real-world Linux command pipelines

---

# 1. What Is a Pipe?

A **pipe** allows the output of one command to become the input of another command.

The symbol is:

```bash
|
```

Basic syntax:

```bash
command1 | command2
```

Think:

```text
command1
   ↓
 output
   ↓
   |
   ↓
command2
```

Example:

```bash
ls | grep ".txt"
```

Here:

```text
ls output
   ↓
grep receives it
   ↓
only .txt files are displayed
```

---

# 2. Why Are Pipes Important?

Linux follows a powerful philosophy:

> Make small commands that do one job well and combine them together.

Instead of creating one huge command, you can create a pipeline.

Example:

```bash
ls | grep ".java" | wc -l
```

This performs three operations:

```text
ls
 ↓
find Java files
 ↓
count them
```

---

# 3. Basic Pipe Example

Run:

```bash
ls
```

Now:

```bash
ls | less
```

The output of `ls` is passed to `less`.

---

# 4. Pipe with `grep`

Suppose:

```bash
ls
```

shows:

```text
Main.java
Test.java
README.md
notes.txt
App.java
```

Run:

```bash
ls | grep ".java"
```

Output:

```text
Main.java
Test.java
App.java
```

The pipe sends `ls` output to `grep`.

---

# 5. Pipe with `wc`

The `wc` command can count lines.

Example:

```bash
ls | wc -l
```

This counts the number of entries displayed by `ls`.

Conceptually:

```text
ls
 ↓
list of files
 ↓
wc -l
 ↓
number
```

---

# 6. Pipe with `head`

Show only the first 5 results:

```bash
ls | head -5
```

Pipeline:

```text
ls
 ↓
head
 ↓
first 5 lines
```

---

# 7. Pipe with `tail`

Show the last 5 results:

```bash
ls | tail -5
```

---

# 8. Multiple Pipes

You can use more than one pipe.

Example:

```bash
ls | grep ".java" | head -5
```

Execution:

```text
ls
 ↓
grep ".java"
 ↓
head -5
```

Another example:

```bash
ls | grep ".java" | wc -l
```

This counts Java files.

---

# 9. Real Example

Suppose you have:

```text
App.java
Main.java
Test.java
index.html
style.css
README.md
Login.java
```

Run:

```bash
ls | grep ".java"
```

Output:

```text
App.java
Main.java
Test.java
Login.java
```

Count them:

```bash
ls | grep ".java" | wc -l
```

Output:

```text
4
```

---

# 10. Pipe with `sort`

Suppose:

```bash
cat names.txt
```

contains:

```text
Rahul
Srushti
Amit
Priya
Neha
```

Run:

```bash
cat names.txt | sort
```

Output:

```text
Amit
Neha
Priya
Rahul
Srushti
```

---

# 11. Reverse Sorting

Use:

```bash
cat names.txt | sort -r
```

Output:

```text
Srushti
Rahul
Priya
Neha
Amit
```

---

# 12. Pipe with `uniq`

Suppose:

```text
Java
Java
Linux
Linux
Linux
Git
```

Run:

```bash
cat subjects.txt | sort | uniq
```

Output:

```text
Git
Java
Linux
```

Why `sort` first?

Because `uniq` removes adjacent duplicate lines.

Therefore:

```text
cat
 ↓
sort
 ↓
uniq
```

---

# 13. Count Unique Values

You can combine:

```bash
cat subjects.txt | sort | uniq -c
```

Example output:

```text
1 Git
2 Java
3 Linux
```

This tells you how many times each value appears.

---

# 14. Pipe with `cut`

Suppose:

```text
Srushti,Java,India
Rahul,Python,India
Amit,C++,India
```

You can extract the first column:

```bash
cat students.csv | cut -d "," -f 1
```

Output:

```text
Srushti
Rahul
Amit
```

Pipeline:

```text
cat
 ↓
cut
```

---

# 15. Pipe with `tr`

Example:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
```

Output:

```text
HELLO LINUX
```

Here:

```text
echo
 ↓
tr
 ↓
uppercase text
```

---

# 16. Pipe with `tee`

You learned `tee` on Day 06.

Example:

```bash
ls | tee files.txt
```

This:

1. Displays output.
2. Saves output to `files.txt`.

You can continue the pipeline:

```bash
ls | tee files.txt | grep ".java"
```

---

# 17. Pipeline with `find`

Example:

```bash
find . -type f | grep ".java"
```

This searches files and then filters Java files.

Another:

```bash
find . -type f | wc -l
```

Counts files found by `find`.

---

# 18. Pipeline with `ps`

Processes can be filtered using pipes.

Example:

```bash
ps aux | grep java
```

This displays processes containing:

```text
java
```

Another:

```bash
ps aux | grep ssh
```

This searches for SSH-related processes.

---

# 19. Pipeline with `df`

Check disk usage:

```bash
df -h
```

Filter a filesystem:

```bash
df -h | grep "/"
```

---

# 20. Pipeline with `du`

Example:

```bash
du -h | sort -h
```

This displays disk usage sorted by size.

Reverse:

```bash
du -h | sort -hr
```

---

# 21. Pipeline with `history`

You can search your command history:

```bash
history | grep git
```

This shows previous commands containing `git`.

Another:

```bash
history | grep java
```

---

# 22. Pipeline with `head`

Example:

```bash
history | head -10
```

Shows the first 10 history entries.

---

# 23. Pipeline with `tail`

Example:

```bash
history | tail -10
```

Shows the last 10 entries.

---

# 24. Command Chaining

Command chaining means executing multiple commands in one line.

Linux provides several operators:

```text
|
&&
||
;
```

They have different meanings.

---

# 25. `;` — Execute Commands Sequentially

The semicolon allows multiple commands to run one after another.

Example:

```bash
pwd; ls; date
```

Execution:

```text
pwd
 ↓
ls
 ↓
date
```

Each command runs regardless of whether the previous command succeeds or fails.

---

# 26. Example of `;`

Run:

```bash
echo "Start"; echo "Middle"; echo "End"
```

Output:

```text
Start
Middle
End
```

---

# 27. `&&` — Run Next Only if Successful

The operator:

```bash
&&
```

means:

> Run the next command only if the previous command succeeds.

Example:

```bash
mkdir project && cd project
```

If `mkdir project` succeeds:

```text
mkdir project
     ↓
   success
     ↓
cd project
```

If it fails, `cd project` is not executed.

---

# 28. Practical `&&` Example

```bash
mkdir Day7 && cd Day7
```

This is safer than:

```bash
mkdir Day7; cd Day7
```

because `cd` runs only if the directory was successfully created.

---

# 29. Another `&&` Example

```bash
echo "Hello" && echo "Success"
```

Output:

```text
Hello
Success
```

Both commands succeed.

---

# 30. `||` — Run if Previous Command Fails

The operator:

```bash
||
```

means:

> Run the next command only if the previous command fails.

Example:

```bash
cd project || echo "Directory not found"
```

If `project` exists:

```text
No error message
```

If it doesn't exist:

```text
Directory not found
```

---

# 31. `&&` vs `||`

| Operator | Meaning                               |   |                                    |
| -------- | ------------------------------------- | - | ---------------------------------- |
| `&&`     | Run next command if previous succeeds |   |                                    |
| `        |                                       | ` | Run next command if previous fails |
| `;`      | Run next command regardless           |   |                                    |

Easy memory:

```text
&& → Success
|| → Failure
;  → Always continue
```

---

# 32. Combining `&&` and `||`

You can build simple success/failure logic.

Example:

```bash
mkdir project && echo "Created successfully" || echo "Creation failed"
```

Concept:

```text
mkdir
 ↓
Success? ── YES → success message
    │
    NO
    ↓
failure message
```

---

# 33. `;` vs `&&`

Consider:

```bash
mkdir test; cd test
```

Even if `mkdir` fails, `cd` will still run.

Now:

```bash
mkdir test && cd test
```

`cd` runs only when `mkdir` succeeds.

For automation, `&&` is often safer.

---

# 34. Check Exit Status

Linux commands return an **exit status**.

Usually:

```text
0 = success
non-zero = failure/error
```

After a command:

```bash
echo $?
```

shows its exit status.

Example:

```bash
pwd
echo $?
```

Possible output:

```text
/home/user
0
```

The `pwd` command succeeded.

---

# 35. Failed Command Exit Status

Run:

```bash
ls /does-not-exist
```

Then:

```bash
echo $?
```

You will normally get a non-zero value.

For example:

```text
2
```

The exact non-zero value depends on the command.

---

# 36. Why Exit Status Matters

`&&` and `||` depend on command success/failure.

Example:

```bash
command1 && command2
```

means:

```text
if command1 succeeds
    run command2
```

While:

```bash
command1 || command2
```

means:

```text
if command1 fails
    run command2
```

This becomes very important in Bash scripting.

---

# 37. Practical Developer Example

Suppose you want to:

1. Create a directory.
2. Enter it.
3. Create a file.

Use:

```bash
mkdir project && cd project && touch README.md
```

This creates a chain:

```text
mkdir
 ↓
cd
 ↓
touch
```

If one important step fails, later steps don't execute.

---

# 38. Git Example

Command chaining is very useful with Git.

For example:

```bash
git add . && git commit -m "update project" && git push
```

Conceptually:

```text
git add
 ↓ success
git commit
 ↓ success
git push
```

If `git add` fails, the commit won't run.

If commit fails, push won't run.

---

# 39. Java Example

Suppose you want to compile and run Java:

```bash
javac Main.java && java Main
```

This means:

```text
Compile
 ↓
Successful?
 ↓
Run program
```

If compilation fails, Java won't run.

This is a very useful pattern.

---

# 40. Linux + Java

For your Java development, this is especially useful:

```bash
javac Main.java && java Main
```

Instead of:

```bash
javac Main.java
java Main
```

The chained version prevents running an old `.class` file when compilation fails.

---

# 41. `xargs`

`xargs` is used to build commands from input.

Basic example:

```bash
echo "one two three" | xargs
```

Another useful example:

```bash
find . -name "*.txt" | xargs wc -l
```

Conceptually:

```text
find
 ↓
.txt files
 ↓
xargs
 ↓
wc -l
```

`xargs` takes input and uses it as arguments to another command.

---

# 42. `find` + `xargs`

Example:

```bash
find . -name "*.log" | xargs ls -l
```

This finds `.log` files and passes them to `ls -l`.

---

# 43. Important `xargs` Warning

If filenames contain spaces, simple `xargs` usage can cause problems.

For example:

```text
my application.log
```

may be interpreted as two separate arguments.

A safer pattern with GNU `find` is:

```bash
find . -name "*.log" -print0 | xargs -0 ls -l
```

Here:

```text
-print0
```

and:

```text
-0
```

work together to safely handle spaces and special characters.

---

# 44. Pipe vs Redirection

These are different.

### Pipe

```bash
command1 | command2
```

Output of command 1 becomes input of command 2.

### Redirection

```bash
command > file
```

Output goes into a file.

Think:

```text
Pipe:
command → command

Redirection:
command → file
```

---

# 45. Pipe vs `&&`

These are also completely different.

### Pipe

```bash
ls | grep ".java"
```

Connects data between commands.

### `&&`

```bash
javac Main.java && java Main
```

Connects command execution based on success.

Remember:

```text
|  → data flow
&& → success-based execution
|| → failure-based execution
;  → sequential execution
```

---

# 46. Powerful Pipeline Example

Suppose you want to count Java files:

```bash
find . -type f | grep "\.java$" | wc -l
```

Pipeline:

```text
find
 ↓
all files
 ↓
grep
 ↓
Java files
 ↓
wc
 ↓
count
```

---

# 47. Another Powerful Example

Find Java files and show the first 10:

```bash
find . -type f | grep "\.java$" | head -10
```

---

# 48. Sort Java Files

```bash
find . -type f | grep "\.java$" | sort
```

---

# 49. Count Unique Extensions

You can create a pipeline to inspect file extensions.

For example:

```bash
find . -type f | sed 's/.*\.//' | sort | uniq -c
```

Possible output:

```text
5 java
3 txt
2 md
1 csv
```

This combines multiple commands into one workflow.

---

# 50. Real-World Log Analysis

Suppose:

```text
application.log
```

contains application logs.

Find errors:

```bash
cat application.log | grep "ERROR"
```

Count errors:

```bash
cat application.log | grep "ERROR" | wc -l
```

Show first 10 errors:

```bash
cat application.log | grep "ERROR" | head -10
```

Sort matching lines:

```bash
cat application.log | grep "ERROR" | sort
```

---

# 51. Better Log Search

You don't always need `cat`.

Instead of:

```bash
cat application.log | grep "ERROR"
```

you can directly use:

```bash
grep "ERROR" application.log
```

This is simpler.

The `cat | grep` pattern is useful for learning pipelines, but unnecessary when the second command already accepts a filename.

---

# 52. Practice Environment

Create:

```bash
mkdir Day7-Practice
cd Day7-Practice
```

Create files:

```bash
touch app.java main.java test.java notes.txt readme.md
```

Check:

```bash
ls
```

---

# 53. Practice 1 — Find Java Files

```bash
ls | grep ".java"
```

---

# 54. Practice 2 — Count Java Files

```bash
ls | grep ".java" | wc -l
```

---

# 55. Practice 3 — First Two Java Files

```bash
ls | grep ".java" | head -2
```

---

# 56. Practice 4 — Sort Files

```bash
ls | sort
```

---

# 57. Practice 5 — Reverse Sort

```bash
ls | sort -r
```

---

# 58. Practice 6 — Command Chaining

Run:

```bash
mkdir test && cd test && touch file.txt
```

Then:

```bash
ls
```

---

# 59. Practice 7 — Failure Handling

Run:

```bash
cd does-not-exist || echo "Cannot enter directory"
```

---

# 60. Practice 8 — Exit Status

Run:

```bash
pwd
echo $?
```

Then:

```bash
ls /wrong-directory
echo $?
```

