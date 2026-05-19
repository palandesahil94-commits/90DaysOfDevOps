Practice Note: Process, Service & Log Inspection

1. Process Inspection
Command: ps aux | grep nginx
Output: sandbox 33 ... grep nginx
Observation: Only grep process found, nginx is not running.
Command: pgrep nginx
Output: No output

Observation: Confirms nginx process is not running.


2. Service Inspection
Command: systemctl status nginx
Output: command not found
Observation: systemctl not available (minimal/container environment).
Command: service nginx status
Output: unrecognized service

Observation: nginx not installed.


3. Log Inspection
Command: journalctl -u nginx
Output: command not found
Observation: journald not available.
Command: tail -n 50 /var/log/syslog
Output: no nginx logs

Observation: no logs related to nginx.


4. Troubleshooting Flow
Problem: nginx service not found.
Steps: Checked processes -> not running; checked service -> systemctl unavailable; tried service-> nginx not installed; checked logs -> no logs found.
Conclusion: nginx is not installed and environment is minimal