======================================================================
                 LINUX TROUBLESHOOTING RUNBOOK (DAY 05)
======================================================================

TARGET SERVICE / PROCESS: docker (Docker Container Engine)
----------------------------------------------------------------------

1. ENVIRONMENT BASICS
----------------------------------------------------------------------
$ uname -a
Linux devops-node-01 5.15.0-88-generic #98-Ubuntu SMP Mon Oct 2 15:18:56 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
--> OBSERVATION: Running a stable Ubuntu 5.15 Linux kernel on x86_64 architecture.

$ cat /etc/os-release | grep -E '^NAME|^VERSION='
NAME="Ubuntu"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
--> OBSERVATION: OS verified as Ubuntu 22.04 LTS; configuration paths will follow Debian/Ubuntu standards.


2. FILESYSTEM SANITY
----------------------------------------------------------------------
$ mkdir /tmp/runbook-demo
--> OBSERVATION: Directory created successfully; implies the /tmp partition is writable and not full.

$ cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
total 4
-rw-r--r-- 1 devops devops 223 Jun 18 12:00 hosts-copy
--> OBSERVATION: File copy operations and read/write permissions are functioning normally on the primary volume.


3. SNAPSHOT: CPU & MEMORY
----------------------------------------------------------------------
$ ps -o pid,pcpu,pmem,state,comm -C dockerd
  PID %CPU %MEM S COMMAND
 1124  0.1  1.4 S dockerd
--> OBSERVATION: The core dockerd daemon (PID 1124) is running stable, utilizing negligible CPU and 1.4% memory.

$ free -h
               total        used        free      shared  buff/cache   available
Mem:           7.7Gi       2.1Gi       3.3Gi       8.0Mi       2.3Gi       5.2Gi
Swap:          2.0Gi          0B       2.0Gi
--> OBSERVATION: System has 5.2GiB of available memory remaining; absolutely no memory pressure or swap usage detected.


4. SNAPSHOT: DISK & IO
----------------------------------------------------------------------
$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        49G   22G   25G  47% /
--> OBSERVATION: Root filesystem has 25GB (53%) of free storage remaining. No imminent disk-full threats for container storage.

$ sudo du -sh /var/log/docker
12M     /var/log/docker
--> OBSERVATION: Total Docker log directory size is quite small (12MB). No runaway log files are flooding the system.


5. SNAPSHOT: NETWORK
----------------------------------------------------------------------
$ sudo ss -tulpn | grep dockerd
--> OBSERVATION: Command returned empty. This is standard since Docker communicates over a local Unix socket (/var/run/docker.sock) rather than an exposed TCP port by default.

$ curl --unix-socket /var/run/docker.sock http://localhost/version
{"Platform":{"Name":""},"Components":[{"Name":"Engine","Version":"24.0.7"...
--> OBSERVATION: The local Docker daemon API socket is healthy and responding with standard engine engine metrics over curl.


6. LOGS REVIEWED
----------------------------------------------------------------------
$ sudo journalctl -u docker -n 50 --no-pager
Jun 18 11:45:02 devops-node-01 dockerd[1124]: API listen on /var/run/docker.sock
Jun 18 11:45:05 devops-node-01 dockerd[1124]: Loading containers: start.
Jun 18 11:45:06 devops-node-01 dockerd[1124]: Default bridge (docker0) is up with an IP address 172.17.0.1/16
--> OBSERVATION: Service initialized clean. Most recent events show successful interface configuration creation for docker0 network. No container crash loops reported.


7. QUICK FINDINGS
----------------------------------------------------------------------
* The `docker` engine daemon is completely healthy, running on PID 1124, and accepting local API socket connection pathways.
* Host resources (CPU, Memory, and Disk availability) are perfectly clear of restrictions.
* System logs do not highlight any active runtime exceptions or storage engine errors.


8. IF THIS WORSENS (NEXT STEPS)
----------------------------------------------------------------------
If containers begin crashing, failing to resolve network requests, or resource usage unexpectedly surges, execute these three escalating tasks:

[ ] STEP 1: ISOLATE & GRACEFULLY RESTART THE ENGINE
    Execute a restart of the orchestration system daemon to spin up fresh processing sockets:
    Command: sudo systemctl restart docker

[ ] STEP 2: INCREASE LOG VERBOSITY FOR DEBUGGING
    Modify /etc/docker/daemon.json, insert `"log-level": "debug"`, then reload the configuration.
    Inspect engine internals step-by-step using:
    Command: sudo systemctl reload docker && sudo journalctl -u docker -f

[ ] STEP 3: TRACE SYSTEM CALLS (IF PROCESS IS COMPLETELY HUNG)
    If the daemon stops responding to commands and stays stuck in an Uninterruptible (D) or Zombie (Z) state, attach strace directly to the process identifier to catch failing kernel loops:
    Command: sudo strace -p 1124 -c
======================================================================