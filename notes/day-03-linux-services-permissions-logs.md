# Day 3 - Linux services, permissions, and logs

## Session goal

Practice inspecting Linux services, understanding file permissions, and
reading system logs from an Ubuntu Terminal. All commands are for Ubuntu.
Service commands in this lesson are read-only; do not start, stop, or change
system services as part of this lab.

Suggested time: 90-120 minutes, including the exercises and notes.

## 1. Prepare an Ubuntu lab workspace

Run these commands in the Ubuntu Terminal:

```bash
mkdir -p ~/sre-lab/day3
cd ~/sre-lab/day3
pwd
```

The final path should end in `/sre-lab/day3`. `mkdir -p` creates the directory
if it does not already exist.

## 2. Inspect services safely

List running service units:

```bash
systemctl list-units --type=service --state=running --no-pager
```

`systemctl` talks to systemd, Ubuntu's service and system manager. This command
only lists services; it does not change them. Your list may differ from the
examples or from another Ubuntu machine.

Check the state of Ubuntu's system journal service:

```bash
systemctl status systemd-journald --no-pager
```

Look for `Active: active (running)`. The pager is disabled so the output
returns directly to the shell. `systemd-journald` collects system and service
messages.

Show any units systemd considers failed:

```bash
systemctl --failed --no-pager
```

No failed units is a normal result. A failed unit is a clue to investigate,
not proof by itself that users are affected.

## 3. Understand Linux permissions

Create a file and directory for permission practice:

```bash
touch permissions-demo.txt
mkdir permissions-demo
ls -l permissions-demo.txt
ls -ld permissions-demo
```

In `ls -l` output, the first permission character identifies the type
(`-` for a regular file and `d` for a directory). The next three characters
are the owner's permissions, the next three are the group's, and the final
three are permissions for everyone else. `r` means read, `w` means write, and
`x` means execute on a file or traverse on a directory.

Show numeric and symbolic permissions, owner, and group:

```bash
stat -c '%A %a %U:%G %n' permissions-demo.txt
```

`%A` displays symbolic permissions, `%a` displays their numeric form, `%U`
shows the owner, `%G` shows the group, and `%n` shows the name. A newly
created file's permissions depend on your `umask`.

Check your account and default permission mask:

```bash
id
umask
```

`id` shows your user ID and group memberships. `umask` shows which permission
bits are removed by default when new files and directories are created.

Restrict the practice file to its owner:

```bash
chmod 600 permissions-demo.txt
ls -l permissions-demo.txt
```

`chmod` changes permissions. `600` means the owner can read and write, while
the group and everyone else have no permissions. The owner should still be
able to read the file:

```bash
cat permissions-demo.txt
```

Allow the owner to read and write and the group to read:

```bash
chmod 640 permissions-demo.txt
ls -l permissions-demo.txt
```

`640` means owner `rw-`, group `r--`, and everyone else `---`. Numeric
permission digits add read (`4`), write (`2`), and execute (`1`) bits.

Restrict directory access to its owner:

```bash
chmod 700 permissions-demo
ls -ld permissions-demo
```

`700` gives the owner read, write, and traverse permissions; group and others
get none. On directories, `x` is needed to enter or access items inside.

These permission changes affect only the practice items under
`~/sre-lab/day3`. Avoid practicing `chmod` or ownership changes on system
paths. `chown` is intentionally not used in this lab because changing
ownership usually requires administrator privileges.

## 4. Read Ubuntu system logs

Show the most recent system journal entries from this boot:

```bash
journalctl -b -n 20 --no-pager
```

`journalctl` reads the systemd journal. `-b` limits results to the current
boot, `-n 20` shows the latest 20 entries, and `--no-pager` returns to the
shell after printing. Some entries may be hidden unless your account has
permission to read the full system journal.

Show warning and more severe messages from this boot:

```bash
journalctl -b -p warning -n 20 --no-pager
```

`-p warning` filters by priority, including warning, error, critical, alert,
and emergency messages. No results can be normal; it does not necessarily
mean logging is broken.

Inspect messages from the journal service:

```bash
journalctl -u systemd-journald -n 20 --no-pager
```

`-u` filters by systemd unit. Replace `systemd-journald` with a service name
when investigating that service, for example `ssh.service` if it is installed.

Create and find a harmless practice log message:

```bash
logger -t sre-day3 "Test log entry from the Day 3 Ubuntu lab"
journalctl -t sre-day3 -n 5 --no-pager
```

`logger` sends a message to the system logging service. `-t` sets its tag;
`journalctl -t` filters for that tag. Journal visibility depends on the
machine's configuration and your account permissions.

Follow new journal entries live:

```bash
journalctl -f
```

This keeps running and displays new entries as they arrive. Press `Ctrl+C` to
stop following and return to the shell.

Ubuntu installations may also have traditional log files such as
`/var/log/syslog`, depending on installed logging services. Check before
reading one:

```bash
ls -l /var/log/syslog
```

If the file exists, inspect its last 20 lines:

```bash
sudo tail -n 20 /var/log/syslog
```

`sudo` runs this read command with administrator privileges and may prompt for
your Ubuntu password. If the file does not exist, use `journalctl`; do not
install or change logging services for this exercise.

## 5. A simple troubleshooting workflow

When a service appears unhealthy, use read-only checks first:

1. Check whether the service is known and running with
   `systemctl status SERVICE --no-pager`.
2. Check for failed units with `systemctl --failed --no-pager`.
3. Read recent service messages with
   `journalctl -u SERVICE -n 50 --no-pager`.
4. Note the time, error message, and whether the issue is ongoing.
5. Avoid restarting or changing a production service without authorization and
   a clear recovery plan.

Replace `SERVICE` with the actual unit name, such as `systemd-journald`.
Service names and available logs vary by Ubuntu installation.

## 6. Review and explain

Explain in your own words:

1. What do the owner, group, and other permission columns mean?
2. What permissions does `640` grant?
3. Why does a directory need the `x` permission to be traversed?
4. What information can `systemctl --failed` provide?
5. How would you read recent logs for one service?
6. Why should an SRE inspect status and logs before restarting a service?

## Day 3 notes

Record three things learned, one command that was confusing, and how you
resolved it:

1.
2.
3.

Confusing command / resolution:

Troubleshooting observation:

