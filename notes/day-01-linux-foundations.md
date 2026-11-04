# Day 1 - Linux fundamentals

## Why this matters for SRE
Linux is the base layer of most production systems. SREs need to understand how processes, files, permissions, logs, and services behave under real load.

## Core concepts
- Kernel: core of the OS; manages CPU, memory, disk, devices, and processes.
- Shell: user interface that interprets commands.
- Terminal: window or program where you type commands.
- Filesystem: hierarchy starting from `/`.
- Process: running instance of a program.
- Daemon: background service such as SSH or web server.

## Commands to practice
```bash
pwd
ls -la
cd /tmp
mkdir sre-lab
touch notes.txt
cp notes.txt notes-copy.txt
mv notes-copy.txt renamed.txt
rm renamed.txt
cat /etc/os-release
head -n 20 /etc/passwd
tail -n 20 /var/log/syslog
find / -maxdepth 2 -type f 2>/dev/null | head
man ls
history
```

## Questions to answer
- What is the difference between a terminal and a shell?
- What is the role of the kernel?
- Why do permissions matter in production?
- What happens when a process exits unexpectedly?

## Exercise
Inside a safe lab environment, create a folder, create a file, move it, and confirm it is present by listing the directory.

## Notes
Write 3 things you learned in your own words:
1.
2.
3.
