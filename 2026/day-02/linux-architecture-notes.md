# Architecture of Linux

Linux has a monolithic architecture based on these components: Hardware, kernel, Shell, Application and utilities.

## Kernel
- Kernel is the heart of the linux operating system
- It is the main core component that lies between Hardware and shell
- It is software that manages communication between the hardware and the system and cannot directly interact with directories or files. Instead, the kernel handles communication between the computer system and the hardware.

## Shell
- Shell is the interface between kernel and Application or user
- It takes commands from the user or application and passes it to the kernel to interact with the hardware accordingly.

## Application and utilities
- System utilities are the commend line tools that perform various tasks provided by user to make system management and administration better.
- These utilities enables user to perform different tasks, such as file management, system monitoring, network configuration, user management etc.

---

# Process management in Linux

Process is a running instance of a program. Every program or command that is executed in a Linux system is called a process.

A running shell script or a webserver, running a python application etc are all processes.

Processes are created when programs are executed, involving the allocation of resources and assignment of a unique Process ID (PID).

They are managed through a lifecycle (new, ready, running, waiting, terminated), utilizing Process Control Blocks (PCBs) to track state, CPU scheduling to manage execution, and inter-process communication (IPC) for coordination.

A new process is usually created by an existing "parent" process using a system call, such as fork() which creates a duplicate child process.

A process terminates when it finishes execution or is killed, releasing its resources (memory, files) back to the system.

---

## Systemd
- It is the first process started by the linux kernel (PID 1) and it starts, stops, manages, and monitors everything else on the system.
- systemd starts the system and decides which services start, under what order and under what condition.

---

## Process state
- When a process is created, the system assigns it a state.
- Processes move between states: New, Ready (waiting for CPU), Running (executing), Waiting (waiting for I/O), and Terminated.
- We can view state of processes using command-line tools like ps and top.

### Process States Table

| State | Meaning | Explanation |
|------|--------|-------------|
| R | Running | Executing on CPU or ready to run |
| S | Sleeping | Waiting for an event (interruptible) |
| D | Uninterruptible Sleep | Waiting for I/O (disk/network) |
| T | Stopped | Paused by signal |
| Z | Zombie | Finished but not cleaned up |

---

## Commands related to process management

### Command `ps`
- To view what processes are running in the current terminal  
Use when: Quick check of what you are running

### Command `ps aux`
- To view all the processes of all users, regardless of terminal. It also provides some extra information like USER(process owner), %CPU(CPU utilization), %MEM(memory utilization), VSZ(virtual memory).  
a = all users  
u = user format  
x = no terminal  
Use when: we want CPU/memory usage info

### Command `ps -ef`
- It shows all process with parent-child relationship.  
Use when: Debugging who started a process, Finding parent of zombie/orphan, Tracing service trees  

UID is the user Id, PPID is the parent PID, C is CPU usage, STIME is start time  
PPID id the main catch here.

### Command `top`
- It is a real-time system monitoring utility available on Linux systems. It presents a continuously updated view of running processes along with overall system resource usage such as CPU load, memory consumption, and system uptime.

### Command `htop`
- It is similar to top, but allows us to scroll vertically and horizontally, and interact using a pointing device (mouse). You can observe all processes running on the system, along with their command line arguments, as well as view them in a tree format, select multiple processes and act on them all at once.

### Command `kill`
- To kill a process.  
Ex:  
kill `<PID>`  
kill -9 `<PID>` → forcefully kill a process  
kill -STOP `<PID>` → Temporarily stop a process  
kill -CONT `<PID>` → Resume a stopped process

### Command `renice`
- To manipulate priority of the process.  
Ex:  
renice -n 10 -p `<PID>`  

Here, -n in priority order  
-19 (High) --- n --- 20 (Low)

