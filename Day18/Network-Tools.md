# 🌐 Day 18 — Linux Network Tools

Network tools help developers and system administrators **test connectivity, inspect ports, troubleshoot DNS, send HTTP requests, and diagnose network problems**.

Today we will move from basic networking concepts to practical commands.

---

## 🎯 Today's Goals

By the end of Day 18, you will understand:

- `curl`
- `wget`
- `ss`
- `netstat`
- `traceroute`
- `tracepath`
- `nslookup`
- `dig`
- `host`
- `nc`
- `ip`
- HTTP requests
- DNS troubleshooting
- Port testing
- Basic network diagnostics

---

# 1. Network Tools Overview

| Tool | Purpose |
|---|---|
| `curl` | Transfer data / test HTTP APIs |
| `wget` | Download files |
| `ss` | Inspect sockets and ports |
| `netstat` | Older network statistics tool |
| `traceroute` | Trace network path |
| `tracepath` | Trace network path |
| `nslookup` | DNS lookup |
| `dig` | Detailed DNS lookup |
| `host` | Simple DNS lookup |
| `nc` | Test TCP/UDP connections |
| `ip` | Network configuration |

---

# 2. Check Available Commands

You can check whether a command exists:

```bash
command -v curl
```

Try:

```bash
command -v wget
command -v ss
command -v dig
command -v nc
```

If a command is not installed, you can install the required package on Ubuntu.

For example:

```bash
sudo apt update
sudo apt install curl wget dnsutils netcat-openbsd
```

---

# 3. curl

`curl` is one of the most useful Linux networking commands.

It can communicate with:

```text
HTTP
HTTPS
FTP
and other supported protocols
```

For developers, `curl` is especially useful for testing REST APIs.

---

# 4. Basic curl

Run:

```bash
curl https://example.com
```

This sends an HTTP request and displays the response body.

You can save the response:

```bash
curl https://example.com > page.html
```

Then:

```bash
cat page.html
```

---

# 5. Check HTTP Headers

Use:

```bash
curl -I https://example.com
```

Example output may contain:

```text
HTTP/...
content-type: text/html
content-length: ...
```

The `-I` option requests headers without downloading the normal response body.

---

# 6. Verbose curl

For troubleshooting:

```bash
curl -v https://example.com
```

This can show details about:

```text
DNS resolution
Connection
TLS
Request
Response
```

Verbose output is extremely useful when debugging APIs.

---

# 7. Follow Redirects

Some websites redirect requests.

Use:

```bash
curl -L https://example.com
```

`-L` tells curl to follow HTTP redirects.

---

# 8. Download Using curl

Use:

```bash
curl -O https://example.com/file.zip
```

`-O` saves the file using the remote filename when supported.

You can specify your own filename:

```bash
curl -o myfile.zip https://example.com/file.zip
```

Difference:

```text
-O → use remote filename
-o → specify output filename
```

---

# 9. curl and REST APIs

This is extremely important for Java developers.

Suppose your Spring Boot API is:

```text
http://localhost:8080/api/students
```

Test it:

```bash
curl http://localhost:8080/api/students
```

If the API returns JSON:

```json
[
  {
    "id": 1,
    "name": "Srushti"
  }
]
```

You have successfully tested the API from Linux.

---

# 10. GET Request

A normal curl request is generally a GET request:

```bash
curl http://localhost:8080/api/students
```

Explicitly:

```bash
curl -X GET http://localhost:8080/api/students
```

For normal GET requests, you usually don't need to specify `-X GET`.

---

# 11. POST Request

Suppose your API accepts:

```text
POST /api/students
```

You can send JSON:

```bash
curl -X POST http://localhost:8080/api/students \
-H "Content-Type: application/json" \
-d '{"name":"Srushti","course":"Information Technology"}'
```

Explanation:

```text
-X POST
    ↓
HTTP method

-H
    ↓
HTTP header

-d
    ↓
Request body
```

---

# 12. PUT Request

Example:

```bash
curl -X PUT http://localhost:8080/api/students/1 \
-H "Content-Type: application/json" \
-d '{"name":"Srushti","course":"Java"}'
```

Used to update a resource depending on the API design.

---

# 13. DELETE Request

Example:

```bash
curl -X DELETE http://localhost:8080/api/students/1
```

This sends a DELETE request.

---

# 14. curl Cheat Sheet

```bash
curl URL
```

GET request.

```bash
curl -I URL
```

Headers.

```bash
curl -v URL
```

Verbose troubleshooting.

```bash
curl -L URL
```

Follow redirects.

```bash
curl -O URL
```

Download using remote filename.

```bash
curl -o file URL
```

Save using your filename.

```bash
curl -X POST ...
```

POST request.

---

# 15. wget

`wget` is commonly used for downloading files.

Example:

```bash
wget https://example.com/file.zip
```

It downloads the file to the current directory.

---

# 16. Download with a Custom Filename

```bash
wget -O myfile.zip https://example.com/file.zip
```

Here:

```text
-O → output filename
```

---

# 17. wget Resume

If a download was interrupted, you can sometimes continue it with:

```bash
wget -c URL
```

`-c` means continue.

---

# 18. curl vs wget

| curl | wget |
|---|---|
| Excellent for APIs | Excellent for downloads |
| HTTP request testing | File downloading |
| Supports many protocols | Primarily designed for downloading |
| Common in backend troubleshooting | Common in installation/download scripts |

Both are useful Linux tools.

---

# 19. ss

`ss` displays socket information.

Check listening ports:

```bash
ss -tuln
```

Meaning:

```text
-t → TCP
-u → UDP
-l → listening
-n → numeric
```

---

# 20. Show Listening TCP Ports

```bash
ss -ltn
```

You might see:

```text
LISTEN
127.0.0.1:8080
```

This could indicate a Java/Spring Boot application listening on port `8080`.

---

# 21. Show Listening UDP Ports

```bash
ss -lun
```

---

# 22. Show Processes with Ports

Use:

```bash
sudo ss -tulpn
```

This can show:

```text
Process
PID
Protocol
Local Address
Port
```

This is useful when you need to find which process is using a port.

---

# 23. Find a Java Application Port

Run:

```bash
sudo ss -tulpn | grep java
```

You may see something similar to:

```text
LISTEN ... 127.0.0.1:8080 ... java
```

Now you know that a Java process is listening on port `8080`.

---

# 24. netstat

`netstat` is an older network utility.

Example:

```bash
netstat -tuln
```

However, many modern Linux distributions prefer:

```bash
ss -tuln
```

If `netstat` is unavailable, you may need the `net-tools` package:

```bash
sudo apt install net-tools
```

Then:

```bash
netstat -tuln
```

---

# 25. ss vs netstat

| `ss` | `netstat` |
|---|---|
| Modern tool | Older tool |
| Usually available on modern Linux | May require installation |
| Faster and feature-rich | Still found in older documentation |
| Preferred for many current systems | Useful for legacy environments |

For new Linux practice:

```bash
ss
```

should be your first choice.

---

# 26. traceroute

`traceroute` displays the network path toward a destination.

Example:

```bash
traceroute google.com
```

It may show:

```text
1   Router
2   Network device
3   ISP device
4   ...
```

The exact output depends on your network.

---

# 27. Install traceroute

On Ubuntu:

```bash
sudo apt install traceroute
```

Then:

```bash
traceroute google.com
```

---

# 28. Why Use traceroute?

Suppose:

```text
Your Computer
      ↓
Router
      ↓
ISP
      ↓
Internet
      ↓
Server
```

If traffic has a problem somewhere along the route, traceroute can provide clues about where packets stop receiving responses.

Important:

A `*` in traceroute does not automatically mean that a router is broken. Some routers intentionally do not respond to traceroute probes.

---

# 29. tracepath

Another path diagnostic tool is:

```bash
tracepath google.com
```

It can provide route and path information without requiring all the same privileges as some traceroute configurations.

---

# 30. DNS Tools

Three useful commands are:

```text
nslookup
dig
host
```

They query DNS information.

---

# 31. nslookup

Run:

```bash
nslookup google.com
```

You may see:

```text
Server:
Address:

Name:
Address:
```

It is useful for basic DNS troubleshooting.

---

# 32. dig

`dig` provides detailed DNS information.

Run:

```bash
dig google.com
```

You may see sections such as:

```text
QUESTION SECTION
ANSWER SECTION
AUTHORITY SECTION
```

---

# 33. Query Only the Answer

Use:

```bash
dig +short google.com
```

This gives a compact result.

Example:

```text
IP_ADDRESS
```

---

# 34. Query Different DNS Record Types

### A record

IPv4 address:

```bash
dig google.com A
```

### AAAA record

IPv6 address:

```bash
dig google.com AAAA
```

### MX record

Mail servers:

```bash
dig google.com MX
```

### NS record

Name servers:

```bash
dig google.com NS
```

---

# 35. host

`host` provides a simple DNS lookup.

Run:

```bash
host google.com
```

Example:

```text
google.com has address ...
```

For a mail record:

```bash
host -t MX google.com
```

---

# 36. DNS Tool Comparison

| Tool | Use |
|---|---|
| `nslookup` | Simple DNS troubleshooting |
| `dig` | Detailed DNS information |
| `host` | Simple DNS lookup |
| `getent hosts` | Uses system name-service configuration |

For deeper DNS investigation:

```bash
dig
```

is particularly useful.

---

# 37. nc — Netcat

`nc` is a powerful network utility.

It can be used for:

```text
Port testing
TCP connections
UDP testing
Simple network communication
```

---

# 38. Test a Port

Check whether port `8080` is accepting TCP connections:

```bash
nc -zv localhost 8080
```

Meaning:

```text
-z → scan/listening check without sending normal data
-v → verbose
```

If a service is listening, you may see a successful connection message.

---

# 39. Test PostgreSQL

PostgreSQL commonly uses port `5432`.

Run:

```bash
nc -zv localhost 5432
```

If PostgreSQL is running and reachable on that address/port, the connection test can succeed.

---

# 40. Java Developer Troubleshooting Example

Suppose your Java application cannot connect to PostgreSQL.

You expect:

```text
localhost:5432
```

First check whether something is listening:

```bash
ss -ltn | grep 5432
```

Then test the port:

```bash
nc -zv localhost 5432
```

Then check PostgreSQL itself.

This gives you a logical troubleshooting process:

```text
Java Application
      ↓
Can resolve host?
      ↓
Can reach host?
      ↓
Is port 5432 listening?
      ↓
Can TCP connection be established?
      ↓
Is PostgreSQL accepting the connection?
      ↓
Check Java/database configuration
```

---

# 41. HTTP Status Codes with curl

Run:

```bash
curl -I https://example.com
```

You may see a status such as:

```text
200
```

Common HTTP status codes:

| Code | Meaning |
|---:|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 301 | Permanent redirect |
| 302 | Temporary redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

These are HTTP response codes, not Linux error codes.

---

# 42. Measure HTTP Request Time

Use:

```bash
curl -o /dev/null -s -w "Time: %{time_total}s\n" https://example.com
```

Here:

```text
-o /dev/null
    ↓
Discard response body

-s
    ↓
Silent mode

-w
    ↓
Print selected information
```

This is useful for basic performance checks.

---

# 43. Check HTTP Headers and Body

Use:

```bash
curl -i https://example.com
```

Difference:

```text
-I → headers only

-i → headers + response body
```

---

# 44. Check Redirects

Try:

```bash
curl -I http://example.com
```

Then:

```bash
curl -IL http://example.com
```

`-L` follows redirects.

---

# 45. Network Troubleshooting Workflow

When a service isn't working:

```text
1. Check interface
       ↓
2. Check IP
       ↓
3. Check route
       ↓
4. Check DNS
       ↓
5. Check port
       ↓
6. Check HTTP
       ↓
7. Check application
```

Commands:

```bash
ip addr
```

```bash
ip route
```

```bash
getent hosts example.com
```

```bash
ss -tuln
```

```bash
curl -v URL
```

---

# 46. Practice Lab 🧪

Create a directory:

```bash
mkdir ~/day18-network-tools
cd ~/day18-network-tools
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
echo "===== ROUTES ====="
ip route

echo
echo "===== LISTENING PORTS ====="
ss -tuln

echo
echo "===== DNS ====="
getent hosts google.com

echo
echo "===== HTTP ====="
curl -I https://example.com

} > network-tools-report.txt
```

View it:

```bash
cat network-tools-report.txt
```

---

# 47. Network Diagnostics Project 🛠️

Create a script:

```bash
nano network-check.sh
```

Add:

```bash
#!/bin/bash

echo "===== NETWORK CHECK ====="

echo
echo "Hostname:"
hostname

echo
echo "IP Address:"
hostname -I

echo
echo "Default Route:"
ip route | grep default

echo
echo "DNS Test:"
getent hosts google.com

echo
echo "Listening Ports:"
ss -tuln

echo
echo "HTTP Test:"
curl -I -s https://example.com | head -n 1

echo
echo "===== CHECK COMPLETE ====="
```

Save the file.

Give execute permission:

```bash
chmod +x network-check.sh
```

Run:

```bash
./network-check.sh
```

---

# 48. Save Diagnostic Output

You can save the output:

```bash
./network-check.sh > network-check.log
```

View:

```bash
cat network-check.log
```

Append another test:

```bash
./network-check.sh >> network-check.log
```

This combines concepts from:

```text
Day 6 → Redirection
Day 7 → Pipes
Day 8 → Permissions
Day 17 → Networking
Day 18 → Network Tools
```

---

# 49. Mini Challenge 🔥

Try these yourself.

### Task 1

Check the headers of `example.com`.

```text
?
```

### Task 2

Follow redirects.

```text
?
```

### Task 3

Show listening ports.

```text
?
```

### Task 4

Find Java processes listening on ports.

```text
?
```

### Task 5

Perform a DNS lookup.

```text
?
```

### Task 6

Get only the IP address from DNS.

```text
?
```

### Task 7

Check whether port `8080` is reachable locally.

```text
?
```

### Task 8

Trace the route to `google.com`.

```text
?
```

### Task 9

Download a file with `wget`.

```text
?
```

### Task 10

Test your Spring Boot API using curl.

```text
?
```

---

# 50. Interview Questions 🎯

### Q1. What is curl?

`curl` is a command-line tool for transferring data and is commonly used to test HTTP/HTTPS APIs.

### Q2. Difference between curl and wget?

`curl` is widely used for data transfer and API testing, while `wget` is particularly convenient for downloading files.

### Q3. What does `ss` do?

It displays socket information, including listening and active network connections.

### Q4. Why is `ss` preferred over netstat?

`ss` is the modern socket-statistics tool commonly available on Linux systems, while `netstat` is an older utility.

### Q5. What is DNS?

DNS translates domain names into IP addresses and provides other DNS information.

### Q6. Difference between `dig` and `nslookup`?

Both perform DNS queries, but `dig` generally provides more detailed and flexible DNS information.

### Q7. What is Netcat?

Netcat (`nc`) is a network utility that can create or test TCP/UDP connections.

### Q8. How do you check whether port 8080 is listening?

```bash
ss -ltn | grep 8080
```

### Q9. How do you test port 8080?

```bash
nc -zv localhost 8080
```

### Q10. How can you test a REST API from Linux?

Using:

```bash
curl URL
```

For example:

```bash
curl http://localhost:8080/api/students
```

---

# 51. Day 18 Cheat Sheet

```text
CURL
curl URL
curl -I URL
curl -i URL
curl -v URL
curl -L URL
curl -O URL
curl -o file URL

WGET
wget URL
wget -O file URL
wget -c URL

SOCKETS
ss -tuln
ss -ltn
ss -lun
sudo ss -tulpn

DNS
nslookup domain
dig domain
dig +short domain
host domain
getent hosts domain

ROUTE
ip route

NETWORK
ip addr
ip link

TRACE
traceroute domain
tracepath domain

NETCAT
nc -zv localhost 8080

HTTP
curl -I URL
curl -v URL
curl -X POST ...
```

---

# 🧠 Day 18 Key Takeaway

The most important tools to remember are:

```text
curl       → HTTP/API testing
wget       → downloads
ss         → ports/connections
dig        → DNS investigation
nslookup   → DNS lookup
traceroute → network path
nc         → port testing
ip         → network configuration
```

For a Java backend developer, remember this workflow:

```text
Java Application
       ↓
     Host
       ↓
      DNS
       ↓
      IP
       ↓
     Route
       ↓
     Port
       ↓
     HTTP
       ↓
   API/Database
```

---

# 📌 GitHub Update

Create:

```text
Day18/
└── Network-Tools.md
```

From PowerShell:

```powershell
cd C:\Desktop\Java-Meets-Linux
```

Check:

```powershell
git status
```

Add:

```powershell
git add Day18\Network-Tools.md
```

Commit:

```powershell
git commit -m "docs: add day 18 network tools"
```

Push:

```powershell
git push
```

Verify:

```powershell
git status
```

You should see:

```text
Your branch is up to date with 'origin/main'.
```

---

