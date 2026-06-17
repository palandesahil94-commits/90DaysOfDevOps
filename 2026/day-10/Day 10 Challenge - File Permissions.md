# Day 10 Challenge - File Permissions

## Files Created

* devops.txt
* notes.txt
* script.sh
* project/

## Permission Changes

### script.sh

Before:
-rw-r--r--
After:
-rwxr-xr-x

### devops.txt

Before:
-rw-r--r--
After:
-r--r--r--

### notes.txt

Before:
-rw-r--r--
After:
-rw-r-----

### project directory

Permissions:
drwxr-xr-x

## Commands Used

* touch devops.txt
* echo "This is my notes file" > notes.txt
* vim script.sh
* ls -l
* cat notes.txt
* vim -R script.sh
* head -n 5 /etc/passwd
* tail -n 5 /etc/passwd
* chmod +x script.sh
* ./script.sh
* chmod -w devops.txt
* chmod 640 notes.txt
* mkdir project
* chmod 755 project

## What I Learned

1. Linux permissions control access using read, write, execute for owner, group, and others.
2. chmod is used to modify permissions using symbolic (+x, -w) or numeric (755, 640) methods.
3. Incorrect permissions can block execution or modification of files, causing "Permission denied" errors.


Why 755 for directories?”

Answer:

Because users need execute (x) permission to enter a directory and read (r) to list files.