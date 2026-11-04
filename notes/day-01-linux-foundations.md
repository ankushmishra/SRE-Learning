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

## Practice: basic navigation commands

Run these commands one by one in the terminal. After `less test.txt`, press
`q` to return to the shell.

```bash
pwd
ls -la
cd ~
mkdir sre-lab
cd sre-lab
touch test.txt
echo "Hello SRE" > test.txt
cat test.txt
less test.txt
cp test.txt backup.txt
mv backup.txt notes.txt
find . -name "*.txt"
history
```

Expected result: `cat test.txt` prints `Hello SRE`, and `find` lists
`./test.txt` and `./notes.txt`. The `>` operator writes to the file, replacing
its contents if it already exists.

If you repeat the exercise, `mkdir sre-lab` may report that the directory
already exists. Continue with `cd ~/sre-lab`; the existing files can be reused.

## Questions to answer
- What is the difference between a terminal and a shell?
- What is the role of the kernel?
- Why do permissions matter in production?
- What happens when a process exits unexpectedly?

## Notes
Write 3 things you learned in your own words:
1.
2.
3.
