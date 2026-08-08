# Lesson 04 - Linux Permissions & Ownership

## Objective

Understand how Linux controls access to files and directories: who owns a file, who can read it, who can modify it, and who can execute it.

---

## Commands Learned

### ls -l

Shows detailed information about each file.

Example:

```text
-rw-rw-r-- 1 hassan hassan 57 Aug 3 07:04 hassan.txt
```

Output format:

```text
Permissions -> Links -> Owner -> Group -> Size -> Date/Time -> Name
```

---

### File vs Directory

- `-` -> Regular file
- `d` -> Directory

Example:

- `-rw-rw-r--` -> file
- `drwxrwxr-x` -> directory

---

### r, w, x

| Symbol | Meaning | Description |
|---|---|---|
| r | Read | View file contents |
| w | Write | Modify or write to the file |
| x | Execute | Run the file as a program/script |

---

### Owner, Group & Others

Permissions are divided into three categories:

- Owner - the user who owns the file
- Group - the group the file belongs to
- Others - everyone else

Example breakdown of `-rw-r--r--`:

```text
- | rw- | r-- | r--
    Owner  Group  Others
```

---

### Numeric Permissions

| Permission | Value |
|---|---|
| r | 4 |
| w | 2 |
| x | 1 |

Values are added together:

- `rw-` = 4 + 2 + 0 = 6
- `r-x` = 4 + 0 + 1 = 5
- `r--` = 4 + 0 + 0 = 4

---

### chmod

Used to modify permissions.

Example:

```bash
chmod 755 script.sh
```

Result: `rwxr-xr-x`

```text
755
7 | 5 | 5
Owner Group Others
```

---

### chown

Used to change file ownership.

```bash
chown user file
```

Key difference:

- `chmod` -> changes permissions
- `chown` -> changes ownership

---

### Least Privilege

A user or process should only have the permissions it actually needs.

Example - a sensitive file:

```text
-rw-------
```

Only the owner can read and write; nobody else has access.

---

## Summary

This lesson covered `ls -l` output, file vs. directory, the read/write/execute model, the Owner/Group/Others structure, numeric permission values, and the `chmod` and `chown` commands. Understanding permissions is essential for enforcing access control and the principle of Least Privilege in real systems.
