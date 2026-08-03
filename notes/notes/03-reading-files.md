# Lesson 03 - Reading Files in Linux

## Objective

A Cybersecurity professional reads files daily: logs, config files, password files, reports, and scripts. Linux provides several commands for reading files, each with a different purpose.

---

## Commands Learned

### cat

`cat` stands for concatenate, but it is mostly used to read a file.

Example:

```bash
cat notes.txt
```

Best for small files (5-10 lines). If the file has 20,000 lines, `cat` dumps everything at once, which is hard to read.

---

### less

Example:

```bash
less notes.txt
```

Unlike `cat`, `less` lets you scroll through the file gradually.

Navigation:
- Arrow Down / Arrow Up
- Space → next page
- q → quit

Cybersecurity example:

```bash
less /var/log/auth.log
```

---

### more

```bash
more notes.txt
```

Similar to `less`, but `less` is more powerful. Most Linux admins prefer `less`.

---

### head

Shows the beginning of a file.

```bash
head notes.txt
```

Default: first 10 lines. Custom amount:

```bash
head -5 notes.txt
```

Cybersecurity example:

```bash
head auth.log
```

---

### tail

Shows the end of a file.

```bash
tail notes.txt
```

Default: last 10 lines. Custom amount:

```bash
tail -20 notes.txt
```

New log entries always appear at the bottom, so this is used to check the latest activity:

```bash
tail auth.log
```

---

### tail -f (Live Monitoring)

```bash
tail -f auth.log
```

`-f` means follow — watch the log live. SIEM tools like Wazuh, Splunk, and Elastic are built on this same concept.

---

### nano

Linux does not have Notepad — it uses text editors instead.

```bash
nano notes.txt
```

Save and exit:
1. `CTRL + O` → save
2. `Enter` → confirm
3. `CTRL + X` → exit

---

## Comparison Table

| Command | Function |
|---|---|
| `cat` | Read a small file |
| `less` | Read a large file |
| `more` | Read a file page by page |
| `head` | View the beginning of a file |
| `tail` | View the end of a file |
| `tail -f` | Live log monitoring |
| `nano` | Edit a file |
