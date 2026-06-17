# Day 17 – Shell Scripting (Loops, Arguments, Error Handling)

## 📌 Task 1: For Loop

### Script: `for_loop.sh`

```bash
#!/bin/bash

fruits=("Apple" "Banana" "Mango" "Orange" "Grapes")

for fruit in "${fruits[@]}"
do
  echo "Fruit: $fruit"
done
```

### Output

```
Fruit: Apple
Fruit: Banana
Fruit: Mango
Fruit: Orange
Fruit: Grapes
```

---

### Script: `count.sh`

```bash
#!/bin/bash

for i in {1..10}
do
  echo $i
done
```

### Output

```
1 2 3 4 5 6 7 8 9 10
```

---

## 📌 Task 2: While Loop

### Script: `countdown.sh`

```bash
#!/bin/bash

echo "Enter a number:"
read num

while [ $num -ge 0 ]
do
  echo $num
  num=$((num - 1))
done

echo "Done!"
```

### Output Example

```
Enter a number:
5
5
4
3
2
1
0
Done!
```

---

## 📌 Task 3: Command-Line Arguments

### Script: `greet.sh`

```bash
#!/bin/bash

if [ -z "$1" ]
then
  echo "Usage: ./greet.sh <name>"
else
  echo "Hello, $1!"
fi
```

### Output

```
./greet.sh Sahil
Hello, Sahil!
```

---

### Script: `args_demo.sh`

```bash
#!/bin/bash

echo "Script Name: $0"
echo "Total Arguments: $#"
echo "All Arguments: $@"
```

### Output Example

```
./args_demo.sh one two three

Script Name: ./args_demo.sh
Total Arguments: 3
All Arguments: one two three
```

---

## 📌 Task 4: Install Packages via Script

### Script: `install_packages.sh`

```bash
#!/bin/bash

# Check if running as root
if [ "$EUID" -ne 0 ]; then
  echo "❌ Please run as root"
  exit 1
fi

packages=("nginx" "curl" "wget")

for pkg in "${packages[@]}"
do
  if dpkg -s "$pkg" &> /dev/null
  then
    echo "✅ $pkg is already installed"
  else
    echo "⬇️ Installing $pkg..."
    apt update -y && apt install -y "$pkg"
  fi
done
```

### Output Example

```
nginx is already installed
Installing curl...
Installing wget...
```

---

## 📌 Task 5: Error Handling

### Script: `safe_script.sh`

```bash
#!/bin/bash

set -e

mkdir /tmp/devops-test || echo "Directory already exists"

cd /tmp/devops-test || echo "Failed to enter directory"

touch test.txt || echo "Failed to create file"

echo "Script completed successfully"
```

### Output

```
Directory already exists (if already present)
Script completed successfully
```

---

## 📚 What I Learned (Key Takeaways)

1. **Loops (for & while)** help automate repetitive tasks efficiently in scripts.
2. **Command-line arguments ($1, $#, $@)** make scripts dynamic and reusable.
3. **Error handling (set -e, ||)** ensures scripts fail safely and provide useful feedback.

---

## ✅ Bonus Commands Used

* `dpkg -s <package>` → Check if package is installed
* `$EUID` → Check if script is run as root
* `set -e` → Exit immediately if a command fails

---

## 🚀 Next Step

Try combining everything:
👉 Build a script that takes package names as arguments and installs them dynamically.

---
