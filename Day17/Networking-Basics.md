# 🌐 Day 17 — Linux Networking Basics

Networking is an important Linux skill for **Java developers, backend developers, DevOps engineers, system administrators, and cloud engineers**.

Today you will learn how Linux communicates with other systems and how to inspect basic network configuration from the terminal.

---

## 🎯 Today's Goals

By the end of Day 17, you will understand:

- What a network is
- IP addresses
- IPv4 and IPv6
- Public vs private IP
- MAC addresses
- `localhost`
- Network interfaces
- Ports
- TCP and UDP
- DNS
- Default gateway
- Routing
- `ip` command
- `ping`
- `hostname`
- Basic network troubleshooting
- Java application networking examples

---

# 1. What Is a Network?

A network allows computers and devices to communicate with each other.

Example:

```text
Your Computer
      ↓
   Router
      ↓
   Internet
      ↓
   Server
```

A Java application may communicate with:

```text
Java Application
      ↓
   Network
      ↓
Database Server
```

For example:

```text
Java Application → PostgreSQL
Java Application → REST API
Browser → Spring Boot Application
Developer PC → Linux Server
```

---

# 2. What Is an IP Address?

An **IP address** identifies a device or network interface on an IP network.

Example IPv4 address:

```text
192.168.1.10
```

IPv4 contains four numeric parts.

Example:

```text
192 . 168 . 1 . 10
```

Each part can range from:

```text
0 to 255
```

---

# 3. IPv4 vs IPv6

### IPv4

Example:

```text
192.168.1.10
```

IPv4 uses 32 bits.

### IPv6

Example:

```text
2001:db8::1
```

IPv6 uses 128 bits.

IPv6 provides a much larger address space than IPv4.

---

# 4. Private IP Addresses

Private IP addresses are commonly used inside local networks.

Common private IPv4 ranges include:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

Example:

```text
192.168.1.20
```

This could be the address of a device inside a home or office network.

---

# 5. Public IP Address

A public IP address can be used for communication across the public Internet.

Your local device may have:

```text
Private IP
    ↓
Router
    ↓
Public IP
    ↓
Internet
```

Do not confuse your computer's private IP with your public Internet address.

---

# 6. Localhost

`localhost` refers to the local machine.

The common IPv4 localhost address is:

```text
127.0.0.1
```

IPv6 localhost is:

```text
::1
```

Example:

```bash
ping localhost
```

You can also run:

```bash
ping 127.0.0.1
```

---

# 7. Why Is Localhost Important for Java?

Suppose you run a Spring Boot application locally:

```text
Spring Boot
    ↓
127.0.0.1
    ↓
Port 8080
```

You may access:

```text
http://localhost:8080
```

This means:

```text
localhost → your own computer
8080      → application port
```

---

# 8. Check Your Hostname

Run:

```bash
hostname
```

Example output:

```text
ubuntu
```

You can also use:

```bash
hostnamectl
```

On systems where `hostnamectl` is available, it provides additional system and hostname information.

---

# 9. Check Your IP Address

The modern Linux command is:

```bash
ip addr
```

Short form:

```bash
ip a
```

Example:

```text
2: eth0:
    inet 192.168.1.20/24
```

The important part is:

```text
192.168.1.20
```

---

# 10. Understanding `ip addr` Output

Run:

```bash
ip addr
```

You may see interfaces such as:

```text
lo
eth0
wlan0
```

### `lo`

Loopback interface.

Usually associated with:

```text
127.0.0.1
```

### `eth0`

Common name for an Ethernet interface.

### `wlan0`

Common naming convention for a wireless interface on some Linux systems.

Modern distributions may use different predictable interface names.

---

# 11. Check Only One Interface

Example:

```bash
ip addr show eth0
```

For loopback:

```bash
ip addr show lo
```

If your interface has a different name, first run:

```bash
ip addr
```

and use the actual interface name.

---

# 12. Check Network Links

Run:

```bash
ip link
```

or:

```bash
ip link show
```

This shows network interfaces and their state.

You may see:

```text
UP
DOWN
```

---

# 13. MAC Address

A MAC address identifies a network interface at the link layer.

Example:

```text
52:54:00:12:34:56
```

Find it with:

```bash
ip link
```

Look for:

```text
link/ether
```

Example:

```text
link/ether 52:54:00:12:34:56
```

---

# 14. IP Address vs MAC Address

| IP Address | MAC Address |
|---|---|
| Used at network layer | Used at link layer |
| Example: `192.168.1.10` | Example: `52:54:00:12:34:56` |
| Can change | Usually associated with network interface |
| Used for IP communication | Used for local network frame delivery |

For basic troubleshooting, remember:

```text
MAC → network interface identity
IP  → IP network addressing
```

---

# 15. Network Interfaces

A computer can have multiple network interfaces.

Example:

```text
Computer
 ├── lo
 ├── Ethernet
 └── Wi-Fi
```

A Linux server may have:

```text
eth0
eth1
```

A virtual machine may also have virtual interfaces.

WSL can have its own networking environment and interface names may differ from a physical Linux machine.

---

# 16. Ping

`ping` checks whether a destination responds to ICMP echo requests.

Example:

```bash
ping google.com
```

Stop it with:

```text
Ctrl + C
```

You can limit the number of packets:

```bash
ping -c 4 google.com
```

This sends four requests.

---

# 17. Ping Localhost

Run:

```bash
ping -c 4 127.0.0.1
```

or:

```bash
ping -c 4 localhost
```

If the local networking stack is functioning, you should receive replies.

---

# 18. What Does Ping Tell You?

Ping can help test basic reachability.

Example:

```text
Your Computer
     ↓
   ping
     ↓
Destination
```

However, a failed ping does **not always mean the destination is down**.

Firewalls or network policies can block ICMP.

So:

```text
ping failure ≠ guaranteed server failure
```

This is an important troubleshooting concept.

---

# 19. DNS

DNS stands for:

**Domain Name System**

DNS converts domain names into IP addresses.

Example:

```text
google.com
     ↓
DNS
     ↓
IP address
```

Humans prefer:

```text
example.com
```

Computers communicate using IP addresses.

---

# 20. Check DNS Resolution

Use:

```bash
getent hosts google.com
```

Example:

```text
IP_ADDRESS google.com
```

You can also use:

```bash
nslookup google.com
```

if `nslookup` is installed.

Another useful command:

```bash
dig google.com
```

if the `dig` utility is installed.

---

# 21. DNS Troubleshooting

Suppose:

```bash
ping 8.8.8.8
```

works, but:

```bash
ping google.com
```

does not.

One possible issue is DNS resolution.

This gives you a useful troubleshooting path:

```text
Can reach IP?
      ↓
YES
      ↓
Can resolve domain?
      ↓
NO
      ↓
Investigate DNS
```

This is only one possible explanation; network policies can produce similar symptoms.

---

# 22. Default Gateway

A **default gateway** is normally the router or next-hop device used to reach destinations outside the local network.

Check routing:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
```

Here:

```text
192.168.1.1
```

is the default gateway.

---

# 23. Routing

Routing determines where network traffic should go.

Run:

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0
```

Think of it as:

```text
Destination
     ↓
Routing table
     ↓
Network interface / gateway
     ↓
Destination
```

---

# 24. Check the Routing Table

Use:

```bash
ip route
```

For more detail:

```bash
ip route show
```

These are commonly equivalent ways to display the routing table.

---

# 25. Ports

An IP address identifies a network endpoint's host/interface context.

A **port** identifies a service endpoint on that host.

Example:

```text
192.168.1.20:8080
```

Here:

```text
IP   → 192.168.1.20
Port → 8080
```

---

# 26. Common Ports

| Port | Common Service |
|---:|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 8080 | Common application/server port |
| 8443 | Common HTTPS alternative |

These are common defaults, not requirements. Services can be configured to use different ports.

---

# 27. TCP

TCP is a connection-oriented transport protocol.

It provides mechanisms such as:

- Reliable delivery
- Ordered data
- Retransmission
- Connection management

Common examples:

```text
HTTP
HTTPS
SSH
PostgreSQL
```

often use TCP.

---

# 28. UDP

UDP is connectionless and has lower protocol overhead than TCP.

It does not provide TCP-style delivery guarantees.

Common examples include:

```text
DNS queries
Streaming
Real-time applications
Online games
```

The exact protocol used depends on the application and configuration.

---

# 29. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable delivery mechanisms | No TCP-style delivery guarantee |
| Ordered byte stream | Datagram-based |
| More protocol overhead | Lower overhead |
| Common for HTTP/HTTPS/SSH | Common for DNS and real-time traffic |

---

# 30. Check Listening Ports

A useful command is:

```bash
ss -tuln
```

Meaning:

```text
-t → TCP
-u → UDP
-l → listening
-n → numeric addresses/ports
```

You may see:

```text
LISTEN
0.0.0.0:8080
```

This means a service is listening on port `8080`.

---

# 31. Check Listening TCP Ports

```bash
ss -ltn
```

Check UDP:

```bash
ss -lun
```

Show process information when permitted:

```bash
sudo ss -tulpn
```

---

# 32. Java Application Example

Suppose your Spring Boot application runs on:

```text
localhost:8080
```

Check:

```bash
ss -ltn
```

You may find:

```text
LISTEN ... 127.0.0.1:8080
```

This indicates something is listening on port `8080`.

Then:

```text
Browser
   ↓
localhost:8080
   ↓
Spring Boot
```

---

# 33. PostgreSQL Example

PostgreSQL commonly uses:

```text
5432
```

A Java application might connect using:

```text
Host: localhost
Port: 5432
Database: campus
```

Conceptually:

```text
Java Application
       ↓
localhost:5432
       ↓
PostgreSQL
```

---

# 34. Test a Port with `nc`

If Netcat is installed:

```bash
nc -zv localhost 8080
```

This checks whether a TCP connection can be made to the specified port.

For PostgreSQL:

```bash
nc -zv localhost 5432
```

If `nc` is not installed, you can install it on Ubuntu with:

```bash
sudo apt install netcat-openbsd
```

---

# 35. Check Hostname and IP Together

Run:

```bash
hostname
```

Then:

```bash
hostname -I
```

`hostname -I` can show IP addresses associated with the system.

Output may look like:

```text
192.168.1.20
```

The exact output depends on your environment.

---

# 36. Useful Network Commands

### Show IP addresses

```bash
ip addr
```

### Show interfaces

```bash
ip link
```

### Show routes

```bash
ip route
```

### Show hostname

```bash
hostname
```

### Show IP addresses

```bash
hostname -I
```

### Test connectivity

```bash
ping -c 4 google.com
```

### Check DNS

```bash
getent hosts google.com
```

### Check listening ports

```bash
ss -tuln
```

---

# 37. Basic Network Troubleshooting

When a network connection fails, don't randomly run commands.

Follow a logical process.

### Step 1 — Check interface

```bash
ip link
```

### Step 2 — Check IP

```bash
ip addr
```

### Step 3 — Check route

```bash
ip route
```

### Step 4 — Test localhost

```bash
ping -c 4 127.0.0.1
```

### Step 5 — Test a known IP

```bash
ping -c 4 8.8.8.8
```

### Step 6 — Test DNS

```bash
getent hosts google.com
```

### Step 7 — Test domain reachability

```bash
ping -c 4 google.com
```

### Step 8 — Check ports

```bash
ss -tuln
```

This gives you a structured troubleshooting workflow.

---

# 38. Practice Lab 🧪

Create a Day 17 practice directory:

```bash
mkdir ~/day17-networking
cd ~/day17-networking
```

Create a report:

```bash
{
echo "===== HOSTNAME ====="
hostname

echo
echo "===== IP ADDRESSES ====="
hostname -I

echo
echo "===== INTERFACES ====="
ip link

echo
echo "===== IP CONFIGURATION ====="
ip addr

echo
echo "===== ROUTING ====="
ip route

echo
echo "===== LISTENING PORTS ====="
ss -tuln

} > network-report.txt
```

View it:

```bash
cat network-report.txt
```

---

# 39. Network Information Project 🛠️

Create a simple report containing:

```text
Hostname
IP address
Network interfaces
Routing table
Listening ports
DNS resolution
```

Run:

```bash
echo "===== DNS TEST =====" >> network-report.txt
getent hosts google.com >> network-report.txt
```

Then:

```bash
cat network-report.txt
```

You have now created a basic Linux network information report.

---

# 40. Mini Challenge 🔥

Try these without looking at the answers.

### Task 1

Show your IP configuration.

```text
?
```

### Task 2

Show your network interfaces.

```text
?
```

### Task 3

Show your routing table.

```text
?
```

### Task 4

Display your hostname.

```text
?
```

### Task 5

Display your IP addresses.

```text
?
```

### Task 6

Ping localhost four times.

```text
?
```

### Task 7

Resolve `google.com`.

```text
?
```

### Task 8

Show listening TCP and UDP ports.

```text
?
```

### Task 9

Check whether port 8080 is listening.

Hint:

```text
ss
```

---

# 41. Interview Questions 🎯

### Q1. What is an IP address?

An IP address is a network-layer address used to identify a host or network interface for IP communication.

### Q2. What is localhost?

`localhost` refers to the local machine.

Common addresses are:

```text
127.0.0.1
::1
```

### Q3. What is the purpose of `ip addr`?

It displays IP addresses and network interface information.

### Q4. What does `ip route` show?

It displays the system's routing table.

### Q5. What is DNS?

DNS translates domain names into IP addresses and supports other DNS lookups.

### Q6. What is a port?

A port identifies a service endpoint associated with a network address.

### Q7. What is the difference between TCP and UDP?

TCP provides connection-oriented, ordered and reliable delivery mechanisms. UDP is connectionless and does not provide TCP-style delivery guarantees.

### Q8. What does `ping` do?

It sends ICMP echo requests to test basic reachability when ICMP is permitted.

### Q9. What does `ss -tuln` do?

It displays listening TCP and UDP sockets using numeric addresses and ports.

### Q10. What is a default gateway?

It is the next-hop route normally used when traffic does not match a more specific route.

---

# 42. Day 17 Cheat Sheet

```text
NETWORK INFORMATION
cat /etc/os-release
hostname
hostname -I

IP
ip addr
ip a

INTERFACES
ip link

ROUTING
ip route

LOCALHOST
127.0.0.1
::1

CONNECTIVITY
ping -c 4 localhost
ping -c 4 google.com

DNS
getent hosts google.com
nslookup google.com
dig google.com

PORTS
ss -tuln
ss -ltn
ss -lun

TCP/UDP
TCP → connection-oriented
UDP → connectionless

COMMON PORTS
22   → SSH
53   → DNS
80   → HTTP
443  → HTTPS
3306 → MySQL
5432 → PostgreSQL
8080 → Common application port
```

---

# 🧠 Day 17 Key Takeaway

Remember this troubleshooting flow:

```text
Interface
    ↓
IP Address
    ↓
Route
    ↓
Connectivity
    ↓
DNS
    ↓
Port
    ↓
Application
```

The most important commands today are:

```bash
ip addr
ip link
ip route
hostname
hostname -I
ping
getent hosts
ss -tuln
```

---

