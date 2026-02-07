# Day – 7
**Task - 1**  
Create notes covering:  
Linux File System Hierarchy (the most important directories)  
Practice solving real-world scenarios step by step  
Everything in linux is either a file or a directory (folder). Every file or directory comes uder root (/)  
- The root - **/ **is the top of every file system.
- No folder or directory can be above root. The root user has permission to write and edit this directory.
- **/root** is the root user’s home directory.
- **/bin** directory is for the essential user binaries. It contains all the binaries that are used by all the users of the system. e.g. ps, ls , ping , grep , cp .
- **/sbin** directory is for the essential system binaries. It contains all the binaries or the commands used by the root users for administrative and system maintenance purposes. Ex: iptables, reboot, fdisk, ifconfig, swapon
- **/boot** directory contains all files required for booting of the system. It includes the GRUB bootloader configuration and essential kernel files that are loaded during startup. Ex: initrd.img-2.6.32-24-generic, vmlinuz-2.6.32-24-generic
- **/dev** directory contains the Device files in Linux. These are special files that act as an interface between hardware and software. Ex: /dev/tty1, /dev/usbmon0
- **/etc** directory contains all the configuration files of system applications, users, services, and tools. ‘etc’ stands for editable text configuration. Ex: sudoers, os-release, fstat etc.
- **/home** is the home directory and it is user specific. It contains all the files and directories related to a specific user. The user can create, delete, or modify files only in their own home directory and cannot access others’ home directories.
- **/lib** directory stores the libraries that are required by the applications to run. These include dynamic libraries needed during runtime.
- **/mnt** directory shows external drives are connected, they are temporarily mounted in /mnt. This is where their contents become accessible to the system.
- **/opt** directory is used for installing and storing the configuration and the third-party software and packages that are not part of the default system.
- **/tmp** directory holds the data are temporarily generated during program executions. Normally these data are volatile and are removed when the program execution is completed or when the system reboots.
- **/proc** directory holds the information about system processes. For example, /proc/meminfo gives real-time data about memory usage including total, free, buffer, and cache statistics.
- **/var** directory stands for variable. It stores the data that keeps on changing during the runtime of system. /var/logs is the place where the real time system and application logs reside. This is the first place that we should check if we are debugging or troubleshooting any application or the system.
- /var/log/auth.log can be used to check the login attempts
- /var/opt: Contains variable data for programs installed in /opt
- The data inside /var can tend to grow significantly over period due to which it is monitored regularly.
- **/srv** directory contains Site-specific data served by this system, such as data and scripts for web servers, data offered by FTP servers, and repositories for version control systems. srv stands for service.
- **Hands On:**
- **root@ubuntu-dev:# du -sh /var/log/* | tail -5**  
```bash
0       /var/log/faillog
```
```bash
0       /var/log/lastlog
```
```bash
0       /var/log/wtmp
```
4.0K    /var/log/private  
8.0K    /var/log/journal  
This command du -sh shows the disk usage by the respective file or directory
- Look at a config file in /etc
- **root@ubuntu-dev:# cat /etc/os-release **
- PRETTY_NAME="Ubuntu 24.04.3 LTS"
- NAME="Ubuntu" VERSION_ID="24.04"
- VERSION="24.04.3 LTS (Noble Numbat)"
- VERSION_CODENAME=noble ID=ubuntu ID_LIKE=debian HOME_URL="https://www.ubuntu.com/" SUPPORT_URL="https://help.ubuntu.com/" BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/" PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy" UBUNTU_CODENAME=noble LOGO=ubuntu-logo
- **ls -al ~**
- This will list all the files and directories under home folder.  
## TASK – 2
### Scenario based troubleshooting:
### Scenario – 1 : Service not starting
A web application service called 'myapp' failed to start after a server reboot.  
What commands would you run to diagnose the issue?  
Write at least 4 commands in order.  
**Step 1: systemctl status apache2**
Why: Check the status of the service  
**Step 2: systemctl start apache2**
Why: Check if the service is starting normally or not.  
**Step 3: systemctl is-enabled apache2**
Why: Check if the service is enabled or not. Enabling a service means the service will automatically start when a service reboots.  
**Step 4: systemctl unmask apache2**
Why: If the service is masked, it will not start on reboot. We need to ensure that the service is unmasked by running the above command.  
**Step 5: systemctl enable apache2**
Why: If the service is not enabled, we can enable the service using the above command.  
**Step 6: systemctl list-dependencies apache2**
Why: We need to ensure that if we have any dependencies of my service,they are running as expected.  
A systemd service can be enabled but not start after reboot due to startup failures, dependency issues, failed conditions, masking, rate limiting, or because it is a oneshot service. Enabled only means it is allowed to start, not guaranteed to be running.  
### Scenario – 2 : High CPU Usage
Your manager reports that the application server is slow.  
You SSH into the server. What commands would you run to identify  
Which process is using high CPU?  
  
**Step 1: **top **or **htop****
Why: Check the processes that are running on the system in real time.  
**Step 2: **ps -eo pid,pmem,pcpu,comm --sort =-pcpu | head -n 5****
Why: This command would list out the top 5 processs that are on high CPU  
**Step 3: **ps aux --sort=-%cpu | head -5****
Why: This command would list out the top 10 processs that are on high CPU  
**Step 4: **uptime****
Why: This command will show how long the system has been is running, the users that are running the system and load average in 1, 10 and 15 mins   
**Scenario 3: **Finding Service Logs  
A developer asks: "Where are the logs for the 'docker' service?"  
The service is managed by systemd.  
What commands would you use?  
**Step 1: **systemctl status docker****
Why: Check the status of the service  
**Step 2: **journalctl -u docker****
Why: Since docker is a systemd service, the logs of the docker service would be in journald. We would use the above command to see the logs of the docker service.  
**Step 3: journalctl -u docker -n 100******
Why: This command is to view the 100 lines of logs of that service  
**Step 4: journalctl -u docker -f******
Why: This command is to view logs of that service in real time.  
**Step 5: journalctl -u docker --since "2026-02-01 10:00" --until "2026-02-01 12:00"******
Why: This command is to view the logs within a specific time range  
**Scenario 4:  File Permissions Issue**  
A script at /home/user/backup.sh is not executing.  
When you run it: ./backup.sh  
You get: "Permission denied"  
What commands would you use to fix this?  
**Step 2: **ls -l /home/ubuntu/script.sh****
Why: This command is for reviewing the current permissions of the file  
**Step 2: **chmod 774 script.ssh****
Why: This command will modify the permission for the user to execute the file. 774 means read,write,execute for user. read,write,execute for owner. Only read for others.  
**Step 2: **ls -l /home/ubuntu/script.sh****
Why: Review that the executing permissions are granted for the script.  
**Step 2: **./script.sh****
Why: Execute the script  

