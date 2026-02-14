# Linux Networking Debugging Commands

A quick reference for diagnosing connectivity, ports, DNS, and routing issues.

---

## ip

Modern replacement for `ifconfig` and `route`.

### Show IP addresses
```bash
ip addr
```

### Show routing table
```bash
ip route
```

### Show network interfaces
```bash
ip link show
```

**When to use:** First command to run when debugging network issues.

---

## ping

Test connectivity to a host.

```bash
ping google.com
```

Limit to 4 packets:
```bash
ping -c 4 8.8.8.8
```

**When to use:** Check if a host is reachable and measure latency.

---

## ss

Inspect sockets (modern replacement for `netstat`).

### Show listening ports
```bash
ss -tulpen
```

### Show active connections
```bash
ss -tan
```

### Filter by port
```bash
ss -tan | grep 443
```

**When to use:** Verify if a service is listening or connections are established.

---

## netstat (legacy but still common)

```bash
netstat -tulnp
```

**When to use:** Older systems where `ss` is not available.

---

## curl

Test HTTP/HTTPS endpoints.

### Fetch a page
```bash
curl https://example.com
```

### Show headers only
```bash
curl -I https://example.com
```

### Show HTTP status code
```bash
curl -s -o /dev/null -w "%{http_code}\n" https://example.com
```

**When to use:** Debug APIs, web services, load balancers.

---

## traceroute

Show the path packets take to a host.

```bash
traceroute google.com
```

If not installed:
```bash
sudo apt install traceroute
```

**When to use:** Identify routing issues or network hops causing delay.

---

## mtr

Real-time traceroute + ping combined.

```bash
mtr google.com
```

**When to use:** Advanced latency and packet loss debugging.

---

## dig

DNS lookup tool.

### Query DNS record
```bash
dig google.com
```

### Query specific DNS server
```bash
dig @8.8.8.8 google.com
```

**When to use:** Debug DNS resolution problems.

---

## nslookup

Simpler DNS query tool.

```bash
nslookup google.com
```

**When to use:** Quick DNS checks.

---

## host

Another DNS lookup utility.

```bash
host google.com
```

**When to use:** Fast DNS resolution check.

---

## nc (netcat)

Test open ports and raw TCP/UDP connections.

### Check if port is open
```bash
nc -zv example.com 443
```

### Listen on a port
```bash
nc -l 8080
```

**When to use:** Port reachability testing.

---

## telnet

Basic TCP connectivity test.

```bash
telnet example.com 80
```

**When to use:** Quick manual TCP testing.

---

## tcpdump

Packet capture tool.

```bash
sudo tcpdump -i any port 443
```

Capture to file:
```bash
sudo tcpdump -i eth0 -w capture.pcap
```

**When to use:** Deep packet-level debugging.

---

## Quick Debug Checklist

1. Check interface & IP → `ip addr`
2. Check routing → `ip route`
3. Test connectivity → `ping`
4. Verify listening ports → `ss -tulpen`
5. Test service → `curl` / `nc`
6. Check DNS → `dig` / `nslookup`
7. Trace path → `traceroute` / `mtr`
8. Capture traffic → `tcpdump`

---

## Notes

- Prefer `ip` and `ss` over older tools (`ifconfig`, `netstat`)
- Root privileges may be required for packet capture
- DNS issues are often mistaken for connectivity issues
- Firewalls commonly block ICMP (ping), but services may still work

---

