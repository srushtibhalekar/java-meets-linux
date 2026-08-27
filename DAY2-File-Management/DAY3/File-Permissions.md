# 🐧 Day 03 — Linux File Permissions

## 🎯 Mission

**Understand who can access what.**

---

## 🔐 Permission Basics

Linux permissions control:

* 👤 **User** — owner
* 👥 **Group** — group members
* 🌍 **Others** — everyone else

Check permissions:

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

### Permission Symbols

| Symbol | Meaning       |
| ------ | ------------- |
| `r`    | Read          |
| `w`    | Write         |
| `x`    | Execute       |
| `-`    | No permission |

---

## 🛠️ chmod

Change file permissions:

```bash
chmod +x script.sh
```

Give the owner read/write/execute permission:

```bash
chmod 700 script.sh
```

---

## 👀 Check Permissions

```bash
ls -l
```

---

## 🧪 Practice

```bash
touch test.sh
ls -l test.sh
chmod +x test.sh
ls -l test.sh
```

Notice how the permissions change after `chmod +x`.

---

## 🎯 Day 3 Complete

**Permissions understood.
Access controlled. 🔐**
