# Day 16 – Shell Scripting Basics

## Task 1: First Script

### Code (hello.sh)

```bash
#!/bin/bash
echo "Hello, DevOps!"
```

### Output

```
Hello, DevOps!
```

### Observation

Without shebang, script may not run correctly using ./hello.sh. Shebang defines interpreter.

---

## Task 2: Variables

### Code (variables.sh)

```bash
#!/bin/bash
NAME="Sahil"
ROLE="DevOps Engineer"

echo "Hello, I am $NAME and I am a $ROLE"
```

### Output

```
Hello, I am Sahil and I am a DevOps Engineer
```

### Observation

* Double quotes expand variables
* Single quotes do not

---

## Task 3: User Input

### Code (greet.sh)

```bash
#!/bin/bash
read -p "Enter your name: " NAME
read -p "Enter your favourite tool: " TOOL

echo "Hello $NAME, your favourite tool is $TOOL"
```

### Output

```
Hello Sahil, your favourite tool is Docker
```

---

## Task 4: If-Else Conditions

### Code (check_number.sh)

```bash
#!/bin/bash
read -p "Enter a number: " NUM

if [ $NUM -gt 0 ]; then
  echo "Positive"
elif [ $NUM -lt 0 ]; then
  echo "Negative"
else
  echo "Zero"
fi
```

---

### Code (file_check.sh)

```bash
#!/bin/bash
read -p "Enter filename: " FILE

if [ -f "$FILE" ]; then
  echo "File exists"
else
  echo "File does not exist"
fi
```

---

## Task 5: Server Check Script

### Code (server_check.sh)

```bash
#!/bin/bash

SERVICE="nginx"

read -p "Do you want to check the status of $SERVICE? (y/n): " CHOICE

if [ "$CHOICE" = "y" ]; then
  systemctl status $SERVICE

  if systemctl is-active --quiet $SERVICE; then
    echo "$SERVICE is running"
  else
    echo "$SERVICE is NOT running"
  fi
else
  echo "Skipped."
fi
```

---

## Key Learnings

1. Shebang ensures correct interpreter execution
2. Variables and user input make scripts dynamic
3. If-else conditions enable decision making in scripts
