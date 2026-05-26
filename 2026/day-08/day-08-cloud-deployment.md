Day 08 - Cloud Deployment (Nginx on EC2)
Objective:
Deploy a real web server on the cloud and practice server management.


Part 1: Launch Instance & SSH

Instance Details:
Cloud: AWS EC2
OS: Ubuntu 22.04
Instance Type: t2.micro
Public IP: http://13.126.93.28/
SSH Command:
ssh -i my-key.pem ubuntu@13.126.93.28
Output:
Connected successfully to server

-------------------------------------

Part 2: Install Nginx
Update System:
sudo apt update && sudo apt upgrade -y
Install Nginx:
sudo apt install nginx -y
Verify Service:
systemctl status nginx
Output:
Active: running
-------------------------------------

Part 3: Security Group
Rules:
Port 22 → SSH
Port 80 → HTTP
Test URL:
http://13.126.93.28
Output:
Nginx Welcome Page displayed

-------------------------------------

Part 4: Logs Extraction
Navigate:
cd /var/log/nginx
Files:
access.log
error.log
Read Logs:
sudo cat access.log
Save Logs:
sudo cp /var/log/nginx/access.log ~/nginx-logs.txt

-------------------------------------

Download Logs (Local Machine):
scp -i my-key.pem ubuntu@13.126.93.28:~/nginx-logs.txt .

-------------------------------------

Learning Outcome:- Launched cloud server- Connected via SSH- Installed Nginx- Configured firewall- Extracted logs