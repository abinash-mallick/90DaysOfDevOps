# Day 11 Challenge

## Files & Directories Created

### Task:1

```
-rw-rw-r-- 1 ubuntu ubuntu 28840 Feb 12 17:14 app.log
```
- Here, Ubuntu is the owner of the file and ubuntu is the default group for the file app.log
  The permissions : rw- read and write for owner, rw- read and write for group, r-- read only for others
  
  What's the difference between owner and group?
  Owner is the user who has created the file. Group is a collection of users who share permissions with the file.
  Any user who is a part of group will share the group permissions.

![img](img/Picture1.PNG)


### Task:2 

- Created a file devops-file.txt
  ```
  touch devops-file.txt
  ```
  ![img](img/Picture2.PNG)
- Check the current owner of file
  ```
  ls -l devops-file.txt
  ```
- Change owner to tokyo (create user if needed)
  ```
  id tokyo
  sudo chown tokyo devops-file.tzt
  ```
- Change owner to berlin
  ```
  id berlin
  sudo chown berlin devops-file.txt
  ```
- Verify the changes
  ```
  ls -l devops-file.txt
  ```
  ![img](img/Picture3.PNG)

  
### Task:3 

- Created file team-notes.txt
  ```
  touch team-notes.txt
  ```
- Check current group
  ```
  ubuntu@ip-172-31-24-250:~$ ls -l team-notes.txt 
  -rw-rw-r-- 1 ubuntu ubuntu 0 Feb 13 06:39 team-notes.txt
  ```
- Created group
  ```
  sudo groupadd heist-team
  ```
- Change file group to heist-team
  ```
  sudo chgrp heist-team team-notes.txt
  ```
- Verified the changes
  ```
  ubuntu@ip-172-31-24-250:~$ ls -l team-notes.txt
  -rw-rw-r-- 1 ubuntu heist-team 0 Feb 13 06:39 team-notes.txt
  ```
  ![img](img/Picture4.PNG)


### Task:4 

- Created file project-config.yaml
  ```
  touch project-config.yaml
  ```
- Changed owner to professor AND group to heist-team (one command)
  ```
  sudo chown professor:heist-team project-config.yaml
  ```
- Created directory app-logs/
  ```
  mkdir app-logs
  ```
- Changed its owner to berlin and group to heist-team
  ```
  sudo chown berlin:heist-team app-logs
  ```
  ![img](img/Picture5.PNG)


### Task:5 

- Create directory structure
  ```
  mkdir -p heist-project/vault
  mkdir -p heist-project/plans
  touch heist-project/vault/gold.txt
  touch heist-project/plans/strategy.conf
  ```
- Created group planners
  ```
  sudo groupadd planners
  ```
- Change ownership of entire heist-project/ directory:
  ```
  sudo chown -R professor:planners heist-project/
  ```
- Verifed all files and subdirectories changed
  ```
  ls -lR heist-project
  ```
  ![img](img/Picture6.PNG)


### Task:6 

- Created groups
  ```
  sudo groupadd vault-team
  sudo groupadd tech-team
  ```
- Created directory
  ```
  mkdir bank-heist
  ```
- created 3 files inside bank-heist
  ```
  touch bank-heist/access-codes.txt
  touch bank-heist/blueprints.pdf
  touch bank-heist/escape-plan.txt
  ```
- Set different ownership:
  `access-codes.txt` → owner: `tokyo`, group: `vault-team`
  ```
  sudo chown tokyo:vault-team heist/access-codes.txt
  sudo chown berlin:tech-team heist/blueprints.pdf
  sudo chown nairobi:vault-team heist/escape-plan.txt
  ```
   ![img](img/Picture7.PNG)



































