# Day-3  
## Commands Cheat Sheet

---

## 📁 Commands Related to File System

| Command | Usage |
|------|------|
| `pwd` | Find the present working directory |
| `ls` | List directory contents. Flags: `-a` (show hidden files), `-l` (list files), `-t` (sort by modified time) |
| `cat` | Show the contents of a file |
| `touch` | Create a new file |
| `mkdir` | Create a new directory |
| `cp` | Copy contents of a file. Use `-r` to recursively copy directories. Example: `cp <source_file> <dest>` |
| `mv` | Move or rename a file/directory. Example: `mv <source> <dest>` |
| `cd` | Change directory. `cd ..` moves to the previous directory |
| `history` | Show previously executed commands |
| `rm` | Remove a file. `rm -r <directory>` removes a directory recursively |
| `tree` | Show directory tree |
| `head` | Show first 10 lines of a file. Example: `head -n 5 <filename>` |
| `tail` | Show last 10 lines of a file. Example: `tail -n 5 <filename>` |

---

## ⚙️ Commands Related to Process Management

| Command | Usage |
|------|------|
| `ps` | List processes running in the current terminal |
| `ps aux` | List all processes with CPU and memory usage |
| `ps -ef` | Show all processes with parent-child relationship |
| `kill` | Kill a process. `kill -9 <PID>` forcefully terminates a process |
| `top` | Show running processes in real time |
| `htop` | Interactive process viewer (scrolling & mouse support) |
| `renice` | Change the priority of a running process |
| `nproc` | Show number of available CPUs |
| `free -h` | Display free memory in human-readable format |
| `df -h` | Show disk space usage in human-readable format |

---

## 🌐 Commands Related to Network Management

| Command | Usage |
|------|------|
| `ping` | Test network connectivity |
| `traceroute` | Trace the network path to a destination |
| `ip addr` | Display IP address information |
| `curl` | Test HTTP/HTTPS endpoints and APIs |
| `ip route` | Show routing table |

---

