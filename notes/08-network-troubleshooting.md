# Lesson 08 - Network Troubleshooting & Connectivity

## Objective

When a network problem occurs, the goal is not to randomly try commands - it's to follow a troubleshooting methodology that identifies exactly where the connectivity chain breaks.

---

## Commands Learned

### The Troubleshooting Chain

```text
Your Machine
    |
Network Interface
    |
IP Configuration
    |
Gateway
    |
DNS
    |
Internet / Destination
```

When something fails, work through this chain step by step to find where it breaks.

Example 1:

```text
127.0.0.1       OK
192.168.18.181  OK
192.168.18.1    FAIL
```

This points to a problem with the local network, interface, or gateway path.

Example 2:

```text
Gateway         OK
8.8.8.8         OK
google.com      FAIL
```

This points to a DNS problem, not a connectivity problem.

---

### ip addr - Check the Interface

```bash
ip addr
```

This confirms:

- Is the interface UP?
- Does it have an IP address?
- Is the IP correct?
- Which interface is the right one?

---

### ip route - Check the Gateway

```bash
ip route
```

Expected output includes something like:

```text
default via 192.168.18.1 dev eth0
```

This tells us:

- Gateway = 192.168.18.1
- Interface = eth0

If the default route is missing, the machine will struggle to reach networks outside its own subnet.

---

### ping - Testing Step by Step

Start with localhost:

```bash
ping -c 4 127.0.0.1
```

Then the gateway:

```bash
ping -c 4 192.168.18.1
```

Then an external destination:

```bash
ping -c 4 8.8.8.8
```

This separates the problem into: Local -> Gateway -> Internet.

---

### IP Works but the Domain Doesn't?

This is an important SOC/network troubleshooting scenario.

If:

```bash
ping -c 4 8.8.8.8
```

works, but:

```bash
ping -c 4 google.com
```

fails - general network connectivity is likely fine, and the problem is in DNS resolution.

Check with:

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

---

### The Methodology to Remember

Never jump around randomly. Follow the chain:

```text
1. Interface
      |
2. IP
      |
3. Route/Gateway
      |
4. Connectivity
      |
5. DNS
      |
6. Destination
```

---

### Separating IP Connectivity from DNS Resolution

- IP connectivity: `ping 8.8.8.8` - tests using a raw IP address directly.
- DNS resolution: `nslookup google.com` - tests whether a domain name correctly resolves to an IP.

This distinction matters:

```text
ping 8.8.8.8        OK
nslookup google.com FAIL
   -> investigate DNS specifically

ping 8.8.8.8        FAIL
   -> the problem is not DNS alone;
      go back to interface -> route -> gateway -> internet path
```

---

## Summary

This lesson introduced a structured network troubleshooting methodology: interface, IP, route/gateway, local connectivity, internet connectivity, and DNS resolution - checked in that order. The key skill is separating an IP connectivity problem from a DNS resolution problem, since they point to very different root causes and require different fixes.
