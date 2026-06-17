# Day 18 – Shell Scripting (Functions & Strict Mode)

## Task 1: Basic Functions

### Code

```bash
#!/bin/bash
greet() {
    name=$1
    echo "Hello, $name!"
}

add() {
    num1=$1
    num2=$2
    echo "Sum is: $((num1 + num2))"
}

greet "Sahil"
add 10 20
```

### Output

Hello, Sahil!
Sum is: 30

---

## Task 2: Disk & Memory Check

### Code

```bash
#!/bin/bash
check_disk() { df -h /; }
check_memory() { free -h; }

check_disk
check_memory
```

---

## Task 3: Strict Mode

### Explanation

* set -e → exit on error
* set -u → error on undefined variable
* set -o pipefail → fail if any pipe command fails

---

## Task 4: Local Variables

* local variables exist only inside function
* global variables are accessible everywhere

---

## Task 5: System Info Script

### Features

* Hostname & OS
* Uptime
* Disk usage
* Memory usage
* CPU processes

---

## Key Learnings

1. Functions improve code reuse
2. Strict mode prevents script failures
3. Local variables avoid conflicts
