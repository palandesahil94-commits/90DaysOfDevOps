# Day 15 – Networking Concepts

---

## 🔹 Task 1: DNS – How Names Become IPs

### What happens when you type google.com?

* Browser asks DNS resolver for IP of google.com
* Resolver queries DNS servers (root → TLD → authoritative)
* Gets IP address and returns it
* Browser connects to that IP via HTTP/HTTPS

### DNS Record Types

* A → Maps domain to IPv4 address
* AAAA → Maps domain to IPv6 address
* CNAME → Alias of another domain
* MX → Mail server for domain
* NS → Name server for domain

### Command

```bash
dig google.com
```

### Observation (example)

* A record: `142.250.183.14`
* TTL: `300`

---

## 🔹 Task 2: IP Addressing

### What is IPv4?

* 32-bit address written as 4 octets (e.g., 192.168.1.10)

### Public vs Private IP

* Public IP → Accessible over internet (e.g., 8.8.8.8)
* Private IP → Internal network use (e.g., 192.168.1.10)

### Private IP Ranges

* 10.0.0.0 – 10.255.255.255
* 172.16.0.0 – 172.31.255.255
* 192.168.0.0 – 192.168.255.255

### Command

```bash
ip addr show
```

### Observation

* Example: `192.168.x.x` → private IP

---

## 🔹 Task 3: CIDR & Subnetting

### What does /24 mean?

* First 24 bits are network, remaining 8 bits for hosts

### Usable Hosts

* /24 → 254
* /16 → 65,534
* /28 → 14

### Why subnet?

* To divide networks for better management, security, and efficiency

### CIDR Table

| CIDR | Subnet Mask     | Total IPs | Usable Hosts |
| ---- | --------------- | --------- | ------------ |
| /24  | 255.255.255.0   | 256       | 254          |
| /16  | 255.255.0.0     | 65,536    | 65,534       |
| /28  | 255.255.255.240 | 16        | 14           |

---

## 🔹 Task 4: Ports – The Doors to Services

### What is a port?

* Logical endpoint that identifies a service on a system

### Common Ports

| Port  | Service |
| ----- | ------- |
| 22    | SSH     |
| 80    | HTTP    |
| 443   | HTTPS   |
| 53    | DNS     |
| 3306  | MySQL   |
| 6379  | Redis   |
| 27017 | MongoDB |

### Command

```bash
ss -tulpn
```

### Observation

* Port 22 → SSH service
* Port 80 → Web server (if running)

---

## 🔹 Task 5: Putting It Together

### curl http://myapp.com:8080

* DNS resolves domain → IP
* TCP connects to port 8080
* HTTP request sent to application

### App can't reach DB (10.0.1.50:3306)

Check:

* Is DB reachable? (ping / telnet / nc)
* Is port 3306 open?
* Firewall/security group blocking?
* DB service running?

---

## 🔹 What I Learned

1. DNS resolution is the first step in any network request
2. CIDR helps efficiently divide networks
3. Ports are critical for identifying services during troubleshooting

---
