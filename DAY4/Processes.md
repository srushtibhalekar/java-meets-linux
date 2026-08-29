# 🐧 Day 04 — Linux Processes

## 🎯 Mission

**See what’s running. Control what runs.**

---

## 🔍 View Processes

```bash
ps
```

Show detailed processes:

```bash
ps aux
```

Live process monitoring:

```bash
top
```

---

## 🆔 Process ID

Every running process has a **PID (Process ID)**.

Find a process:

```bash
ps aux
```

---

## 🛑 Stop a Process

```bash
kill PID
```

Force stop:

```bash
kill -9 PID
```

Replace `PID` with the actual process ID.

---

## 🔎 Find a Process

```bash
ps aux | grep java
```

This searches for processes containing `java`.

---

## 🧪 Practice

Run:

```bash
ps aux
top
```

Find a process and note its PID.

Then practice:

```bash
ps aux | grep <process-name>
```

---

## 🏁 Day 4 Complete

**Processes visible.
Terminal in control. ⚡**
