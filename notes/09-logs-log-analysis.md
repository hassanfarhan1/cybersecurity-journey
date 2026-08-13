# Lesson 09 - Linux Logs & Log Analysis

## Objective

Understand what a log is, where Linux stores logs, how to read them, and how to search them for security-relevant events. Reading logs is one of the core daily tasks of a SOC Analyst.

---

## Commands Learned

### What is a Log?

A log is a record the system keeps whenever something happens - a user login, an SSH connection, a service starting or stopping, a failed authentication, a system error.

Think of it like a security camera for the computer: it tells you who did what, and when.

---

### Where Linux Stores Logs - /var/log/

Most Linux distributions store logs in:

```bash
ls -lah /var/log/
```

Typical contents may include auth.log, syslog, kern.log, dpkg.log, or an apache2/ folder - but not every Linux distribution has the same files. Always check what your specific system actually uses.

---

### auth.log

```text
/var/log/auth.log
```

This file normally covers authentication and authorization events - failed passwords, accepted passwords, sudo usage, SSH activity. If an attacker attempts an SSH brute-force, auth.log is often the key place to investigate.

Important finding on this Kali system: `less /var/log/auth.log` returned "No such file or directory." This system does not use a standalone auth.log file - instead, authentication and system events are stored in the systemd journal (/var/log/journal/), read using journalctl.

Lesson: Don't assume every Linux system has the same log files - first check what logging system it actually uses.

---

### Reading Log Files

```bash
cat /var/log/auth.log
```

`cat` dumps the whole file at once - not ideal for large logs.

```bash
less /var/log/auth.log
```

`less` lets you scroll through a large file gradually.

```bash
tail -n 20 /var/log/auth.log
```

Shows only the most recent 20 lines - useful when you want the latest events.

---

### grep - Searching Logs

```bash
grep "Failed password" /var/log/auth.log
```

Searches for failed authentication attempts.

```bash
grep "Accepted" /var/log/auth.log
```

Searches for successful authentication events.

The SOC mindset:

```text
Log -> Search -> Find suspicious event -> Investigate -> Determine what happened
```

Example pattern: 10 failed SSH logins from the same IP in a short time period suggests possible brute-force activity. This is where Linux knowledge connects directly to SOC work.

---

### journalctl - Modern Linux Logging

Since this system stores logs in the systemd journal rather than flat files like auth.log, journalctl is the primary tool.

```bash
journalctl -n 20
```

Shows the 20 most recent journal entries (-n = number of entries). Useful right after an alert, to see what happened most recently.

```bash
journalctl -p err
```

Shows only logs at error priority or worse (-p = priority). Important caveat: an error is not automatically an attack. An error like "sound device failed" could be a driver, hardware, or configuration issue - it must be investigated before being labeled security-related.

```bash
journalctl --list-boots
```

Lists the boot sessions the journal has recorded, e.g.:

```text
-2   Aug 12 11:31
-1   Aug 12 11:32
 0   Aug 12 11:33
```

-2 and -1 are previous boots, 0 is the current boot. This is essential for building a timeline across reboots.

```bash
journalctl -b -2
journalctl -b -1
```

Shows the full journal for a specific boot (-b).

---

### Case Study - Investigating Two Reboots

Using `journalctl -b -2 | grep -Ei "reboot|shutdown|poweroff"` and the same for -b -1, both boots showed a clean, systemd-managed sequence:

```text
systemd-logind: The system will reboot now!
...
Reached target shutdown.target
Finished systemd-reboot.service
Reached target reboot.target
Syncing filesystems and block devices
Sending SIGTERM to remaining processes
```

What this proves: Both reboots were graceful - no kernel panic, no crash, no unexpected forced shutdown. If a crash had occurred, you would instead expect to see things like a kernel panic, "Oops," a watchdog trigger, or the system disappearing from the log with no clean shutdown sequence.

What this does NOT prove: The logs show systemd-logind handling the reboot request, but not who or what triggered it. A cron "@reboot jobs" entry appeared, but that runs after boot - it is not the cause of the reboot.

The key discipline: separate fact from assumption.

- FACT: Kali performed two consecutive graceful reboots.
- NOT PROVEN: What caused the reboots, or who/what requested them.

This is the structure of a real SOC investigation:

```text
1. Detect event
2. Find relevant logs
3. Identify timeline
4. Inspect previous boot
5. Inspect shutdown sequence
6. Determine what is proven
7. Identify what is still unknown
```

---

## Key Takeaways

- /var/log/ is where traditional Linux log files live - but not every system uses the same files.
- auth.log covers authentication events where present; on systems without it, use journalctl instead.
- journalctl -n, -p err, --list-boots, and -b are core tools for modern systemd-based logging.
- grep turns a log file into a targeted search for suspicious patterns.
- An error is an event, not automatically a security incident.
- A SOC analyst works from evidence, not assumption - always ask: Who? What? When? Where? Why? Is it normal? What evidence proves it?
