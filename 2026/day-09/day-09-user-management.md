Day 09 Challenge

Users & Groups Created
Users:
tokyo
berlin
professor
nairobi

Groups:
developers
admins
project-team


Group Assignments

| User      | Groups Assigned          |
| --------- | ------------------------ |
| tokyo     | developers, project-team |
| berlin    | developers, admins       |
| professor | admins                   |
| nairobi   | project-team             |


Directories Created

| Directory           | Group Owner  | Permissions     |
| ------------------- | ------------ | --------------- |
| /opt/dev-project    | developers   | 775 (rwxrwxr-x) |
| /opt/team-workspace | project-team | 775 (rwxrwxr-x) |

Commands Used
🔹 Task 1: Create Users

sudo useradd -m tokyo
sudo passwd tokyo

sudo useradd -m berlin
sudo passwd berlin

sudo useradd -m professor
sudo passwd professor

# Verify users
cat /etc/passwd | grep -E "tokyo|berlin|professor"
ls /home

🔹 Task 2: Create Groups

sudo groupadd developers
sudo groupadd admins

# Verify groups
cat /etc/group | grep -E "developers|admins"


🔹 Task 3: Assign Users to Groups

sudo usermod -aG developers tokyo

sudo usermod -aG developers berlin
sudo usermod -aG admins berlin

sudo usermod -aG admins professor

# Verify
groups tokyo
groups berlin
groups professor


🔹 Task 4: Shared Directory Setup

sudo mkdir -p /opt/dev-project

sudo chgrp developers /opt/dev-project
sudo chmod 775 /opt/dev-project

# Test as users
sudo -u tokyo touch /opt/dev-project/tokyo_file.txt
sudo -u berlin touch /opt/dev-project/berlin_file.txt

# Verify
ls -ld /opt/dev-project
ls -l /opt/dev-project

🔹 Task 5: Team Workspace

sudo useradd -m nairobi
sudo passwd nairobi

sudo groupadd project-team

sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo

sudo mkdir -p /opt/team-workspace

sudo chgrp project-team /opt/team-workspace
sudo chmod 775 /opt/team-workspace

# Test
sudo -u nairobi touch /opt/team-workspace/nairobi_file.txt

# Verify
ls -ld /opt/team-workspace
ls -l /opt/team-workspace
groups nairobi
groups tokyo




What I Learned
User & Group Management
Learned how to create users and assign them to multiple groups using usermod -aG.
Linux Permissions & Ownership
Understood how chmod and chgrp control access in shared environments.
Real-World Collaboration Setup
Simulated a team-based environment where multiple users can safely work in shared directories.


Troubleshooting Notes
Permission denied error
→ Use sudo or check directory permissions:
ls -ld /path

User cannot access directory
→ Check group membership:
groups username

Changes not reflecting
→ User may need to log out and log back in