# Day 2 - Linux command line and log analysis

## Session goal

Use shell pipelines to inspect a web access log, count HTTP status codes, find
frequently requested URLs, and save server errors to a separate file. This
session uses a synthetic log created in the personal Ubuntu VM; do not use
work or production logs.

Suggested time: about 90 minutes, including the exercises and notes.

## 1. Prepare a workspace

Open Terminal in Ubuntu and run:

```bash
mkdir -p ~/sre-lab/day2
cd ~/sre-lab/day2
pwd
```

The final command should show a path ending in `/sre-lab/day2`. `mkdir -p`
creates the folder if needed and does not complain if it already exists.

## 2. Create a small synthetic web access log

Run this whole block in the Ubuntu terminal. The `EOF` marker must be on a
line by itself:

```bash
cat > access.log <<'EOF'
192.0.2.10 04/Nov/2026 09:00:01 GET / HTTP/1.1 200 512
192.0.2.11 04/Nov/2026 09:00:02 GET /products HTTP/1.1 200 1024
192.0.2.12 04/Nov/2026 09:00:03 GET /products HTTP/1.1 200 1024
192.0.2.13 04/Nov/2026 09:00:04 GET /health HTTP/1.1 200 32
192.0.2.14 04/Nov/2026 09:00:05 GET /login HTTP/1.1 200 256
192.0.2.15 04/Nov/2026 09:00:06 GET /products HTTP/1.1 500 0
192.0.2.16 04/Nov/2026 09:00:07 GET /api/items HTTP/1.1 503 0
192.0.2.17 04/Nov/2026 09:00:08 GET / HTTP/1.1 200 512
192.0.2.18 04/Nov/2026 09:00:09 GET /search?q=linux HTTP/1.1 404 128
192.0.2.19 04/Nov/2026 09:00:10 GET /products HTTP/1.1 200 1024
192.0.2.20 04/Nov/2026 09:00:11 GET /api/items HTTP/1.1 200 768
192.0.2.21 04/Nov/2026 09:00:12 GET /checkout HTTP/1.1 502 0
192.0.2.22 04/Nov/2026 09:00:13 GET /static/app.css HTTP/1.1 304 0
192.0.2.23 04/Nov/2026 09:00:14 GET /login HTTP/1.1 401 64
192.0.2.24 04/Nov/2026 09:00:15 GET /health HTTP/1.1 200 32
EOF
```

Confirm the file exists and inspect a few lines:

```bash
ls -l access.log
head -n 3 access.log
wc -l access.log
```

There should be 15 lines. Each line contains space-separated fields: client
IP, date, time, method, path, protocol, status code, and response size. This
deliberately simplified format makes the fields easy to inspect with `awk`;
real web-server log formats vary.

## 3. Understand the pipeline tools

- `grep` filters lines matching a pattern.
- `awk` reads fields; in this log `$5` is the request path, `$7` is the HTTP
  status code, and `$8` is the response size.
- `sort` groups identical values together when used before `uniq`.
- `uniq -c` counts adjacent identical lines.
- `wc -l` counts lines.
- `>` writes command output to a file, replacing its previous contents.
- `|` sends one command's output to the next command.

Inspect the fields before combining commands:

```bash
awk '{print $5, $7}' access.log | head
```

The output should show a path followed by its status code for each displayed
request.

## 4. Count requests by HTTP status code

```bash
awk '{print $7}' access.log | sort | uniq -c | sort -nr
```

Expected counts: 9 responses with status `200`, and 1 each with `304`, `401`,
`404`, `500`, `502`, and `503`. Tied rows may appear in either order.

Read the pipeline from left to right: extract the status field, group matching
codes, count each group, then sort by count with the largest first.

## 5. Find the five most-requested URLs

```bash
awk '{print $5}' access.log | sort | uniq -c | sort -nr | head -n 5
```

`/products` appears 4 times. `/`, `/api/items`, `/health`, and `/login` each
appear 2 times. Because of the tie, the order among those four paths can vary.
`head -n 5` displays five rows, not necessarily five unique counts.

## 6. Save all server-error responses

HTTP status codes from 500 through 599 indicate server errors. Extract those
complete log lines into `errors.txt`:

```bash
awk '$7 >= 500 && $7 < 600' access.log > errors.txt
```

Verify the result:

```bash
cat errors.txt
wc -l errors.txt
```

There should be 3 lines: statuses `500`, `502`, and `503`. `cat` is suitable
for this tiny file; for a large log, inspect only a small portion with `head`
or `less`.

## 7. Search and inspect the log

Find every request that returned a server error:

```bash
grep -E ' 5[0-9]{2} ' access.log
```

Compare that output with `errors.txt`. `grep` is useful for quick text
searches; `awk` is more useful when you need to test or print specific fields.

Inspect the newest lines in a log file:

```bash
tail -n 5 access.log
```

For a live log, `tail -f filename` follows new lines as they are written. Press
`Ctrl+C` to stop it. The sample file does not change, so use `tail -n` here.

## 8. More Ubuntu commands practiced

Open the log in a scrollable viewer with line numbers:

```bash
less -N access.log
```

Use the arrow keys to scroll, type `/500` and press Enter to search for `500`,
then press `q` to quit. `less` is useful for large files because it does not
print the entire file at once.

Count lines containing the exact status `500`:

```bash
grep -c ' 500 ' access.log
```

`grep -c` counts matching lines. The spaces around `500` help avoid matching
other numbers. The result should be `1`.

Show matching lines with their line numbers:

```bash
grep -n ' 500 ' access.log
```

`-n` adds the line number to each result. The sample's `500` response is on
line 6.

Show all lines that do not contain status `200`:

```bash
grep -v ' 200 ' access.log
```

`-v` reverses the match and shows non-matching lines. This includes statuses
such as `304`, `401`, and `404`, not just server errors.

Count requests from each client IP:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

`awk` prints field 1 (the client IP); `sort` groups identical IPs;
`uniq -c` counts each group; and `sort -nr` orders the counts from largest to
smallest. In this sample, each IP appears once.

## 9. Review and explain

Run the analyses again without looking at the examples. Then explain in your
own words:

1. What does each `|` do in the status-count pipeline?
2. Why does `sort` come before `uniq -c`?
3. Which field contains the path, and which contains the status?
4. Why do we filter `500-599` instead of searching for the word `error`?
5. What does `>` do if `errors.txt` already exists?

## Optional stretch

Find the total number of response bytes for each status code:

```bash
awk '{bytes[$7] += $8} END {for (code in bytes) print code, bytes[code]}' access.log | sort
```

Here `$8` is the response-size field in this simplified log. A production log
may use a different format, so first inspect its format before assuming field
positions.

## Day 2 notes

Record three things learned, one command that was confusing, and how you
resolved it:

1.
2.
3.

Confusing command / resolution:
