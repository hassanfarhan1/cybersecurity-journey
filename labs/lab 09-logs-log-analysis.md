# Lab 09 - Linux Logs & Log Analysis

## Objective

Explore Linux log storage and search for authentication-related events.

---

## Environment

- Operating System: Kali Linux
- User: hassan

---

## Commands Executed

```bash
ls -lah /var/log/
ls -l /var/log/auth.log
tail -n 20 /var/log/auth.log
grep "Failed password" /var/log/auth.log
```

### Follow-up Investigation - Boot Timeline

```bash
journalctl -n 20
journalctl -p err
journalctl --list-boots
journalctl -b -2 | grep -Ei "reboot|shutdown|poweroff"
journalctl -b -1 | grep -Ei "reboot|shutdown|poweroff"
```

---

## Findings

- `auth.log` does not exist on this Kali system - logging is handled through the systemd journal instead.
- `journalctl --list-boots` identified two prior boots (-2, -1) plus the current boot (0), enabling a timeline to be built.
- Both prior reboots showed a clean, systemd-managed shutdown/reboot sequence with no evidence of a kernel crash or forced shutdown.
- The specific trigger for each reboot (who or what requested it) could not be confirmed from the available logs.

---

## Skills Gained

- Locating and browsing log files under `/var/log/`
- Reading logs with `cat`, `less`, and `tail`
- Searching logs for specific events with `grep`
- Using `journalctl -n`, `-p err`, `--list-boots`, and `-b` to investigate a systemd journal
- Building a timeline across reboots from log evidence
- Separating confirmed facts from unproven assumptions in an investigation

---

## Conclusion

This lab demonstrated a small-scale SOC-style log investigation: identifying available logs, building a timeline, and clearly separating what the evidence proves from what remains unknown. This evidence-based mindset - rather than jumping from "error" to "attack" - is central to real log analysis work.
