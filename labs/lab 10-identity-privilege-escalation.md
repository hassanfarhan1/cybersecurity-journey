# Lab 10 - Linux Identity & Privilege Escalation

## Objective

Identify the current user's identity and group memberships, confirm sudo privileges, and investigate sudo activity in the logs.

---

## Environment

- Operating System: Kali Linux
- User: hassan

---

## Commands Executed

```bash
whoami
id
groups
cat /etc/passwd | grep hassan
sudo -l
journalctl | grep sudo
```

---

## Findings

- `whoami` / `id` confirmed user `hassan`, UID = 1000, GID = 1000.
- `groups` showed membership in several groups, including `sudo`, `adm`, and `wireshark`.
- `/etc/passwd` entry: `hassan:x:1000:1000:Hassan ,,,:/home/hassan:/usr/bin/zsh`
- `sudo -l` returned `(ALL : ALL) ALL` - hassan can run any command via sudo with root privileges.
- Log search found:

```text
sudo[319113]: pam_unix(sudo:session): session opened for user root(uid=0) by hassan(uid=1000)
...
session closed for user root
```

This confirms a sudo session was opened and closed by hassan, escalating to root (UID 0). The log confirms the session occurred but does not show the specific command that was executed during it.

---

## Skills Gained

- Identifying user identity with `whoami`, `id`, and `groups`
- Reading a user's account entry in `/etc/passwd`
- Checking sudo permissions with `sudo -l`
- Recognizing a sudo session in logs and understanding what it does and does not prove
- Applying the SOC principle of evidence over assumption when reviewing privilege escalation activity

---

## Conclusion

This lab connected user identity (UID/GID, groups) to privilege escalation via sudo, and demonstrated how to verify sudo activity using log evidence rather than assumption. A confirmed sudo session is not by itself evidence of malicious activity - it must be evaluated against who, when, how, and what command was run.
