# Lab 04 - Linux Permissions & Ownership

## Objective

Practice reading and modifying Linux file permissions using `ls -l`, `chmod`, and numeric permission values.

---

## Environment

- Operating System: Kali Linux
- User: hassan

---

## Commands Executed

### Step 1 - Create Lab Directory

```bash
cd ~/Desktop
mkdir permissionslab
cd permissionslab
```

### Step 2 - Create Files

```bash
touch public.txt private.txt script.sh
```

### Step 3 - Check Permissions

```bash
ls -l
```

### Step 4 - Test 755

```bash
chmod 755 script.sh
ls -l script.sh
```

Expected: `-rwxr-xr-x`

### Step 5 - Test 754

```bash
chmod 754 script.sh
ls -l script.sh
```

Expected: `-rwxr-xr--`

### Step 6 - Test 640

```bash
chmod 640 private.txt
ls -l private.txt
```

Expected: `-rw-r-----`

### Step 7 - Final Challenge

Goal: `secret.txt` should have sensitive data protection:

- Owner -> rw-
- Group -> ---
- Others -> ---

Calculation:

```text
rw- = 6
--- = 0
--- = 0
```

Command used:

```bash
chmod 600 secret.txt
```

Expected: `-rw-------`

---

## Expected Output

Permissions verified during this lab:

- `755` -> `rwxr-xr-x`
- `754` -> `rwxr-xr--`
- `640` -> `rw-r-----`
- `600` -> `rw-------`

---

## Skills Gained

- Reading and interpreting `ls -l` output
- Understanding Owner / Group / Others
- Converting permissions to numeric values
- Changing permissions with `chmod`
- Understanding the difference between `chmod` and `chown`
- Applying the principle of Least Privilege

---

## Conclusion

This lab demonstrated how to inspect and modify Linux file permissions in practice. Correctly applying permissions like `600` for sensitive files is a core part of access control and system hardening in a SOC/Blue Team role.
