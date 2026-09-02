# 🐧 Day 05 — Text Searching with `grep`

> **30 Days of Linux — Day 05**

## 🎯 Mission

**Search inside files instead of reading them line by line. 🔍**

---

# 1. What is `grep`?

`grep` is a Linux command used to **search for text patterns inside files or command output**.

Example:

```bash
grep "Linux" notes.txt
```

If `notes.txt` contains:

```text
Linux is powerful.
Windows is popular.
Linux is used on servers.
Linux is open source.
```

Output:

```text
Linux is powerful.
Linux is used on servers.
Linux is open source.
```

---

# 2. Basic Syntax

```bash
grep [options] "pattern" file
```

Example:

```bash
grep "Java" programming.txt
```

Breakdown:

```text
grep          → command
"Java"        → search pattern
programming.txt → file
```

---

# 3. Create a Practice File

Create a directory:

```bash
mkdir Day5-Practice
cd Day5-Practice
```

Create a file:

```bash
nano students.txt
```

Add:

```text
Alice Java Developer
Bob Python Developer
Charlie Java Developer
David Linux Administrator
Eva Java Developer
Frank Linux Engineer
Grace Python Developer
Henry DevOps Engineer
```

Save with:

```text
Ctrl + O
Enter
Ctrl + X
```

---

# 4. Basic `grep`

Search for `Java`:

```bash
grep "Java" students.txt
```

Output:

```text
Alice Java Developer
Charlie Java Developer
Eva Java Developer
```

---

# 5. Case-Sensitive Search

By default, `grep` is case-sensitive.

```bash
grep "java" students.txt
```

If the file contains `Java`, this may return nothing.

---

# 6. `grep -i` — Ignore Case

Use:

```bash
grep -i "java" students.txt
```

This can match:

```text
Java
java
JAVA
JaVa
```

Example:

```bash
grep -i "linux" students.txt
```

---

# 7. `grep -n` — Show Line Numbers

```bash
grep -n "Java" students.txt
```

Example:

```text
1:Alice Java Developer
3:Charlie Java Developer
5:Eva Java Developer
```

This tells you exactly where the match occurs.

---

# 8. `grep -v` — Invert the Search

Normally:

```bash
grep "Java" students.txt
```

shows lines containing `Java`.

With:

```bash
grep -v "Java" students.txt
```

you get lines that **do not contain** `Java`.

---

# 9. `grep -c` — Count Matches

```bash
grep -c "Java" students.txt
```

Example:

```text
3
```

This counts matching lines.

---

# 10. `grep -l` — Show Matching Filenames

Suppose you have:

```text
file1.txt
file2.txt
file3.txt
```

Search:

```bash
grep -l "Linux" *.txt
```

It displays filenames containing the pattern.

---

# 11. `grep -L` — Files Without a Match

```bash
grep -L "Linux" *.txt
```

This displays files that **do not contain** `Linux`.

---

# 12. Search Multiple Files

```bash
grep "Java" file1.txt file2.txt file3.txt
```

Output may include filenames:

```text
file1.txt:Java Developer
file3.txt:Java Engineer
```

---

# 13. Search All Files in a Directory

```bash
grep "Linux" *
```

This searches files in the current directory.

---

# 14. `grep -r` — Recursive Search

One of the most important options:

```bash
grep -r "Linux" .
```

This searches through:

* Current directory
* Subdirectories
* Files inside those directories

Example:

```bash
grep -r "TODO" ~/Projects
```

This is very useful when searching source-code projects.

---

# 15. `grep -R`

You may also see:

```bash
grep -R "Linux" .
```

`-R` performs recursive searching and follows symbolic links differently from `-r`.

For everyday directory searching, you'll commonly encounter both.

---

# 16. `grep -w` — Match Whole Words

Suppose a file contains:

```text
Java
JavaScript
Java Developer
```

Search:

```bash
grep "Java" file.txt
```

It can also match lines containing `JavaScript`.

To match the complete word:

```bash
grep -w "Java" file.txt
```

Now the intended whole-word matches are selected.

---

# 17. `grep -x` — Match the Entire Line

```bash
grep -x "Java" file.txt
```

This matches only a line that is exactly:

```text
Java
```

A line such as:

```text
Java Developer
```

will not match.

---

# 18. `grep -o` — Show Only the Matching Part

Example:

```bash
echo "Linux Linux Java" | grep -o "Linux"
```

Output:

```text
Linux
Linux
```

Instead of displaying the complete line, `-o` displays only the matching text.

---

# 19. Search for Multiple Patterns

You can use:

```bash
grep -E "Java|Python" students.txt
```

This searches for either:

```text
Java
```

or:

```text
Python
```

`-E` enables extended regular expressions.

---

# 20. `grep -E` — Extended Regular Expressions

Example:

```bash
grep -E "Java|Linux" students.txt
```

Another example:

```bash
grep -E "Developer|Engineer" students.txt
```

This is useful for searching multiple patterns.

---

# 21. Search Using Regular Expressions

Regular expressions allow more flexible searching.

Example:

```bash
grep "^Java" file.txt
```

`^` means:

**Beginning of the line**

---

# 22. `$` — End of Line

```bash
grep "Linux$" file.txt
```

This matches lines ending with:

```text
Linux
```

Example:

```text
I am learning Linux
Linux
```

Both lines end with `Linux`.

---

# 23. `.` — Any Character

In regular expressions:

```text
.
```

matches a single character.

Example:

```bash
grep "J.va" file.txt
```

This pattern can match:

```text
Java
Jova
J1va
```

depending on the actual text.

---

# 24. Character Classes

Example:

```bash
grep "[0-9]" file.txt
```

This searches for lines containing a digit.

Letters:

```bash
grep "[a-z]" file.txt
```

Uppercase letters:

```bash
grep "[A-Z]" file.txt
```

---

# 25. Search for Errors in Logs

This is one of the most practical uses of `grep`.

Example:

```bash
grep "ERROR" application.log
```

Case-insensitive:

```bash
grep -i "error" application.log
```

Search several related terms:

```bash
grep -Ei "error|failed|warning" application.log
```

---

# 26. Search Java Code

Suppose you have a Java project:

```text
src/
├── Main.java
├── Student.java
└── Login.java
```

Search for `public static void main`:

```bash
grep -r "public static void main" src/
```

Search for `TODO`:

```bash
grep -r "TODO" .
```

Search for `System.out.println`:

```bash
grep -r "System.out.println" .
```

---

# 27. Combine `grep` With Other Commands

You can search command output.

Example:

```bash
ls -la | grep ".txt"
```

This displays lines containing `.txt`.

Another:

```bash
ps aux | grep java
```

This can help identify running Java-related processes.

Pipes will be covered deeply on **Day 7**.

---

# 28. Search Environment Information

Example:

```bash
env | grep PATH
```

This searches the environment variables for `PATH`.

We will study environment variables properly on **Day 19**.

---

# 29. `grep` Options Cheat Sheet

| Option | Meaning                                            |
| ------ | -------------------------------------------------- |
| `-i`   | Ignore case                                        |
| `-n`   | Show line number                                   |
| `-v`   | Invert match                                       |
| `-c`   | Count matching lines                               |
| `-l`   | Show matching filenames                            |
| `-L`   | Show files without matches                         |
| `-r`   | Recursive search                                   |
| `-R`   | Recursive search with symlink behavior differences |
| `-w`   | Match whole word                                   |
| `-x`   | Match whole line                                   |
| `-o`   | Show only matching text                            |
| `-E`   | Extended regular expressions                       |

---

# 30. Practical Example

Create:

```bash
nano server.log
```

Add:

```text
INFO Server started
INFO Database connected
WARNING Memory usage high
ERROR Database connection failed
INFO User logged in
ERROR Authentication failed
WARNING Disk usage high
INFO Backup completed
```

### Find errors

```bash
grep "ERROR" server.log
```

### Find warnings

```bash
grep "WARNING" server.log
```

### Find errors and warnings

```bash
grep -E "ERROR|WARNING" server.log
```

### Ignore case

```bash
grep -i "error" server.log
```

### Show line numbers

```bash
grep -n "ERROR" server.log
```

### Count errors

```bash
grep -c "ERROR" server.log
```

---

# 31. Real-World Log Investigation

Imagine a server has thousands of log lines.

Instead of:

```bash
cat server.log
```

you can use:

```bash
grep -i "error" server.log
```

Then:

```bash
grep -i "failed" server.log
```

Then:

```bash
grep -Ei "error|failed|warning" server.log
```

This is much faster for troubleshooting.

---

# 32. `grep` + `find`

You can combine file searching and text searching.

Example:

```bash
find . -type f -name "*.log" -exec grep -H "ERROR" {} \;
```

This means:

1. Find `.log` files.
2. Search each file for `ERROR`.
3. Display matching lines.

This combines the skills from **Day 4 + Day 5**.

---

# 33. Important Difference

Remember:

### `find`

Searches for:

**files and directories**

Example:

```bash
find . -name "*.txt"
```

### `grep`

Searches for:

**text inside files or command output**

Example:

```bash
grep "Linux" notes.txt
```

Think:

```text
find → Where is the file?
grep → Where is the text?
```

---

