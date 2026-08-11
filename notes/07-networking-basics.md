# Lesson 07 - Linux Networking Basics

## Objective

Before doing any network reconnaissance or scanning, a Cybersecurity analyst must first understand their own machine's network identity: what interfaces it has, what IP address it uses, how it reaches other networks, and what services are listening for connections.

---

## Commands Learned

### Network Interfaces: lo and eth0

A Linux machine typically has at least two important interfaces:

- `lo` - the loopback interface
- `eth0` - the main network interface

---

### lo - Loopback

```text
1: lo: <LOOPBACK,UP,LOWER_UP>
inet 127.0.0.1/8
```

`lo` is the loopback interface. The IP `127.0.0.1` always refers to the machine itself - also known as `localhost`.

Example: if a web server is running on Kali on port 8000, it can be reached at:

```text
127.0.0.1:8000
```

---

### eth0 - Main Network Interface

```text
2: eth0
state UP
```

`eth0` is Kali's network interface. `state UP` means the interface is active and working.

---

### MAC Address

```text
link/ether 00:0c:29:1b:9f:22
```

This is the MAC address of `eth0` - the hardware/link-layer address.

```text
IP address  -> logical/network address
MAC address -> hardware/link-layer address
```

These are two different things and should not be confused.

---

### IP Address and CIDR (/24)

```text
inet 192.168.18.181/24
```

Kali's IP address is `192.168.18.181`. The `/24` is CIDR notation, describing the subnet:

```text
Network: 192.168.18.0/24
Kali:    192.168.18.181
```

`/24` corresponds to subnet mask `255.255.255.0`. In this subnet, the usable host range is generally:

```text
192.168.18.1  ->  192.168.18.254
```

`192.168.18.0` is the network address, and `192.168.18.255` is the broadcast address.

---

### Broadcast Address

```text
brd 192.168.18.255
```

The broadcast address is used to send a message to every device on the subnet at once.

```text
Network       192.168.18.0/24
Kali IP       192.168.18.181
Broadcast     192.168.18.255
```

---

### ip route - Default Gateway

```text
default via 192.168.18.1 dev eth0
```

`192.168.18.1` is the default gateway. If Kali needs to reach a network outside its own subnet, traffic is sent to the gateway first.

```text
Kali
192.168.18.181
      |
Gateway
192.168.18.1
      |
Other networks / Internet
```

---

### hostname

```bash
hostname
```

Output: `kali` - this is the machine's name.

Summary of this machine's networking identity:

| Item | Value |
|---|---|
| Hostname | kali |
| Interface | eth0 |
| Kali IP | 192.168.18.181 |
| MAC | 00:0c:29:1b:9f:22 |
| Gateway | 192.168.18.1 |
| Network | 192.168.18.0/24 |
| Loopback | 127.0.0.1 |

Cybersecurity relevance: Before running any scan or reconnaissance, an analyst must know "what network am I on?" Without knowing your own IP, interface, and gateway, you can easily misinterpret the results of a scan.

```text
Kali
192.168.18.181/24
       |
Network
192.168.18.0/24
       |
Potential hosts
192.168.18.x
```

---

### ss -tuln - Listening Ports

```bash
ss -tuln
```

This shows TCP/UDP listening sockets - ports where a service is waiting for incoming connections.

Example output columns:

```text
Netid    State    Recv-Q    Send-Q    Local Address:Port    Peer Address:Port
```

- Netid - TCP or UDP
- State - socket state, e.g. LISTEN
- Recv-Q - data waiting to be read
- Send-Q - data waiting to be sent
- Local Address:Port - this machine and the port
- Peer Address:Port - the other side of the connection

Example - if SSH were running:

```text
tcp   LISTEN   0   128   0.0.0.0:22   0.0.0.0:*
```

This means:

- tcp - TCP is being used
- LISTEN - a service is waiting for incoming connections
- 22 - the port is 22, normally used by SSH
- 0.0.0.0 - the service is listening on all IPv4 interfaces, not just 127.0.0.1

The correct way to describe this: "One TCP listening socket is on port 22, which may indicate an SSH service." (LISTEN describes a socket waiting for connections - not an already-established connection.)

```text
IP
 |
192.168.18.181
 |
Port
 |
22
 |
SSH service
 |
Potential attack surface
```

Cybersecurity connection:

```text
Machine
   |
Listening ports
   |
Running services
   |
Potential attack surface
```

Seeing an unexpected open port does not automatically mean it's a vulnerability - it means the service behind that port needs to be investigated.

---

### Four Key Reconnaissance Commands

When starting a basic assessment of your own machine - IP, gateway, DNS server, and listening ports - these four commands cover it:

| Command | What it tells you |
|---|---|
| `ip addr` | Network interfaces, IP addresses, MAC address, interface status - "Who am I on the network?" |
| `ip route` | Default gateway and routing information - "Where do I send traffic?" |
| `nslookup` | DNS resolution and which DNS server is used - "How does a domain resolve?" |
| `ss -tuln` | TCP/UDP listening sockets and open ports - "What ports/services are listening?" |

Example:

```bash
nslookup google.com
```

---

## Summary

This lesson covered how to identify your own machine's network identity: interfaces (lo, eth0), IP address and CIDR notation, MAC address, broadcast address, default gateway, hostname, and listening ports (ss -tuln). Together, ip addr, ip route, nslookup, and ss -tuln form the foundation for any network reconnaissance - you must know your own network before investigating anyone else's.
