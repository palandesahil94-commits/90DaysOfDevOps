# Day 11 Challenge – File Ownership

## Files & Directories Created

* devops-file.txt
* team-notes.txt
* project-config.yaml
* app-logs/
* heist-project/
* bank-heist/

## Ownership Changes

* devops-file.txt: sahil:sahil → tokyo:sahil → berlin:sahil
* team-notes.txt: sahil:sahil → sahil:heist-team
* project-config.yaml: sahil:sahil → professor:heist-team
* app-logs/: sahil:sahil → berlin:heist-team
* heist-project/: sahil:sahil → professor:planners (recursive)

Bank Heist Files:

* access-codes.txt → tokyo:vault-team
* blueprints.pdf → berlin:tech-team
* escape-plan.txt → nairobi:vault-team

## Commands Used

```bash
ls -l
touch filename

sudo chown user filename
sudo chgrp group filename
sudo chown user:group filename

sudo chown -R user:group directory/

sudo useradd username
sudo groupadd groupname
```

## What I Learned

1. Every file has an owner and a group that controls access.
2. chown is used to change ownership, chgrp for group changes.
3. Recursive (-R) is useful for updating entire directory structures at once.
