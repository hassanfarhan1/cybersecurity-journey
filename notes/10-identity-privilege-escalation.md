# Lesson 10 - Linux Identity & Privilege Escalation

## 1. Identity - whoami, id, groups

```bash
whoami
id
groups
```

Result: user `hassan`, UID = 1000, GID = 1000.

- UID identifies the user.
- GID identifies the primary group.

## 2. Groups

Hassan belongs to several groups; the security-relevant ones are `sudo`, `adm`, and `wireshark`. Being in the `sudo` group is what allows elevated privileges.

## 3. /etc/passwd

```text
hassan:x:1000:1000:Hassan ,,,:/home/hassan:/usr/bin/zsh
```

Fields: username, UID, GID, home directory, login shell.

## 4. sudo -l

```bash
sudo -l
```

Result: `(ALL : ALL) ALL` - Hassan is permitted to run any command via sudo with root privileges.

Key distinction: `hassan != root`. Hassan is UID 1000; when sudo is used, the command runs as UID 0 (root).

## 5. Log Evidence

```text
sudo[319113]: pam_unix(sudo:session): session opened for user root(uid=0) by hassan(uid=1000)
...
session closed for user root
```

This shows: hassan (UID 1000) -> sudo -> root (UID 0) -> session opened -> session closed. This is not evidence of an attack - it's normal sudo activity. But note the limitation: this log confirms a sudo session, not the specific command that was executed. "sudo session detected" != "specific command identified".

## 6. Privilege Escalation

```text
Normal user -> sudo / privilege mechanism -> root
```

Privilege escalation becomes a security incident only if a user/process gains privilege it isn't authorized for, or misuses it. When an alert says "user obtained root privileges," don't assume attack - ask: Who? When? How? What command? Was it authorized? What happened afterward?

## 7. Three Rules to Remember

1. Error != Attack
2. No log != No activity
3. Evidence > Assumption

---

## Summary

This lesson connected Linux identity (UID/GID, groups, /etc/passwd) to privilege escalation via sudo, and showed how to read sudo session logs. The core SOC skill: confirm what the evidence actually proves - a sudo session - versus what it does not prove, such as the exact command run or intent, before calling anything an incident.
