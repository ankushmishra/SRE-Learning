# Day 21 - Automate a safe check

## Session goal

Wrap one localhost health probe in a short shell script with a two-second
timeout, a clear result, and explicit exit status. Test it once with the server
up and once down. The script must observe only; it must not restart the server.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

Safe automation is predictable, bounded, and observable. A timeout bounds how
long a check can wait. An exit code lets another tool or operator distinguish
success from failure. Repeating a read-only probe is safe; blindly repeating a
state-changing operation may not be. Add automatic recovery only after the
behavior and risks are understood.

## 2. Practical lab

From Ubuntu/WSL, in `~/sre-followon-lab`, create `probe.sh`:

```bash
#!/usr/bin/env bash
url="http://127.0.0.1:8000/health.txt"
status="$(curl -sS --max-time 2 -o /dev/null -w '%{http_code}' "$url")"
curl_status=$?

if [ "$curl_status" -ne 0 ]; then
  printf 'probe failed: curl_exit=%s http_status=%s\n' "$curl_status" "$status"
  exit 1
fi

if [ "$status" = "200" ]; then
  printf 'probe ok: HTTP %s\n' "$status"
  exit 0
fi

printf 'probe failed: HTTP %s\n' "$status"
exit 1
```

Make it executable and run with the local service up:

```bash
chmod +x probe.sh
./probe.sh
echo "$?"
```

Expected exit code is `0`. Press `Ctrl+C` in Terminal 1 to stop only your lab
server, run the probe again, and confirm it exits nonzero within two seconds.
Restart the server and confirm the script returns to `0`. Do not add a restart
command to the script. Save the script and both results in your lab notes.

## 3. Completion check and notes

Show the success and failure outputs. Explain what the script changes (nothing
on the service), how its timeout works, and why it does not automatically
restart the server.

- Success output and exit code:
- Failure output and exit code:
- Timeout behavior:
- Notes file:

## Done when

- [ ] The check returns 0 for HTTP 200.
- [ ] The check returns nonzero when the server is down or the HTTP status is not 200.
- [ ] Both outcomes finish within the two-second request timeout plus small shell overhead.
- [ ] The script performs no restart or other state-changing action.

## Reading

[Google SRE Book - The Evolution of Automation at Google](https://sre.google/sre-book/automation-at-google/): The Value of Automation, Use Cases for Automation, and Resolving Inconsistencies Idempotently.

