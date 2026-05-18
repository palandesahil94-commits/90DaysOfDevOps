File and Directory Management
- mkdir: Creates a new directory. The -p flag creates entire parent-child paths at once.
- touch: Creates a new, empty file or updates the timestamp of an existing one.
- cp: Copies files or directories. Use cp -r to copy a directory and its contents.
- mv: Moves or renames files and directories.
- rm: Deletes files. To remove a directory and everything inside it, use rm -rf.
- rmdir: Deletes an empty directory. 

Navigation and Exploration
- These commands help you find your way around the directory structure. 
- pwd: (Print Working Directory) Displays the full absolute path of your current location.
- ls: Lists the files and directories in your current folder. Common options include -l for detailed info and -a to show hidden files.
- cd: (Change Directory) Used to move between folders. Using cd .. takes you up one level, while cd ~ returns you to your home directory.

Networking troubleshooting
Monitoring Processes
These commands allow you to view active processes and system resource usage. 
- ps (Process Status): Provides a snapshot of current processes.
- ps aux: Displays all processes from all users with details like CPU and memory usage.
- ps -ef: Shows full command details and parent-child relationships (PPID).
- top: A real-time, dynamic view of running processes. It updates every few seconds to show which tasks are consuming the most resources.
- htop: An enhanced, interactive version of top with a more user-friendly visual interface and color coding.
- pstree: Displays processes in a tree-like hierarchy to visualize parent-child relationships.
- pgrep: Searches for processes by name and returns their PIDs

Check Local Configuration
Before looking at external factors, verify your local system's setup. 
- ip addr: Displays IP addresses and interface statuses (the modern replacement for ifconfig).
- ip link: Specifically used to check if a network interface is "UP" or "DOWN".
- hostname: Shows the system's current network name.

Verify Basic Connectivity
Once you know your local IP, test if you can reach other devices. 
- ping <IP/Domain>: Sends ICMP requests to check if a host is reachable and measures latency.
- arp -a or ip neigh: Shows the ARP cache, mapping local IP addresses to physical MAC addresses to ensure local communication is working. 