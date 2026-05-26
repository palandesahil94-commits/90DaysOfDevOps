Day 07: Linux File System & Troubleshooting Notes

Part 1: File System Hierarchy

/ → Root directory. Starting point of Linux.
Used when exploring system structure.
/home → User directories.
Used for accessing user files.
/root → Admin home directory.
Used for root-level operations.
/etc → Configuration files.
Used when modifying system/service configs.
/var/log → Logs for system/services.
Used for debugging issues.
/tmp → Temporary files.
Used during testing/scripts.
/bin → Essential commands.
Used for basic Linux operations.
/usr/bin → Installed software binaries.
Used to verify installed tools.
/opt → Third-party apps.
Used for custom installations.

Part 2: Scenario Practice

Service Not Starting:

1. systemctl status myapp → Check state
2. journalctl -u myapp → Check logs
3. systemctl is-enabled myapp → Boot status
4. systemctl start myapp → Start service

High CPU Usage:

1. top → Live CPU
2. ps aux --sort=-%cpu → Top processes
3. kill -9 PID → Stop process
Service Logs:
1. systemctl status docker
2. journalctl -u docker -n 50
3. journalctl -f

Permission Issue:

1. ls -l file
2. chmod +x file
3. ./file
Key Takeaway:
Check status → logs → config → fix