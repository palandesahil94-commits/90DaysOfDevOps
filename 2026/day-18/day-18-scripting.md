# Day 18 – Shell Scripting: Functions & Intermediate Concepts

## 📌 Task Summary

Learn how to write reusable shell scripts using:

* Functions
* Return values
* Strict mode (`set -euo pipefail`)
* Local variables
* Build a real-world system info script

---

# ✅ Task 1: Basic Functions

### 📄 functions.sh

```bash
#!/bin/bash

# Function to greet user
greet() {
    echo "Hello, $1!"
}

# Function to add two numbers
add() {
    sum=$(( $1 + $2 ))
    echo "Sum: $sum"
}

# Calling functions
greet "Sahil"
add 5 10
```

### ▶ Output

```
Hello, Sahil!
Sum: 15
```

---

# ✅ Task 2: Functions with Return Values

### 📄 disk_check.sh

```bash
#!/bin/bash

check_disk() {
    echo "Disk Usage:"
    df -h /
}

check_memory() {
    echo "Memory Usage:"
    free -h
}

main() {
    check_disk
    echo "----------------------"
    check_memory
}

main
```

### ▶ Output

```
Disk Usage:
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /

----------------------
Memory Usage:
              total        used        free
Mem:           7.5G        3.2G        2.1G
```

---

# ✅ Task 3: Strict Mode — set -euo pipefail

### 📄 strict_demo.sh

```bash
#!/bin/bash
set -euo pipefail

echo "Demo for strict mode"

# Undefined variable (will fail due to -u)
echo "Value: $UNDEFINED_VAR"

# Command failure (will stop script due to -e)
false

# Pipe failure
cat missingfile.txt | grep "test"
```

### ▶ Observations

* Script stops immediately when error occurs
* Prevents silent failures

### 📘 Explanation

| Flag              | Meaning                               |
| ----------------- | ------------------------------------- |
| `set -e`          | Exit script if any command fails      |
| `set -u`          | Treat undefined variables as errors   |
| `set -o pipefail` | Fail if any command in pipeline fails |

---

# ✅ Task 4: Local Variables

### 📄 local_demo.sh

```bash
#!/bin/bash

demo_local() {
    local var="I am local"
    echo "Inside function: $var"
}

demo_global() {
    var="I am global"
}

demo_local
echo "Outside function (local): ${var:-Not Available}"

demo_global
echo "Outside function (global): $var"
```

### ▶ Output

```
Inside function: I am local
Outside function (local): Not Available
Outside function (global): I am global
```

### 📘 Insight

* `local` variables exist only inside functions
* Global variables persist outside functions

---

# ✅ Task 5: System Info Reporter

### 📄 system_info.sh

```bash
#!/bin/bash
set -euo pipefail

print_header() {
    echo "=============================="
    echo "$1"
    echo "=============================="
}

system_info() {
    print_header "System Info"
    hostname
    uname -a
}

uptime_info() {
    print_header "Uptime"
    uptime
}

disk_usage() {
    print_header "Disk Usage (Top 5)"
    du -h / 2>/dev/null | sort -rh | head -5
}

memory_usage() {
    print_header "Memory Usage"
    free -h
}

cpu_processes() {
    print_header "Top CPU Processes"
    ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head -5
}

main() {
    system_info
    uptime_info
    disk_usage
    memory_usage
    cpu_processes
}

main
```

### ▶ Output (Sample)

```
==============================
System Info
==============================
my-host
Linux my-host 5.15.0 ...

==============================
Uptime
==============================
10:30:12 up 2 days, 3 users

==============================
Disk Usage (Top 5)
==============================
2.5G /var
1.8G /usr

==============================
Memory Usage
==============================
Mem: 7.5G ...

==============================
Top CPU Processes
==============================
PID CMD %MEM %CPU
1234 chrome 10.5 30.2
```

---

# 📚 What I Learned (Key Points)

1. Functions help make scripts reusable and cleaner
2. `set -euo pipefail` prevents hidden bugs and improves script reliability
3. Local variables help avoid conflicts and keep scope controlled

---

# 🎯 Conclusion

This exercise helped me move from basic scripting to writing structured, production-ready scripts using best practices like strict mode, modular functions, and proper variable handling.

---
