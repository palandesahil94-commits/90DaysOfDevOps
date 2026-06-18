Linux Architecture, Processes, and systemd
1. Core Components of Linux
Linux is divided into two primary execution spaces to ensure security and stability:

Kernel: The core of the OS. It has direct access to the hardware (CPU, memory, disks) and manages system resources.

User Space: The area where user applications, shells, and UI run. Applications cannot touch hardware directly; they must ask the kernel via System Calls (syscalls).

Init / systemd: The very first process started by the kernel during boot (PID 1). It acts as the parent of all other processes and initializes the system.

2. Process Creation and Management
A process is simply a program in execution. Every process has a unique PID (Process ID).

How Processes are Created
fork(): An existing parent process duplicates itself to create a child process.

exec(): The child process replaces its own code/data with the new program to be executed.

Example: When you type ls in your terminal, the shell calls fork() to clone itself, and then the clone calls exec(ls) to execute the listing tool.

Process States
Running/Runnable (R): The process is either currently using the CPU or waiting in the execution queue.

Sleeping (S / D): * Interruptible (S): Waiting for an event or signal (e.g., waiting for user keyboard input).

Uninterruptible (D): Waiting directly on hardware, usually disk I/O. Cannot be killed by signals in this state.

Stopped (T): Paused by a user or signal (e.g., pressing Ctrl + Z).

Zombie (Z): The process has finished executing, but its parent hasn't read its exit status yet. It consumes no resources except a slot in the process table.

3. What is systemd and Why it Matters
systemd is the modern init system and service manager used by most Linux distributions today.

Why it Matters for DevOps
Parallel Booting: It starts services concurrently, making system boot significantly faster.

Dependency Management: If Service B depends on Service A, systemd ensures A starts properly before launching B.

Process Tracking: It uses Linux cgroups (control groups) to trace exactly which processes belong to a service, ensuring that stopping a service leaves no orphan processes behind.

4. 5 Daily DevOps Commands (with Examples)
1. ps (Process Status)
Used to snapshot current active processes.

Bash
# Snapshot all running processes with user and resource details
ps aux | grep nginx
2. top or htop (Table of Processes)
Provides a dynamic, real-time view of system resource usage (CPU, Memory).

Bash
# Launch interactive real-time resource monitor
top
3. kill (Terminate Processes)
Sends a signal to a process to terminate it using its PID.

Bash
# Gracefully terminate process 1234 (SIGTERM)
kill 1234

# Forcefully kill process 1234 if it's frozen (SIGKILL)
kill -9 1234
4. systemctl (Control systemd)
The central utility to view and manage services.

Bash
# Restart the Docker service after a configuration change
sudo systemctl restart docker

# Check if the Nginx service is running correctly
systemctl status nginx
5. journald / journalctl (Query systemd logs)
The built-in logging tool that collects messages from the kernel and services.

Bash
# View live, real-time logs for a specific service (e.g., SSH daemon)
sudo journalctl -u ssh -f