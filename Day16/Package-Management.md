# 📦 Day 16 — Linux Package Management

Package management is one of the most important Linux skills for developers and system administrators.

It helps you **install, update, remove, search, and manage software** from the terminal.

---

## 🎯 Today's Goals

By the end of Day 16, you will understand:

* What package management means
* What a Linux package is
* Package managers
* `apt`
* `apt-get`
* `dpkg`
* `dnf`
* `rpm`
* Package repositories
* Package dependencies
* Installing and removing software
* Checking installed packages
* Managing Java-related software
* Creating a package inventory

---

# 1. What Is a Package?

A **package** is a collection of files required to install software.

For example:

```text
Java
Git
curl
tree
unzip
vim
```

Instead of manually downloading and configuring every file, Linux package managers can handle this for you.

Example:

```bash
sudo apt install tree
```

Linux downloads the package and installs it.

---

# 2. What Is a Package Manager?

A **package manager** is a tool that manages software packages.

It can:

```text
Search
   ↓
Download
   ↓
Install
   ↓
Update
   ↓
Remove
```

Common package managers:

| Linux Family      | Package Manager |
| ----------------- | --------------- |
| Ubuntu/Debian     | `apt`           |
| Ubuntu/Debian     | `dpkg`          |
| Fedora/RHEL       | `dnf`           |
| RPM-based systems | `rpm`           |

If you are using **Ubuntu in WSL**, you will normally use `apt`.

---

# 3. Package Repositories

A **repository** is a location containing software packages.

When you run:

```bash
sudo apt install tree
```

Linux can obtain the package from configured repositories.

Think of a repository as:

```text
Linux Repository
       ↓
   Package
       ↓
    Download
       ↓
    Install
```

Repositories help provide software in a consistent and manageable way.

---

# 4. Check Your Linux Distribution

Before using package commands, check your Linux distribution:

```bash
cat /etc/os-release
```

Example:

```text
NAME="Ubuntu"
VERSION="24.04..."
```

You can also run:

```bash
uname -a
```

---

# 5. APT

`apt` is commonly used on Debian and Ubuntu systems.

Basic syntax:

```bash
apt command package-name
```

Commands that modify the system generally require:

```bash
sudo
```

Example:

```bash
sudo apt install tree
```

---

# 6. Update Package Lists

Use:

```bash
sudo apt update
```

Important:

`apt update` **does not normally upgrade your installed software**.

It refreshes information about available packages and versions.

Think:

```text
apt update
     ↓
Refresh package information
```

---

# 7. Upgrade Installed Packages

To upgrade installed packages:

```bash
sudo apt upgrade
```

Typical workflow:

```bash
sudo apt update
sudo apt upgrade
```

Meaning:

```text
update → refresh available package information

upgrade → install available upgrades
```

---

# 8. Install a Package

Syntax:

```bash
sudo apt install package-name
```

Example:

```bash
sudo apt install tree
```

Another example:

```bash
sudo apt install curl
```

Multiple packages can be installed together:

```bash
sudo apt install tree curl unzip
```

---

# 9. Check Whether a Command Exists

Before installing software, check whether it is already available:

```bash
which tree
```

or:

```bash
command -v tree
```

For Java:

```bash
which java
```

Then:

```bash
java --version
```

---

# 10. Remove a Package

To remove installed software:

```bash
sudo apt remove tree
```

This removes the package but can leave some configuration files behind.

---

# 11. Purge a Package

To remove the package and its system configuration files:

```bash
sudo apt purge tree
```

Difference:

```text
remove → remove package

purge → remove package + configuration files
```

Use purge carefully.

---

# 12. Remove Unused Dependencies

Use:

```bash
sudo apt autoremove
```

This can remove packages that were installed as dependencies but are no longer required.

Review what Linux proposes before confirming.

---

# 13. Search for Packages

Search the package repository:

```bash
apt search tree
```

Example:

```bash
apt search java
```

Another command:

```bash
apt-cache search java
```

---

# 14. View Package Information

Use:

```bash
apt show tree
```

You may see information such as:

```text
Package
Version
Architecture
Description
Dependencies
```

This is useful before installing software.

---

# 15. List Installed Packages

Use:

```bash
apt list --installed
```

You can search the output:

```bash
apt list --installed | grep java
```

Another useful command:

```bash
dpkg -l
```

---

# 16. What Is dpkg?

`dpkg` is the lower-level package management tool used by Debian-based systems.

APT works with repositories and dependencies, while `dpkg` works directly with Debian packages.

Debian packages usually have:

```text
.deb
```

Example:

```text
application.deb
```

---

# 17. Check a Package with dpkg

List installed packages:

```bash
dpkg -l
```

Check information about a specific installed package:

```bash
dpkg -s tree
```

Find files installed by a package:

```bash
dpkg -L tree
```

Example:

```bash
dpkg -L curl
```

---

# 18. Install a Local .deb File

If you have a local Debian package:

```text
application.deb
```

You can use:

```bash
sudo dpkg -i application.deb
```

If dependencies are missing, you may need:

```bash
sudo apt -f install
```

The `-f` option asks APT to attempt to fix dependency problems.

For normal repository installation, prefer:

```bash
sudo apt install package-name
```

---

# 19. apt vs apt-get

You may see both:

```bash
apt
```

and:

```bash
apt-get
```

Example:

```bash
sudo apt update
```

or:

```bash
sudo apt-get update
```

For interactive terminal use, `apt` provides a convenient user-facing interface.

`apt-get` is also widely used, especially in scripts and older documentation.

---

# 20. Package Dependencies

Software often depends on other software.

Example:

```text
Application
    ↓
Library A
    ↓
Library B
```

If you install the application manually, managing these dependencies can become difficult.

Package managers can resolve many dependencies automatically.

Example:

```bash
sudo apt install curl
```

APT determines required dependencies and installs them when needed.

---

# 21. Check Package Version

You can check available package versions with:

```bash
apt policy tree
```

For Java:

```bash
java --version
```

For Git:

```bash
git --version
```

For curl:

```bash
curl --version
```

---

# 22. Package Cache

APT stores downloaded package files in its cache.

You can remove downloaded package files that are no longer needed:

```bash
sudo apt clean
```

Another command:

```bash
sudo apt autoclean
```

Be careful when cleaning system package data.

---

# 23. Fedora / RHEL — DNF

Not every Linux distribution uses APT.

Fedora and modern RHEL-family systems commonly use:

```bash
dnf
```

Install:

```bash
sudo dnf install package-name
```

Search:

```bash
dnf search package-name
```

Remove:

```bash
sudo dnf remove package-name
```

Update:

```bash
sudo dnf update
```

Information:

```bash
dnf info package-name
```

---

# 24. RPM

RPM is the lower-level package system commonly associated with RPM-based distributions.

List installed RPM packages:

```bash
rpm -qa
```

Check package information:

```bash
rpm -qi package-name
```

List files installed by a package:

```bash
rpm -ql package-name
```

---

# 25. APT vs DNF vs dpkg vs rpm

| Tool      | Common Usage                                               |
| --------- | ---------------------------------------------------------- |
| `apt`     | Ubuntu/Debian package management                           |
| `apt-get` | Debian/Ubuntu package management, commonly used in scripts |
| `dpkg`    | Low-level Debian package management                        |
| `dnf`     | Fedora/RHEL-family package management                      |
| `rpm`     | Low-level RPM package management                           |

---

# 26. Important Rule ⚠️

Do not randomly mix package managers.

For example, if you are using Ubuntu:

```bash
sudo apt install tree
```

Do not try:

```bash
sudo dnf install tree
```

unless you are actually working on a distribution that uses DNF.

First check:

```bash
cat /etc/os-release
```

---

# 27. Java Developer Example

As a Java developer, you may need software such as:

```text
JDK
Git
Maven
curl
unzip
```

Check Java:

```bash
java --version
```

Check Git:

```bash
git --version
```

Check Maven:

```bash
mvn --version
```

Check curl:

```bash
curl --version
```

Check unzip:

```bash
unzip -v
```

---

# 28. Installing Development Tools

On Ubuntu, examples include:

```bash
sudo apt install git
```

```bash
sudo apt install maven
```

```bash
sudo apt install curl
```

```bash
sudo apt install unzip
```

You can install several together:

```bash
sudo apt install git maven curl unzip
```

Always review the packages and dependencies APT proposes before confirming.

---

# 29. Practice Lab

Create a practice directory:

```bash
mkdir ~/day16-package-lab
cd ~/day16-package-lab
```

Create a file:

```bash
touch package-notes.txt
```

Check your distribution:

```bash
cat /etc/os-release
```

Now check whether `tree` is installed:

```bash
command -v tree
```

If it is not installed:

```bash
sudo apt update
sudo apt install tree
```

Test it:

```bash
tree .
```

---

# 30. Package Search Lab

Search for Git:

```bash
apt search git
```

View information:

```bash
apt show git
```

Check installed Git:

```bash
dpkg -s git
```

Find Git's installed files:

```bash
dpkg -L git
```

Check the Git version:

```bash
git --version
```

---

# 31. Package Inventory Project 🛠️

Create a simple system package inventory.

Run:

```bash
mkdir ~/package-inventory
cd ~/package-inventory
```

Create a report:

```bash
{
echo "===== SYSTEM INFORMATION ====="
cat /etc/os-release

echo
echo "===== JAVA ====="
java --version 2>&1

echo
echo "===== GIT ====="
git --version

echo
echo "===== MAVEN ====="
mvn --version 2>&1 | head -n 1

echo
echo "===== CURL ====="
curl --version | head -n 1

echo
echo "===== INSTALLED JAVA PACKAGES ====="
apt list --installed 2>/dev/null | grep -i java

} > package-report.txt
```

View the report:

```bash
cat package-report.txt
```

---

# 32. Useful Package Troubleshooting

### Problem 1 — Package not found

Try:

```bash
sudo apt update
```

Then:

```bash
apt search package-name
```

---

### Problem 2 — Permission denied

If package installation requires administrative privileges:

```bash
sudo apt install package-name
```

---

### Problem 3 — Broken dependencies

A possible repair command is:

```bash
sudo apt -f install
```

Review the proposed changes carefully.

---

### Problem 4 — Is the package installed?

Use:

```bash
dpkg -s package-name
```

or:

```bash
apt list --installed | grep package-name
```

---

### Problem 5 — Which package installed a file?

If you know the installed file path:

```bash
dpkg -S /path/to/file
```

Example:

```bash
dpkg -S /usr/bin/curl
```

---

# 33. Important Safety Rules

### Rule 1

Don't blindly run:

```bash
sudo apt remove ...
```

Check what will be removed.

### Rule 2

Don't blindly copy commands from random websites.

### Rule 3

Be careful when adding third-party repositories.

### Rule 4

Don't remove important system packages just for practice.

### Rule 5

Use `sudo` only when administrative privileges are actually required.

---

# 34. Real-World Developer Workflow

A common workflow can look like:

```bash
sudo apt update
```

Then:

```bash
sudo apt upgrade
```

Install development tools:

```bash
sudo apt install git maven curl unzip
```

Verify:

```bash
git --version
mvn --version
curl --version
unzip -v
```

This is a practical example of how Linux package management supports a development environment.

---

# 35. Mini Challenge 🔥

Without looking at the notes, try to complete these:

### Task 1

Check your Linux distribution.

```text
?
```

### Task 2

Refresh package information.

```text
?
```

### Task 3

Search for the `tree` package.

```text
?
```

### Task 4

Install `tree`.

```text
?
```

### Task 5

Display package information.

```text
?
```

### Task 6

Check whether `tree` is installed.

```text
?
```

### Task 7

Remove `tree`.

```text
?
```

### Task 8

List installed packages.

```text
?
```

---

# 36. Interview Questions 🎯

### Q1. What is a package manager?

A tool used to install, update, remove, search, and manage software packages.

### Q2. What is APT?

APT is a package management system commonly used on Debian and Ubuntu.

### Q3. What does `apt update` do?

It refreshes information about available packages and versions from configured repositories.

### Q4. Does `apt update` upgrade installed packages?

No. `apt update` refreshes package information. `apt upgrade` performs available package upgrades.

### Q5. What is `dpkg`?

A low-level Debian package management tool that works with `.deb` packages.

### Q6. What is DNF?

DNF is a package manager commonly used by Fedora and other RPM-based Linux systems.

### Q7. What is a package dependency?

A software component required by another package for it to work correctly.

### Q8. Difference between `apt remove` and `apt purge`?

`remove` removes the package, while `purge` also removes associated system configuration files.

### Q9. How do you check installed packages?

```bash
dpkg -l
```

or:

```bash
apt list --installed
```

### Q10. How do you check the files installed by a Debian package?

```bash
dpkg -L package-name
```

---

# 37. Day 16 Cheat Sheet

```text
Linux Distribution
cat /etc/os-release

APT
sudo apt update
sudo apt upgrade
sudo apt install package
sudo apt remove package
sudo apt purge package
sudo apt autoremove

Search
apt search package
apt show package
apt policy package

Installed Packages
apt list --installed
dpkg -l

dpkg
dpkg -s package
dpkg -L package
dpkg -i file.deb
dpkg -S /path/to/file

Cache
sudo apt clean
sudo apt autoclean

DNF
sudo dnf install package
sudo dnf remove package
sudo dnf update
dnf search package
dnf info package

RPM
rpm -qa
rpm -qi package
rpm -ql package
```

---

# 🧠 Day 16 Key Takeaway

Remember these five commands first:

```bash
sudo apt update
sudo apt upgrade
sudo apt install package
sudo apt remove package
apt search package
```

And remember:

```text
apt → high-level Debian/Ubuntu package management
dpkg → low-level Debian package management
dnf → Fedora/RHEL-family package management
rpm → low-level RPM package management
```

---



