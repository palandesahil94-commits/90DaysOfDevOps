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


======================================================================
     LINUX ARCHITECTURE, PROCESSES, AND SYSTEMD INTERVIEW QUESTIONS
======================================================================

----------------------------------------------------------------------
PART 1: CONCEPTUAL QUESTIONS
----------------------------------------------------------------------

Q1: What is the fundamental difference between User Space and Kernel Space?
A1: Kernel Space is the highly privileged core of the operating system 
    where the kernel executes and has unrestricted access to all system 
    hardware (CPU, memory, disks). User Space is the restricted area 
    where user applications, shells, and UI services run. Applications 
    cannot access hardware directly; they must make a System Call (syscall) 
    to request that the kernel perform the task on their behalf.

Q2: Can you explain the difference between fork() and exec() during process creation?
A2: fork() clones the current parent process to create an identical child 
    process with a new PID but the exact same memory snapshot and open files. 
    exec() takes that child process and completely overwrites its memory 
    space with a new program's binary code (like running 'ls' or 'nginx'), 
    transforming it into the target application.

Q3: What is a Zombie process, and how does it differ from an Orphan process?
A3: - A Zombie process has finished executing but remains listed in the 
      process table because its parent hasn't read its exit status yet via 
      a wait() system call. It consumes zero memory or CPU, only a PID slot.
    - An Orphan process is a process that is actively running, but its 
      parent process died. It is immediately adopted by PID 1 (systemd), 
      which manages and cleanly reaps it when it finishes.

Q4: Why is systemd preferred over the older System V init system in modern DevOps workflows?
A4: systemd supports parallel booting of services, which drastically 
    decreases system startup times. It handles complex service dependencies 
    cleanly and uses Linux control groups (cgroups) to track every child 
    process a service spawns, preventing untracked orphan processes from 
    cluttering the system when a service restarts.

----------------------------------------------------------------------
PART 2: SCENARIO & TROUBLESHOOTING QUESTIONS
----------------------------------------------------------------------

Q5: You run 'kill -9 <PID>' on a process, but it refuses to die and is still listed in 'top'. What is happening?
A5: The process is likely stuck in an Uninterruptible Sleep state (D state), 
    usually waiting for blocked hardware resources like disk or network I/O. 
    In this state, the process cannot register or process any incoming 
    signals—including SIGKILL (-9). It will only terminate once the 
    underlying hardware resource responds, or if the server is rebooted.

Q6: A web application container or service crashed unexpectedly, and there are no logs in the application's native directory. How would you troubleshoot this using systemd utilities?
A6: I would use systemd utilities to inspect the centralized system logs:
    1. Run 'systemctl status <service-name>' to check the immediate exit 
       code, uptime state, and the most recent log lines.
    2. Run 'sudo journalctl -u <service-name> -n 50 --no-pager' to inspect 
       the last 50 log entries to find runtime errors, segmentation faults, 
       or Out-Of-Memory (OOM) kills.

Q7: How would you monitor live traffic authentication attempts hitting an SSH server in real-time?
A7: I would use journalctl with the follow flag (-f) filtered by the ssh 
    service unit:
    'sudo journalctl -u ssh -f' (or 'sshd' depending on the Linux distribution).
    This streams incoming logs live to the terminal as connections occur.
======================================================================