
How to Approach This Drill

You are simulating a real production issue.

Step 1: Pick a service

Example:

ssh
cron
nginx
docker

👉 We’ll use: ssh service

Step 2: Take system snapshot

You check:

Is CPU overloaded?
Is memory full?
Is disk full?
Is network working?
Step 3: Check logs

Logs tell you:

Errors
Failures
Authentication issues
Step 4: Write findings

Short observations like:

“CPU normal”
“No disk pressure”
“SSH logs show failed login attempts”
Step 5: Define next steps

If things get worse:

Restart service
Increase logging
Debug deeper


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

Here is a simple, plain-English breakdown of what these troubleshooting checks actually mean. Think of it as a **quick doctor’s checkup** for a computer server.

---

### 1. Environment Basics: "Who am I talking to?"

Before fixing a machine, you need to know exactly what kind of machine it is.

* **`uname -a`:** Asks the system, *"What engine (kernel) are you running?"*
* **`cat /etc/os-release`:** Asks, *"What brand are you?"* (e.g., Ubuntu, CentOS, RedHat). This tells you where configuration files are kept.

### 2. Filesystem Sanity: "Can I actually write things down?"

Sometimes a system breaks so badly that it locks the hard drive to prevent data corruption (making it "Read-Only").

* **`mkdir` & `cp`:** This is like grabbing a scrap piece of paper, scribbling on it, and checking if your pen works. If the system lets you create a dummy folder and copy a file into it, the storage system is healthy and responsive.

### 3. CPU / Memory: "Is the brain overwhelmed?"

* **`ps` / `top` / `htop`:** Opens up the computer's task manager. It points at a specific app and asks, *"How much of the brain (CPU) are you hogging right now?"*
* **`free -h`:** Checks the short-term memory (RAM). It tells you, *"Out of 8GB of memory, how much space do we have left before the server completely runs out of breath?"*

### 4. Disk / IO: "Is the closet full or jammed?"

* **`df -h`:** Checks the size of your storage closet. If a hard drive hits 100% full, applications instantly crash because they have no room to breathe.
* **`du -sh /var/log`:** Finds the biggest box in the closet. It measures how heavy your log files are to ensure they aren't secretly filling up the entire system.

### 5. Network: "Are the doors open and answering?"

* **`ss -tulpn`:** Checks the building doors (network ports). It asks, *"Is our app actually sitting at Port 80 waiting for visitors, or did it close the door?"*
* **`curl -I` / `ping`:** Knocks on the door from the outside. It’s like shouting, *"Hey, are you awake?"* and waiting to see if it replies with a friendly *"Yes, I am!"* (a `200 OK` response).

### 6. Logs: "What is the diary saying?"

* **`journalctl` & `tail`:** When applications get confused, they write their complaints into a diary (log files). Running these commands is like opening the diary to the **very last page** to see exactly what the application screamed out right before it crashed.
