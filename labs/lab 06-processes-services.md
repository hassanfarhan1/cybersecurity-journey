# Lab 06 - Linux Processes & Services

## Objective

Observe currently running processes and the status of key services, without making any changes yet.

---

## Environment

- Operating System: Kali Linux
- User: hassan

---

## Commands Executed

### Part 1 - Process Observation

```bash
whoami
ps
ps aux
top
```

Ran `top`, observed live process activity for about 10-15 seconds, then pressed `q` to exit.

### Part 2 - Service Observation

```bash
systemctl status ssh
systemctl is-active ssh
systemctl is-enabled ssh
systemctl status cron
```

---

## Rules Followed

- Did not run `kill`, `systemctl stop`, or `systemctl restart`.
- This stage focused on observation only - no processes or services were modified.

---

## Questions Reflected On

- What is a PID?
- What is the difference between `ps` and `ps aux`?
- What does `top` show that `ps aux` does not?
- What is the difference between a process and a service?
- What is the difference between `is-active` and `is-enabled`?
- Why does a Cybersecurity analyst need to know what processes and services are running on a machine?

---

## Skills Gained

- Listing and interpreting running processes with `ps` and `ps aux`
- Monitoring processes live with `top`
- Checking service status with `systemctl status`, `is-active`, and `is-enabled`
- Understanding the difference between a process and a service

---

## Conclusion

This lab focused on safely observing running processes and service states - a foundational skill for incident investigation, where identifying what is currently running on a system (and whether it should be) is often the first step.
