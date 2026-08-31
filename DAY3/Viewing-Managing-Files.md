# 🐧 Day 03 — Viewing & Managing Files

> **30 Days of Linux — Day 03**

## 🎯 Mission

**Don't just store files. Learn to inspect what is inside them. 👀**

---

# 1. Why View Files?

Before modifying or deleting a file, you should know:

* What type of file it is
* What content it contains
* How large it is
* When it was modified
* What is at the beginning or end of the file

Linux provides several commands for this.

---

# 2. `cat` — Display File Content

Create a test file:

```bash
touch notes.txt
```

Add some text:

```bash
echo "Linux Day 3" > notes.txt
```

View it:

```bash
cat notes.txt
```

Output:

```text
Linux Day 3
```

---

# 3. `cat` With Multiple Files

Create two files:

```bash
echo "Hello Linux" > file1.txt
echo "Hello Terminal" > file2.txt
```

View both:

```bash
cat file1.txt file2.txt
```

Output:

```text
Hello Linux
Hello Terminal
```

---

# 4. `cat -n` — Show Line Numbers

```bash
cat -n notes.txt
```

Example:

```text
     1  Linux Day 3
     2  File Management
     3  Learning Linux
```

This is useful when you need to identify specific lines.

---

# 5. `less` — Read Large Files

For a large file, `cat` can flood the terminal.

Use:

```bash
less notes.txt
```

Useful keys inside `less`:

| Key     | Action        |
| ------- | ------------- |
| `Space` | Next page     |
| `b`     | Previous page |
| `↑`     | Move up       |
| `↓`     | Move down     |
| `/text` | Search        |
| `q`     | Exit          |

Example:

```bash
less /etc/services
```

---

# 6. `more` — Simple Page-by-Page Viewing

```bash
more notes.txt
```

`more` displays content one page at a time.

`less` is generally more powerful and flexible.

---

# 7. `head` — View Beginning of a File

```bash
head notes.txt
```

By default, `head` displays the first **10 lines**.

Specify the number of lines:

```bash
head -n 5 notes.txt
```

This displays the first 5 lines.

---

# 8. `tail` — View End of a File

```bash
tail notes.txt
```

By default, it displays the last **10 lines**.

Show the last 5 lines:

```bash
tail -n 5 notes.txt
```

---

# 9. `tail -f` — Follow a File

One of the most useful Linux commands for monitoring logs:

```bash
tail -f application.log
```

It keeps running and displays new lines as they are added to the file.

Stop it with:

```text
Ctrl + C
```

This becomes especially useful when we study **Linux logs on Day 26**.

---

# 10. `file` — Identify File Type

Run:

```bash
file notes.txt
```

Example:

```text
notes.txt: ASCII text
```

Try it with different files:

```bash
file /etc/hosts
```

```bash
file /bin/bash
```

The command examines the file and reports what type of data it contains.

---

# 11. `wc` — Count Information

`wc` means **word count**.

Basic:

```bash
wc notes.txt
```

It can show:

```text
lines words bytes
```

### Count lines

```bash
wc -l notes.txt
```

### Count words

```bash
wc -w notes.txt
```

### Count characters

```bash
wc -m notes.txt
```

### Count bytes

```bash
wc -c notes.txt
```

---

# 12. `du` — Check File Size

Check the size of a file:

```bash
du notes.txt
```

Human-readable:

```bash
du -h notes.txt
```

Check a directory:

```bash
du -h Documents/
```

Summarize a directory:

```bash
du -sh Documents/
```

---

# 13. `stat` — Detailed File Information

Run:

```bash
stat notes.txt
```

You can see information such as:

* File type
* Size
* Permissions
* Owner
* Access time
* Modification time
* Change time

Example:

```text
File: notes.txt
Size: 25
Access: ...
Modify: ...
Change: ...
```

---

# 14. `nano` — Edit a File

Linux provides terminal text editors.

A beginner-friendly option is:

```bash
nano notes.txt
```

Type:

```text
Linux is powerful.
Linux is flexible.
Linux is everywhere.
```

### Useful Nano shortcuts

| Shortcut   | Action     |
| ---------- | ---------- |
| `Ctrl + O` | Save       |
| `Ctrl + X` | Exit       |
| `Ctrl + W` | Search     |
| `Ctrl + K` | Cut line   |
| `Ctrl + U` | Paste line |

When saving:

```text
Ctrl + O
Enter
```

Then exit:

```text
Ctrl + X
```

---

# 15. `vim` / `vi`

Another common Linux editor is:

```bash
vim notes.txt
```

or:

```bash
vi notes.txt
```

Vim is extremely powerful but has a steeper learning curve.

For now, focus on:

```bash
nano
```

We can learn Vim separately later.

---

# 16. `sort` — Sort Lines

Create:

```bash
printf "banana\napple\nmango\norange\n" > fruits.txt
```

View:

```bash
cat fruits.txt
```

Sort:

```bash
sort fruits.txt
```

Output:

```text
apple
banana
mango
orange
```

---

# 17. `sort -r` — Reverse Sorting

```bash
sort -r fruits.txt
```

Output:

```text
orange
mango
banana
apple
```

---

# 18. `uniq` — Remove Duplicate Lines

Create:

```bash
printf "apple\napple\nbanana\nbanana\norange\n" > fruits.txt
```

Run:

```bash
uniq fruits.txt
```

Output:

```text
apple
banana
orange
```

### Important

`uniq` removes **adjacent duplicate lines**.

For a complete duplicate-removal workflow:

```bash
sort fruits.txt | uniq
```

Pipes will be covered deeply on **Day 7**.

---

# 19. `cut` — Extract Parts of Text

Suppose:

```text
Alice:Developer
Bob:Tester
Charlie:Admin
```

Create:

```bash
printf "Alice:Developer\nBob:Tester\nCharlie:Admin\n" > users.txt
```

Extract the first field:

```bash
cut -d ':' -f 1 users.txt
```

Output:

```text
Alice
Bob
Charlie
```

Here:

```text
-d ':' → delimiter
-f 1   → first field
```

---

# 20. `tr` — Transform Characters

Convert lowercase to uppercase:

```bash
echo "linux" | tr 'a-z' 'A-Z'
```

Output:

```text
LINUX
```

Replace characters:

```bash
echo "hello-linux" | tr '-' '_'
```

Output:

```text
hello_linux
```

---

# 21. `nl` — Number Lines

Another way to display numbered lines:

```bash
nl notes.txt
```

Example:

```text
     1  Linux is powerful.
     2  Linux is flexible.
     3  Linux is everywhere.
```

Compare:

```bash
cat -n notes.txt
```

and:

```bash
nl notes.txt
```

---

# 22. `touch` and Modification Time

Although `touch` was introduced on Day 2, it is useful to understand what it does.

```bash
touch notes.txt
```

If the file already exists, `touch` updates its timestamps.

Check:

```bash
stat notes.txt
```

---

# 23. `cp`, `mv`, and `rm` — Review

These were introduced on Day 2.

### Copy

```bash
cp notes.txt backup.txt
```

### Move

```bash
mv backup.txt Documents/
```

### Rename

```bash
mv notes.txt linux-notes.txt
```

### Delete

```bash
rm linux-notes.txt
```

Day 3 focuses more on **inspecting files**, while Day 2 focused on filesystem organization.

---

# 24. File Inspection Workflow

A useful Linux workflow is:

```text
        File
         │
         ▼
       file
         │
         ▼
       stat
         │
         ▼
        cat
         │
         ▼
       head / tail
         │
         ▼
       less
```

For example:

```bash
file notes.txt
stat notes.txt
head notes.txt
tail notes.txt
less notes.txt
```

---

# 25. Practice Project

Create a directory:

```bash
mkdir Day3-Practice
cd Day3-Practice
```

Create a file:

```bash
nano system-info.txt
```

Add:

```text
Linux Day 3
File Viewing Practice
Learning Linux Commands
Terminal Skills
System Administration
```

Save and exit.

---

# 26. Inspect the File

Check the content:

```bash
cat system-info.txt
```

Number the lines:

```bash
cat -n system-info.txt
```

Check the file type:

```bash
file system-info.txt
```

Count lines:

```bash
wc -l system-info.txt
```

Count words:

```bash
wc -w system-info.txt
```

Check size:

```bash
du -h system-info.txt
```

Get detailed information:

```bash
stat system-info.txt
```

---

# 27. Create a Larger Practice File

Run:

```bash
for i in {1..20}; do echo "Linux Line $i"; done > large.txt
```

View the first lines:

```bash
head large.txt
```

View the last lines:

```bash
tail large.txt
```

View only the first 5:

```bash
head -n 5 large.txt
```

View only the last 5:

```bash
tail -n 5 large.txt
```

Read page by page:

```bash
less large.txt
```

Exit:

```text
q
```

---

# 28. Useful Command Summary

| Command   | Purpose                            |
| --------- | ---------------------------------- |
| `cat`     | Display file contents              |
| `cat -n`  | Display contents with line numbers |
| `less`    | Read files page by page            |
| `more`    | Basic page-by-page viewing         |
| `head`    | Show beginning of file             |
| `tail`    | Show end of file                   |
| `tail -f` | Follow a changing file             |
| `file`    | Identify file type                 |
| `wc`      | Count lines, words, bytes          |
| `du`      | Show disk usage                    |
| `stat`    | Detailed file information          |
| `nano`    | Edit text files                    |
| `sort`    | Sort lines                         |
| `uniq`    | Remove adjacent duplicates         |
| `cut`     | Extract fields                     |
| `tr`      | Transform characters               |
| `nl`      | Number lines                       |

---

# 🧠 What I Learned Today

* How to read file contents
* How to inspect large files
* How to view the beginning and end of files
* How to identify file types
* How to inspect file metadata
* How to count lines, words, and bytes
* How to check disk usage
* How to edit files from the terminal
* How to sort and process text
* How to monitor changing files with `tail -f`

