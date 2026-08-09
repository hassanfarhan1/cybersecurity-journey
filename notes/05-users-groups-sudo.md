# Lesson 05 - Linux Users, Groups & sudo

## Objective

The previous lesson covered permissions: read, write, and execute. This lesson covers who those permissions belong to - Linux users, groups, and the elevated-privilege system (`sudo`) that controls system administration.

Topics covered:

1. Users
2. Groups
3. whoami
4. id
5. /etc/passwd
6. /etc/group
7. Root user
8. sudo
9. Security relevance
10. Numeric permissions review (chmod)

---

## Commands Learned

### What is a User?

Linux is a multi-user operating system, meaning the system can be used by different users. Each user has:

- a username
- a User ID (UID)
- a group
- permissions
- a home directory

This is why the terminal prompt shows something like:

```text
hassan@kali
```

Here, `hassan` is the username of the currently active user.

---

### whoami

To check which user you are currently working as:

```bash
whoami
```

Since the username is `hassan`, the expected output is:

```text
hassan
```

---

### Users, Groups, and Privileges - Overview

These concepts connect together to form the Linux access control model:

```text
User      -> UID
Group     -> GID
Owner / Group / Others -> Permissions
sudo      -> Elevated privileges
/etc/passwd -> User account information
/etc/group  -> Group information
```

- UID (User ID): A unique number identifying each user on the system.
- GID (Group ID): A unique number identifying each group on the system.
- /etc/passwd: The system file that stores information about user accounts.
- /etc/group: The system file that stores information about groups.
- Root user: The Linux administrator account with full privileges over the system.
- sudo: Allows a permitted user to run a command with elevated (root) privileges, without permanently switching to the root account.

Security relevance: Because sudo grants elevated privileges, controlling who has sudo access is one of the most important parts of Linux system security - an attacker or misconfigured account with unnecessary sudo access can compromise the entire system.

---

### chmod - Numeric Permissions Review

| Value | Permission |
|---|---|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 0 | --- |

Example reviewed this lesson:

Goal:

```text
Owner  -> full permission = rwx
Group  -> read only       = r--
Others -> no permission   = ---
```

Calculation:

```text
rwx = 7
r-- = 4
--- = 0
```

Command:

```bash
chmod 740 secret.txt
```

---

## Summary

This lesson introduced the identity side of Linux access control: users, groups, UIDs, GIDs, and the system files that store this information (/etc/passwd, /etc/group). It also introduced sudo and the root user, and reinforced numeric chmod values from the previous lesson. Together, permissions (what can be done) and users/groups (who can do it) form the foundation of Linux access control.
