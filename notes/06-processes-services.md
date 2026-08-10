# Lesson 06 - Linux Processes & Services

## Objective

The previous lesson covered users, groups, and sudo. This lesson covers what is actually running on a Linux system - processes and services - and the commands used to observe and manage them.

---

## Part 1 - Processes

### What is a Process?

A process is a program or command currently running on the system.

Examples: Firefox, a terminal, python, ssh, bash, nano. Linux manages each of these as a process.

Every process has a PID (Process ID) - a number Linux uses to identify it.

Example:

```text
Firefox  -> PID 2314
bash     -> PID 1452
python   -> PID 3201
```

Why this matters for Cybersecurity: A security analyst needs to know what is currently running on a machine. If an unknown process appears, it should be investigated: What is it? Who started it? When did it start? What is it using? Is it connected to the network? Could it be malicious? Process monitoring is a core part of system security.

---

### PID and PPID

- PID = Process ID - a unique ID assigned to every process.
- PPID = Parent Process ID - the ID of the process that started another process.

Example:

```text
zsh (48619)
   |
sleep (65047)

zsh    = parent
sleep  = child
```

---

### ps

```bash
ps
```

Shows currently running processes.

Example output:

```text
PID   TTY      TIME     CMD
1234  pts/0    00:00:00 zsh
1250  pts/0    00:00:00 ps
```

- PID - process ID
- TTY - the terminal the process is attached to
- TIME - CPU time used
- CMD - the command/program

---

### ps aux

`ps` alone shows limited information. For a fuller picture:

```bash
ps aux
```

Example output:

```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1    ...
hassan    1234  0.1  1.2    ...
hassan    1456  0.0  0.5    ...
```

Key columns for Cybersecurity:

- USER - who owns the process
- PID - the process ID
- %CPU - CPU usage
- %MEM - memory usage
- COMMAND - the running program/command

---

### top

```bash
top
```

Provides real-time process monitoring.

```text
ps aux -> snapshot (shows data once)
top     -> live monitoring (continuously updates)
```

`top` shows if a process is using high CPU, using high memory, is newly started, or has ended. Press `q` to exit `top`.

---

### pstree -p

```bash
pstree -p
```

Displays the parent/child process tree, showing which processes started which.

---

### Background Processes

```bash
sleep 1000 &
```

The `&` runs the command in the background, so the terminal remains free to use.

```bash
echo $!
```

Returns the PID of the last background process started.

```bash
echo $?
```

Returns the exit status of the last executed command.

---

### kill

```bash
kill 1234
```

Sends a signal to the process with PID 1234. By default, `kill` sends SIGTERM, which gives the process a chance to shut down cleanly - it does not mean "force kill."

```bash
kill -9 1234
```

`-9` = SIGKILL - this forcefully and immediately terminates the process.

Rule: Do not use `kill -9` by default. Try `kill PID` first; only use `kill -9 PID` if the process refuses to stop.

To verify a process has stopped:

```bash
ps -p 65047
```

---

## Part 2 - Services

### What is a Service?

A service is a program or system component designed to run in the background and provide an ongoing function.

Examples: SSH (remote access), a web server, a database, cron (scheduled tasks).

```text
SSH Service
     |
  Network
     |
Remote login
```

A Cybersecurity analyst needs to know which services are running on a machine, because every open service is part of the attack surface.

---

### Process vs Service

- Process - a program currently running (e.g. sleep, PID 65047).
- Service - a background system component that is managed to keep running (e.g. ssh.service).

```text
SERVICE
   |
manages/runs
   |
PROCESS
```

A service can involve one or more processes/child processes - the two concepts are related but not identical.

---

### systemctl

Most Linux distributions use systemd, managed with the `systemctl` command.

```bash
systemctl status ssh
```

Shows whether SSH is running, stopped, or failed, along with its main PID and related log information.

Possible states:

- `Active: active (running)` -> the service is running
- `Active: inactive (dead)` -> the service is not running
- `Active: failed` -> the service tried to start but failed

---

### systemctl start / stop / restart

```bash
sudo systemctl start ssh
```

Starts the SSH service now.

```bash
sudo systemctl stop ssh
```

Stops the SSH service now.

```bash
sudo systemctl restart ssh
```

Performs a stop followed by a start - commonly used after a configuration change.

---

### enable vs start

This distinction is important:

- `systemctl start ssh` -> starts the service now
- `systemctl enable ssh` -> configures the service to start automatically on boot
- `systemctl disable ssh` -> removes the service from automatic startup (it does not stop it immediately - use `stop` for that)

---

### Cybersecurity Example

On a server with SSH, a web server, and a database all active, a security analyst asks: which services are actually necessary?

If a server needs a web server but not SSH, leaving SSH open unnecessarily increases the attack surface.

Security principle: Run only what is necessary - this is part of attack surface reduction.

---

## Summary

This lesson covered how to observe and manage what is running on a Linux system: processes (ps, ps aux, top, pstree, kill) and services (systemctl status/start/stop/restart/enable/disable). Together, these tools let a SOC analyst answer one of the most important questions in an investigation - what is actually running on this machine, and should it be?
