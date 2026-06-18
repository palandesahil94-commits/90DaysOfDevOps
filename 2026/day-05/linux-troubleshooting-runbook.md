# Linux Troubleshooting Runbook

## 🔧 Target Service / Process

Service: **ssh**

---

## 🖥️ Environment Basics

### Command:

```bash
uname -a
```

**Observation:**
System is running Linux kernel 5.x on Ubuntu.

---

### Command:

```bash
cat /etc/os-release
```

**Observation:**
Ubuntu 22.04 LTS detected.

---

## 📁 Filesystem Sanity

### Command:

```bash
mkdir /tmp/runbook-demo
cp /etc/hosts /tmp/runbook-demo/hosts-copy
ls -l /tmp/runbook-demo
```

**Observation:**
Directory and file created successfully. Filesystem is writable.

---

## ⚙️ CPU & Memory Snapshot

### Command:

```bash
ps -o pid,pcpu,pmem,comm -C sshd
```

**Observation:**
SSH process using very low CPU and memory (<1%).

---

### Command:

```bash
free -h
```

**Observation:**
Sufficient free memory available. No memory pressure.

---

## 💾 Disk & I/O Snapshot

### Command:

```bash
df -h
```

**Observation:**
Disk usage below 70%. No immediate disk space issues.

---

### Command:

```bash
du -sh /var/log
```

**Observation:**
Log directory size is moderate. No abnormal growth.

---

## 🌐 Network Snapshot

### Command:

```bash
ss -tulpn | grep ssh
```

**Observation:**
SSH is listening on port 22.

---

### Command:

```bash
curl -I localhost
```

**Observation:**
Localhost responding. Network stack is working.

---

## 📜 Logs Reviewed

### Command:

```bash
journalctl -u ssh -n 50
```

**Observation:**
Recent logs show multiple failed login attempts.

---

### Command:

```bash
tail -n 50 /var/log/auth.log
```

**Observation:**
Repeated "Failed password" entries detected.

---

## 🔍 Quick Findings

* System resources (CPU, memory, disk) are healthy
* SSH service is running and listening correctly
* Multiple failed login attempts detected in logs
* Possible brute-force or unauthorized access attempts

---

## 🚨 If This Worsens (Next Steps)

1. **Secure SSH**

   * Disable password login
   * Enable key-based authentication

2. **Block suspicious IPs**

   ```bash
   sudo ufw deny from <ip>
   ```

3. **Enable deeper debugging**

   * Increase SSH log level in `/etc/ssh/sshd_config`
   * Use tools like `fail2ban`

---

## ✅ Conclusion

System is stable, but security risk detected due to repeated failed SSH login attempts. Preventive actions recommended.
