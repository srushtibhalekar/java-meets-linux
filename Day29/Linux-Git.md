# 🐙 Day 29 — Linux + Git

## 🎯 Learning Objectives

By the end of Day 29, you will understand:

* What Git is and why developers use it
* Git installation on Linux
* `git init` and `git clone`
* `git status`, `git add`, and `git commit`
* `git log` and `git diff`
* Branches and merging
* `git pull` and `git push`
* Connecting GitHub using SSH
* `.gitignore`
* Handling merge conflicts
* Managing Java projects with Git
* Practical Linux + Git workflow

---

# 1. What Is Git?

Git is a **version control system** that tracks changes in your code and files.

For example, imagine you are developing a Java project.

```text
Banking Management System
        ↓
Version 1
        ↓
Add JDBC
        ↓
Version 2
        ↓
Add Transaction History
        ↓
Version 3
```

Git helps you track these changes and return to earlier versions when needed.

## Why do developers use Git?

* Track code changes
* Save project versions
* Work with teams
* Create branches for new features
* Collaborate through GitHub
* Restore previous versions
* Review code before merging

---

# 2. Git vs GitHub

These are different things.

| Git                      | GitHub                                       |
| ------------------------ | -------------------------------------------- |
| Version control software | Online platform for hosting Git repositories |
| Works locally            | Stores and shares repositories online        |
| Tracks file changes      | Supports collaboration and pull requests     |
| Can work offline         | Requires internet for remote operations      |

Remember:

```text
Git     = Version Control
GitHub  = Online Repository Hosting
```

---

# 3. Install Git on Linux

First, check whether Git is installed.

```bash
git --version
```

Example output:

```text
git version 2.x.x
```

The exact version may differ.

If Git is missing, install it using the package manager for your distribution.

### Ubuntu/Debian

```bash
sudo apt update
sudo apt install git
```

### Fedora

```bash
sudo dnf install git
```

Verify:

```bash
git --version
```

---

# 4. Configure Git

Before creating commits, configure your name and email.

```bash
git config --global user.name "Srushti Bhalekar"
```

```bash
git config --global user.email "your-email@example.com"
```

Replace the example email with the email you want associated with your commits.

Check the configuration:

```bash
git config --global --list
```

You can check individual values:

```bash
git config --global user.name
```

```bash
git config --global user.email
```

**Important:** The name and email identify your commits. They are not your GitHub password.

---

# 5. Understand a Git Repository

A Git repository contains your project files and their version history.

Example:

```text
Java-Meets-Linux/
│
├── DAY1/
├── DAY2/
├── DAY28/
├── README.md
└── .git/
```

The `.git` directory stores Git's internal repository data.

Check hidden files:

```bash
ls -la
```

You may see `.git` when you are inside a Git repository.

Do not delete `.git` if you want to preserve the repository's history.

---

# 6. Create a New Repository with git init

Suppose you want to create a new Linux practice project.

```bash
mkdir Linux-Git-Practice
```

Enter the directory:

```bash
cd Linux-Git-Practice
```

Initialize Git:

```bash
git init
```

Git creates a local repository.

Check:

```bash
git status
```

You may see a message saying there are no commits yet.

### Remember

```text
git init
    ↓
Creates a local Git repository
```

It does not automatically create a repository on GitHub.

---

# 7. Create Your First File

Create a README file:

```bash
echo "# Linux Git Practice" > README.md
```

View it:

```bash
cat README.md
```

Expected output:

```text
# Linux Git Practice
```

Check the repository:

```bash
git status
```

You should see `README.md` listed as an untracked file.

---

# 8. Understand the Three Main Git Areas

Git uses three important areas.

```text
Working Directory
       ↓
Staging Area
       ↓
Local Repository
```

### Working directory

Your actual project files.

### Staging area

The changes you have selected for your next commit.

### Local repository

The committed history stored by Git.

Example:

```bash
git add README.md
```

This stages the file.

Then:

```bash
git commit -m "docs: add project README"
```

This saves the staged changes in your local repository.

---

# 9. git status

Use:

```bash
git status
```

It tells you:

* Which files are modified
* Which files are untracked
* Which files are staged
* Whether your branch is ahead of or behind its remote

Run this command frequently. It is one of the most useful Git commands for beginners.

---

# 10. git add

Stage a particular file:

```bash
git add README.md
```

Stage several files:

```bash
git add README.md Day29.md
```

Stage all changes under the current directory:

```bash
git add .
```

Check:

```bash
git status
```

**Important:** `git add .` may stage more files than you intend. Check the status before committing.

For your Linux roadmap, prefer a specific path when you know which file you want to add.

---

# 11. git commit

A commit records a snapshot of the staged changes.

Example:

```bash
git commit -m "docs: add Linux Git practice"
```

A good commit message describes the change.

Examples:

```text
docs: add day 29 Linux and Git notes
feat: add Java calculator
fix: correct file path
refactor: simplify input validation
```

After committing, run:

```bash
git status
```

If everything is committed, Git will usually report a clean working tree.

---

# 12. git log

View the commit history:

```bash
git log
```

For a shorter view:

```bash
git log --oneline
```

Show a graph of branches:

```bash
git log --oneline --graph --decorate --all
```

This is useful when you need to understand which changes have been committed and how branches relate to each other.

---

# 13. git diff

`git diff` helps you inspect changes.

Show unstaged changes:

```bash
git diff
```

Show staged changes:

```bash
git diff --staged
```

Compare two commits:

```bash
git diff COMMIT1 COMMIT2
```

Replace `COMMIT1` and `COMMIT2` with actual commit IDs.

Before committing a large change, review the diff to make sure you are not accidentally including unwanted content.

---

# 14. Clone a GitHub Repository

`git clone` downloads an existing repository.

Example:

```bash
git clone https://github.com/srushtibhalekar/java-meets-linux.git
```

Enter the directory:

```bash
cd java-meets-linux
```

Check:

```bash
git status
```

View the remote:

```bash
git remote -v
```

**Important:** Clone the repository only if you need another local copy. If you already have the repository on your computer, use its existing folder instead.

---

# 15. What Is a Remote?

A remote is a named reference to another Git repository.

The common remote name is:

```text
origin
```

Check your remote:

```bash
git remote -v
```

For example, you may see the GitHub URL for your repository.

Add a remote if your local repository does not already have one:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Replace the placeholders with your actual repository details.

If `origin` already exists, do not add it again. Check the current URL first.

---

# 16. Push Code to GitHub

The general workflow is:

```text
Modify Files
     ↓
git status
     ↓
git add
     ↓
git commit
     ↓
git push
     ↓
GitHub
```

Push your current branch:

```bash
git push
```

For a new branch that has no upstream configured, you may need:

```bash
git push -u origin main
```

The `-u` option sets the upstream branch so future pushes can usually use `git push`.

Your local commit and your GitHub repository are separate until you push.

---

# 17. Pull Changes from GitHub

To download and integrate changes from the configured upstream branch:

```bash
git pull
```

This is commonly used before continuing work when another commit may have been added to GitHub.

You can also specify the remote and branch:

```bash
git pull origin main
```

If you have uncommitted changes, check `git status` first. Commit or safely stash your work before pulling if the incoming changes may conflict with it.

---

# 18. Fetch vs Pull

These commands are related but different.

### git fetch

```bash
git fetch
```

Downloads remote updates but does not integrate them into your current branch.

### git pull

```bash
git pull
```

Fetches updates and then integrates them into your current branch, normally using a merge or rebase depending on configuration.

Remember:

```text
fetch = Download updates
pull  = Download + integrate updates
```

---

# 19. What Is a Git Branch?

A branch allows you to work on a feature without immediately changing the main development line.

Example:

```text
main
 |
 ├── feature/login
 |
 └── feature/report
```

For example, you can add a new section to your Linux notes in a separate branch and merge it after reviewing the changes.

---

# 20. Create a Branch

Create a branch:

```bash
git branch feature/linux-git
```

List branches:

```bash
git branch
```

Switch to it:

```bash
git switch feature/linux-git
```

You can create and switch in one command:

```bash
git switch -c feature/linux-git
```

This creates the branch if it does not already exist and switches to it.

---

# 21. Merge a Branch

Suppose you finished working on `feature/linux-git`.

First, make sure your changes are committed.

Switch to the branch you want to merge into:

```bash
git switch main
```

Merge the feature branch:

```bash
git merge feature/linux-git
```

Git attempts to integrate the branch's changes into `main`.

Inspect the result:

```bash
git status
git log --oneline --graph --decorate --all
```

Do not merge unfinished work blindly. Review and test it first.

---

# 22. Delete a Branch

After merging a feature branch, you can delete the local branch:

```bash
git branch -d feature/linux-git
```

The `-d` option protects against deleting a branch that Git considers unmerged.

To delete a remote branch:

```bash
git push origin --delete feature/linux-git
```

Only do this when you intend to remove that branch from GitHub.

---

# 23. What Is a Merge Conflict?

A merge conflict occurs when Git cannot automatically reconcile changes.

For example:

```text
main branch:
Hello Linux

feature branch:
Hello Linux and Git
```

If both branches change the same lines differently, Git may need you to decide what the final content should be.

You might see markers like:

```text
<<<<<<< HEAD
Content from the current branch
=======
Content from the incoming branch
>>>>>>> feature/linux-git
```

These markers are temporary conflict indicators, not valid final content for most files.

---

# 24. Resolve a Merge Conflict

Follow these steps:

1. Run `git status` to find conflicted files.
2. Open each conflicted file.
3. Decide which changes to keep.
4. Remove the conflict markers.
5. Save the file.
6. Test the result.
7. Stage the resolved file.
8. Complete the merge commit if Git requires one.

Example:

```bash
git status
```

After resolving `README.md`:

```bash
git add README.md
```

Then, if Git has not already completed the merge:

```bash
git commit
```

Do not delete conflict markers without understanding which content should remain.

---

# 25. What Is .gitignore?

`.gitignore` tells Git which untracked files it should ignore.

This is important for Java projects because compiled files and local configuration often should not be committed.

Create a file named:

```text
.gitignore
```

Example for a Java project:

```gitignore
# Compiled Java files
*.class

# Build output
target/
build/
out/

# IDE settings
.idea/
.vscode/

# Operating system files
.DS_Store
Thumbs.db

# Local environment secrets
.env
*.pem
```

Only include ignore patterns that suit your project. For example, a team may intentionally commit selected `.vscode` settings.

**Important:** `.gitignore` does not automatically stop tracking a file that Git already tracks. You may need to remove it from the index separately.

---

# 26. Stop Tracking a File Without Deleting It Locally

Suppose Git is already tracking a generated file.

To remove it from Git's index while keeping the local copy:

```bash
git rm --cached filename.class
```

Then commit the change:

```bash
git add .gitignore
git commit -m "chore: ignore generated files"
```

Use the actual file path in place of `filename.class`.

For a tracked directory, `git rm -r --cached DIRECTORY` removes it from the index recursively while keeping the local files.

Check carefully before using these commands.

---

# 27. Git with Java Projects

A Java project might look like:

```text
BankingManagementSystem/
├── src/
│   └── Main.java
├── README.md
├── .gitignore
└── target/
```

Usually, you want to track:

```text
src/
README.md
.gitignore
```

You normally do not want to track:

```text
*.class
target/
build/
```

unless your project has a specific reason to include generated output.

For a Maven project, the `target/` directory is usually generated during builds.

---

# 28. Java Developer Git Workflow

A typical workflow:

```text
Write Java Code
       ↓
Compile and Test
       ↓
git status
       ↓
git diff
       ↓
git add
       ↓
git commit
       ↓
git push
```

Example:

```bash
javac Main.java
java Main
```

Then:

```bash
git status
git diff
git add Main.java
git commit -m "feat: add Java application"
git push
```

Make sure the program works before committing it.

---

# 29. Connect GitHub Using SSH

SSH authentication lets Git communicate with GitHub using an SSH key.

Check whether you already have a key:

```bash
ls -la ~/.ssh
```

If needed, generate one:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Replace the example email with your GitHub-associated email if desired.

Display the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Add the public key to the SSH keys section of your GitHub account.

Never upload or share the private key.

Test the connection:

```bash
ssh -T git@github.com
```

The first connection may ask you to verify GitHub's host key. Verify it using GitHub's published SSH key fingerprints before accepting it.

A successful authentication message confirms that SSH authentication worked; it does not provide an interactive GitHub shell.

---

# 30. HTTPS vs SSH Remote URLs

A GitHub remote can use HTTPS:

```text
https://github.com/USERNAME/REPOSITORY.git
```

or SSH:

```text
git@github.com:USERNAME/REPOSITORY.git
```

Check the current remote:

```bash
git remote -v
```

If you want to switch an existing repository to SSH:

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Replace the placeholders with the correct username and repository name.

Make sure SSH authentication works before switching the remote URL.

---

# 31. Practical Project — Linux Git Practice

## Goal

Create a small repository and practice the complete Git workflow.

### Step 1 — Create a project

```bash
mkdir Linux-Git-Practice
cd Linux-Git-Practice
git init
```

### Step 2 — Configure Git if needed

```bash
git config --global user.name "Srushti Bhalekar"
git config --global user.email "your-email@example.com"
```

### Step 3 — Create a README

```bash
echo "# Linux Git Practice" > README.md
```

### Step 4 — Create a .gitignore

```bash
printf "*.class\ntarget/\n.env\n" > .gitignore
```

### Step 5 — Inspect changes

```bash
git status
git diff
```

### Step 6 — Stage files

```bash
git add README.md .gitignore
```

### Step 7 — Commit

```bash
git commit -m "docs: initialize Linux Git practice"
```

### Step 8 — Inspect history

```bash
git log --oneline
```

### Step 9 — Create a feature branch

```bash
git switch -c feature/add-notes
```

Add a new section to `README.md`, then:

```bash
git add README.md
git commit -m "docs: add Git practice notes"
```

### Step 10 — Merge into main

```bash
git switch main
git merge feature/add-notes
```

If your initial branch has another name, use the actual branch name shown by `git branch`.

### Step 11 — Review

```bash
git status
git log --oneline --graph --decorate --all
```

You have now practiced repository creation, staging, commits, branching, merging, and history inspection.

---

# 32. Your Java-Meets-Linux Repository Workflow

Your existing repository is:

```text
C:\Desktop\Java-Meets-Linux
```

On Windows, open PowerShell and run:

```powershell
cd C:\Desktop\Java-Meets-Linux
```

Check:

```powershell
git status
```

Review changes:

```powershell
git diff
```

Stage the Day 29 document:

```powershell
git add Day29\Linux-Git.md
```

Commit:

```powershell
git commit -m "docs: add day 29 Linux and Git"
```

Push:

```powershell
git push
```

These PowerShell commands work with your existing Windows repository. You do not need to clone it again.

---

# 33. Common Git Errors

## Error 1 — Nothing to commit

Check:

```bash
git status
```

Possible reasons:

* No files have changed.
* Your changes were already committed.
* You forgot to save the file.

## Error 2 — Authentication failed

Check:

```bash
git remote -v
```

Verify that the remote URL is correct and that you have valid HTTPS credentials or working SSH authentication.

## Error 3 — Push rejected

Someone may have pushed changes that your local branch does not have.

Inspect:

```bash
git status
git log --oneline --graph --decorate --all
```

Then integrate the remote changes carefully before pushing again.

## Error 4 — Not a Git repository

You may be in the wrong directory.

Check:

```bash
pwd
ls -la
```

Navigate to the correct repository.

## Error 5 — Accidentally staged the wrong file

Unstage a file without deleting it:

```bash
git restore --staged filename
```

Replace `filename` with the actual path.

---

# 34. Useful Git Commands Cheat Sheet

| Command                     | Purpose                                  |
| --------------------------- | ---------------------------------------- |
| `git init`                  | Initialize a repository                  |
| `git clone URL`             | Clone a repository                       |
| `git status`                | Inspect working tree                     |
| `git add FILE`              | Stage a file                             |
| `git commit -m "message"`   | Create a commit                          |
| `git log --oneline`         | View compact history                     |
| `git diff`                  | View unstaged changes                    |
| `git diff --staged`         | View staged changes                      |
| `git branch`                | List branches                            |
| `git switch BRANCH`         | Switch branches                          |
| `git switch -c BRANCH`      | Create and switch branches               |
| `git merge BRANCH`          | Merge a branch                           |
| `git fetch`                 | Fetch remote updates                     |
| `git pull`                  | Fetch and integrate updates              |
| `git push`                  | Push commits                             |
| `git remote -v`             | View remote URLs                         |
| `git restore --staged FILE` | Unstage a file                           |
| `git stash`                 | Temporarily stash changes                |
| `git show`                  | Inspect a commit                         |
| `git rm --cached FILE`      | Stop tracking a file but keep it locally |

---

# 🎤 Interview Questions

### Q1. What is Git?

Git is a distributed version control system used to track changes in source code and collaborate with other developers.

### Q2. What is the difference between Git and GitHub?

Git is the version control tool. GitHub is an online platform for hosting Git repositories and collaborating.

### Q3. What is a repository?

A repository contains project files and their version history.

### Q4. What is the difference between `git add` and `git commit`?

`git add` stages selected changes. `git commit` records staged changes in the local repository.

### Q5. What is a branch?

A branch is an independent line of development that allows work on a feature without immediately changing another branch.

### Q6. What is the difference between `git fetch` and `git pull`?

`git fetch` downloads remote updates without integrating them into the current branch. `git pull` fetches and then integrates them.

### Q7. What is `.gitignore`?

It specifies patterns for untracked files that Git should ignore.

### Q8. What is a merge conflict?

A merge conflict happens when Git cannot automatically combine changes and requires a person to resolve them.

### Q9. What is the purpose of `git log`?

It displays commit history.

### Q10. How do you upload changes to GitHub?

```bash
git add .
git commit -m "describe changes"
git push
```

For safer everyday use, inspect `git status` before staging and committing.

### Q11. Why should `.class` files usually be ignored in Java projects?

They are compiled output generated from Java source files and can usually be regenerated.

### Q12. How do you check which remote repository is configured?

```bash
git remote -v
```

---

# 🧠 Day 29 Quick Revision

```text
Git
 ↓
Track Changes
 ↓
Stage Changes
 ↓
Commit Changes
 ↓
Push to GitHub
```

Remember these commands:

```bash
git status
git add FILE
git commit -m "message"
git log --oneline
git diff
git branch
git switch
git merge
git pull
git push
```

For Java projects:

```text
Source Code → Test → Git Commit → GitHub
```

For Linux administration:

```text
Linux Terminal → Git → GitHub → Collaboration
```

---

