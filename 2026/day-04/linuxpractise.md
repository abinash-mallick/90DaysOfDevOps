# Day – 4 - Linux Practice: Processes and Services

## Process checks

Every running instance in linux is a process

- To view the running process we use the command `ps`  
  This command gives very minimal output i.e ProcessID, terminal, time and the command that is executed.

- As the command `ps` gives very minimal out, we can use command `ps aux`  
  This command gives more information irrespective of the terminal, also it provides user, CPU and memory utilization along with the Process id, command executed, process status and virtual memory  

  - a – all user  
  - u – user format  
  - x – Irrespective of terminal  

- To view the processes running with respect to the parent process, we can use the command `ps -ef`  
  UID is the user Id, PPID is the parent PID, C is CPU usage, STIME is start time  
  PPID id the main catch here  

- To get the information on running process in real time along with overall system resource usage such as CPU load, memory consumption, and system uptime, we use the command `top`.  
  Also to get a better representation of running process in real time, we use the command `htop`.  
  It allows us to scroll vertically and horizontally, and interact using a pointing device (mouse)

- Command `pgrep` looks through the currently running processes and lists the process IDs which matches the pattern.  

  Examples:  
  - `pgrep -u devops` – This will list the process ran by user devops  
  - `pgrep -l sudo` – It will list the process name as well as process id  
  - `pgrep -i sudo` – It will list the process irrespective of the case  

---

## Services checks

Services are special kind of process that starts and restarts automatically when the linux system boots.  
Services run in background.

- We use the command `systemctl` to manage and control the status of services  
  It will list all the services running.

- `systemctl list-unit-files` : To show all installed unit files on disk with their enabled state  
- `systemctl list-units` : Shows unit files that are loaded into memory and their running state  

list-unit-files → What CAN run  
list-units → What IS running  

- `systemctl start apache2` : Start the service apache2  
- `systemctl stop apache2` : Stop the service apache2  
- `systemctl status apache2` : Status of the service apache2  
- `systemctl enable apache2` : To enable the service, means the service will start automatically when the system reboots  
- `systemctl disable apache2` : To disable the service apache2  
- `systemctl is-enabled apache2` : To check if the service is enabled or not  
- `systemctl mask apache2` : To prevent the service from starting  
- `systemctl unmask apache2` : To unmask the service  

---

## Log checks

`journalctl` command is used to print the log entries from the systemd journal.  
It used to view and manage system logs maintained by the systemd-journald service.  
It provides a centralized and efficient way to access and analyze log data.  

Systemd uses configuration files called "units" to define resources like services, mount points, or sockets.

- `journalctl` – This command will display the recent log messages from all units in reverse chronological order, starting from the most recent entries.  
- `journalctl -n 2` – If we only want to display a specific number of log entries, we can use the `-n` option followed by the desired number  
- `journalctl -u apache2` – To display logs for a specific systemd unit or service  
- `journalctl -u apache2 -n 20` – Displays only the last 20 log entries for apache2 service  
- `journalctl --list-boots` – To view information about previous system boots. It lists all system boot sessions with boot IDs, dates, and times.

