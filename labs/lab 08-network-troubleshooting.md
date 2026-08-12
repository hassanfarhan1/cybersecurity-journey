# Lab 08 - Network Troubleshooting & Connectivity

## Objective

Run through the full troubleshooting chain on this machine and interpret each result.

---

## Environment

- Operating System: Kali Linux
- User: hassan

---

## Commands Executed

```bash
ip addr
ip route
ping -c 4 127.0.0.1
ping -c 4 192.168.18.1
ping -c 4 8.8.8.8
nslookup google.com
```

---

## Output and Analysis

### ip addr

```text
1: lo: <LOOPBACK,UP,LOWER_UP>
    inet 127.0.0.1/8 scope host lo

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    link/ether 00:0c:29:1b:9f:22
    inet 192.168.18.181/24 brd 192.168.18.255 scope global eth0
```

Result: `eth0` is UP, IP is `192.168.18.181/24`. Interface is healthy.

### ip route

```text
default via 192.168.18.1 dev eth0 proto dhcp src 192.168.18.181 metric 100
192.168.18.0/24 dev eth0 proto kernel scope link src 192.168.18.181 metric 100
```

Result: Gateway = `192.168.18.1`, Interface = `eth0`. Routing configuration is correct.

### ping -c 4 127.0.0.1

```text
4 packets transmitted, 4 received, 0% packet loss
rtt avg = 0.098 ms
```

Result: Local TCP/IP stack is working.

Note: `ping -c 127.0.0.1` without a number failed with "invalid argument" - `-c` requires a count, e.g. `-c 4`. Corrected on the next attempt.

### ping -c 4 8.8.8.8

```text
4 packets transmitted, 4 received, 0% packet loss
rtt avg = 21.849 ms
```

Result: The machine can reach the internet by IP (Kali -> eth0 -> Gateway -> Internet -> 8.8.8.8).

### nslookup google.com

```text
Server:  192.168.18.1
Address: 192.168.18.1#53

Non-authoritative answer:
Name: google.com
Address: 142.251.27.138
(and other IPs)
```

Result: DNS resolution is working - `google.com` correctly resolves to IP addresses.

---

## Full Chain Result

```text
1. Interface             -> eth0 UP              OK
2. IP                    -> 192.168.18.181/24    OK
3. Route                 -> Gateway 192.168.18.1  OK
4. Local connectivity    -> 127.0.0.1            OK
5. Internet connectivity -> 8.8.8.8              OK
6. DNS resolution        -> google.com -> IPs    OK
```

Conclusion: The machine's network is fully functional end to end.

---

## Scenario Analysis

**Scenario A:**

```text
ip addr        -> eth0 UP    OK
ip route       -> gateway exists  OK
ping 127.0.0.1 -> OK
ping 8.8.8.8    -> FAIL
nslookup google.com -> FAIL
```

Since `127.0.0.1` works, the local TCP/IP stack is fine. Because `8.8.8.8` fails, the problem is somewhere between the local network and the internet - check `ip route`, the gateway, and connectivity to the gateway (`ping` the gateway) before assuming it's DNS.

**Scenario B:**

```text
ping 8.8.8.8         -> OK
nslookup google.com  -> FAIL
```

IP connectivity is confirmed working, so the network path itself is fine. Since only domain resolution fails, the problem is specifically DNS - check the DNS server configuration, e.g.:

```bash
cat /etc/resolv.conf
```

---

## Skills Gained

- Applying a step-by-step network troubleshooting chain instead of guessing
- Verifying interface and IP configuration with `ip addr`
- Verifying gateway and routing with `ip route`
- Testing local, gateway, and internet connectivity with `ping`
- Testing DNS resolution with `nslookup`
- Distinguishing an IP connectivity problem from a DNS resolution problem

---

## Conclusion

This lab confirmed that the machine's network is fully functional end to end: interface, IP, route, local connectivity, internet connectivity, and DNS all passed. More importantly, it reinforced the core SOC/network analyst mindset: never say "the internet isn't working" - instead, ask "where exactly does the connectivity chain break?"
