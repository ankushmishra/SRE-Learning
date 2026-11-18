# Day 4 - Linux processes, networking, and basic monitoring

## Session goal

Understand how Linux processes work, how to read basic system health signals,
check active network listeners, and troubleshoot service issues safely using
read-only commands. This session combines the fundamentals with a live practical
lab in Ubuntu.

Suggested time: 90-120 minutes, including the theoretical notes and the
exercise work.

## 1. Fundamentals

### 1.1 What is a process?

A process is a running instance of a program. Linux runs many processes at the
same time, including system daemons, user apps, and shell commands. Every
process has a unique process ID (PID), and it may have a parent process (PPID).

Examples of processes include:

- `systemd` - the main system manager
- `sshd` - secure shell service
- `bash` - your terminal shell
- `nginx`, `apache2`, `mysql`, or other services

Understanding which processes exist and how they relate to each other helps you
find the source of a problem.

### 1.2 CPU, memory, and load

Linux systems expose several useful indicators:

- CPU usage: how much work the CPU is doing
- Memory usage: how much RAM is currently used or available
- Load average: how many processes are waiting for CPU over 1, 5, and 15
  minutes
- I/O wait: time spent waiting for storage or other slow devices

A system can be busy without being broken. For example, a database server may
show high CPU or memory use because it is processing a real workload. The goal is
not to panic when numbers rise, but to interpret them correctly.

### 1.3 Process states

Linux processes can be in different states:

- `R` = running
- `S` = sleeping
- `D` = uninterruptible sleep, often waiting on disk or I/O
- `Z` = zombie
- `T` = stopped or traced

A zombie process is especially important to notice. It is not using CPU, but it
still has an entry in the process table because its parent process has not
cleaned it up yet. Many zombies often suggest a parent process is not
reaping its children correctly.

### 1.4 Networking basics

Linux networking is centered on sockets, interfaces, and listening ports.

- A listening port means a service is waiting for new connections.
- `ss` shows active sockets and listening network services.
- `ip addr show` lists interfaces and assigned IP addresses.
- `ping` checks whether a host responds to network requests.

A service might be running but still not reachable if the port is closed,
misconfigured, or blocked by a firewall.

### 1.5 Why monitoring is important

Before changing a service or restarting it, a good operator first asks:

1. Is the process running?
2. Is it using too much CPU or memory?
3. Is it listening on the correct port?
4. Is the system under load or memory pressure?
5. Are there recent errors in logs?

This keeps troubleshooting safe and evidence-based.

### 1.6 /proc and live system information

`/proc` is a virtual filesystem that exposes live kernel and process information.
It is a very useful read-only debugging source. For example, you can inspect a
process's status, memory details, and scheduler information without stopping the
service.

Examples:

- `/proc/<PID>/status` shows process state information
- `/proc/meminfo` shows memory details
- `/proc/loadavg` shows load averages

Use `/proc` carefully and avoid editing files in it unless you are following a
specific, authorized procedure.

## 2. Practical lab

### 2.1 Prepare a Ubuntu workspace

Run these commands in the Ubuntu Terminal:

```bash
mkdir -p ~/sre-lab/day4
cd ~/sre-lab/day4
pwd
```

The final path should end in `/sre-lab/day4`.

### 2.2 Check overall system health

Show how long the machine has been running and current load:

```bash
uptime
```

What to observe:

- current time
- system uptime
- logged-in users
- 1-minute, 5-minute, and 15-minute load averages

Check memory usage:

```bash
free -h
```

`free -h` shows total RAM, used memory, free memory, and available memory in a
human-friendly format. The `available` column is often more informative than the
raw used value when checking whether the system is under memory pressure.

Check a short live sample of CPU and memory activity:

```bash
vmstat 1 5
```

`vmstat` prints system activity. `1` is the sample delay in seconds and `5` is
the number of reports. This gives a quick overview of CPU, memory, swap, and I/O
activity.

### 2.3 Inspect running processes

List processes and their key metrics:

```bash
ps -eo pid,ppid,comm,%cpu,%mem,rss --sort=-%cpu | head -n 20
```

This shows:

- PID
- PPID
- command name
- CPU usage
- memory usage
- resident set size

Now look at the process tree:

```bash
ps -ef --forest | head -n 50
```

`ps --forest` is useful for seeing parent-child relationships. It helps answer
questions like: which service started which worker process?

Inspect the most memory-hungry processes:

```bash
ps -eo pid,comm,%mem,rss --sort=-%mem | head -n 20
```

These are often database servers, app runtimes, or system services. Use this as a
starting point, not as proof of a problem by itself.

### 2.4 Use the process monitor

Run the process overview in batch mode:

```bash
top -b -n 1
```

`top` is one of the most common Linux monitors. `-b` runs in batch mode, so it
prints once and exits. `-n 1` means one refresh cycle.

Key fields to watch:

- `us` = user CPU time
- `sy` = system/kernel CPU time
- `wa` = I/O wait time
- `id` = idle time
- `RES` = resident memory in use
- `COMMAND` = process name

If `htop` is available:

```bash
htop
```

Press `q` to exit `htop`.

### 2.5 Check process state and resource signal

Ask for a quick summary of running processes:

```bash
ps aux --sort=-%cpu | head -n 20
```

This lists all processes with useful details like user, PID, CPU percentage,
memory percentage, state, and command.

Look for unusual or non-running states:

```bash
ps -eo pid,comm,state,stat --sort=-pid | head -n 20
```

Common states include:

- `R` = running
- `S` = sleeping
- `D` = waiting on I/O
- `Z` = zombie
- `T` = stopped or traced

If you see multiple zombie processes, investigate the parent process. A zombie
process is not actively running but still remains in the process table until a
parent reaps it.

### 2.6 Inspect listening network services

Check active TCP and UDP sockets:

```bash
ss -tunlp | head -n 50
```

This shows:

- `-t` = TCP sockets
- `-u` = UDP sockets
- `-n` = numeric addresses
- `-l` = listening sockets
- `-p` = process information when permissions allow

If you only need TCP listeners:

```bash
ss -tulpn | head -n 50
```

This is very useful when checking whether a service is actually expecting
connections on a port.

Show the network interfaces and their addresses:

```bash
ip addr show
```

This displays loopback and active interfaces, plus IPv4 and IPv6 addresses.

Check if the local host is reachable:

```bash
ping -c 3 127.0.0.1
```

This tests whether the local system responds to ICMP echo requests. Some hosts or
networks block ICMP, so a failed ping is not always a sign of a broken service.

### 2.7 Read process information from /proc safely

List the entries in `/proc`:

```bash
ls /proc | head -n 20
```

Inspect one live process status file:

```bash
ps -eo pid,comm --sort=-pid | head -n 5
```

Now choose a PID and read its status file:

```bash
cat /proc/<PID>/status
```

Example:

```bash
cat /proc/$$/status
```

This shows information such as the process state, parent PID, memory details, and
status flags. Replace `$$` with another PID if needed.

Check process and file limits:

```bash
ulimit -a
```

This helps diagnose issues like too many open files, limited memory, or restricted
process size.

### 2.8 Simple troubleshooting workflow

When something is slow or not responding, use a safe read-only sequence:

1. Check whether the process exists:
   ```bash
   ps -ef | grep <process-name>
   ```
2. Check CPU and memory use:
   ```bash
   top
   ps aux
   ```
3. Check which ports are listening:
   ```bash
   ss -tulpn | head -n 50
   ```
4. Check whether the machine is under memory stress:
   ```bash
   free -h
   vmstat 1 5
   ```
5. Review the relevant logs only after confirming the process and port state.

Avoid killing or restarting a process without confirming that it is the correct
service and that the impact is understood.

## 3. Review and explain

Run the commands again without looking back at the examples. Then explain in your
own words:

1. What does `ps -eo pid,ppid,comm,%cpu,%mem,rss --sort=-%cpu` show?
2. Why is `free -h` often more useful than just checking `used` memory?
3. What does `ss -tulpn` help you confirm?
4. Why is `ps --forest` useful for troubleshooting a service chain?
5. What is the difference between a running process and a zombie process?
6. Why is checking `uptime` and `vmstat` helpful before blaming an application?

## 4. Optional stretch

Collect a 3-second snapshot of system activity:

```bash
vmstat 1 3
```

Compare it with:

```bash
top -b -n 1
```

Both show similar system health data, but `top` tends to be easier for a quick
human read, while `vmstat` is easier for repeated command-line snapshots.

Create a file with the top 10 CPU-consuming processes:

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head -n 10 > top-cpu.txt
cat top-cpu.txt
```

This is a basic example of saving monitoring data for later comparison.

## Day 4 notes

Record three things learned, one command that was confusing, and how you
resolved it:

- 3 things I learned:
  1.
  2.
  3.

- One command that was confusing:
  -

- How I resolved it:
  -

- One troubleshooting example I observed:
  -

