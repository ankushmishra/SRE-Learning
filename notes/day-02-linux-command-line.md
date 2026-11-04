# Day 2 - Linux command line and log analysis

## Objective
Gain confidence using command line tools for filtering, counting, summarizing, and extracting meaningful data from logs.

## Commands to practice
```bash
grep -i "error" /var/log/syslog | head
grep -E "404|500" access.log | head
awk '{print $9}' access.log | sort | uniq -c | sort -nr | head
cut -d ' ' -f 9 access.log | sort | uniq -c
wc -l access.log
cat access.log | grep 500 > errors.txt
```

## What SREs do with logs
- Find failures and error spikes.
- Count responses by status code.
- Identify top requests and high-traffic endpoints.
- Separate noise from real user impact.

## Typical analysis tasks
1. Count requests by HTTP status code.
2. Find the 5 most requested URLs.
3. Save all 5xx requests to a file.

## Example thought process
- `grep` filters matching lines.
- `sort` organizes data.
- `uniq -c` counts repeated values.
- `wc -l` counts lines.
- Redirection (`>`, `>>`) stores output for later analysis.

## Exercise
Create or download a sample access log, then answer:
- How many requests were 200?
- Which URL was most requested?
- How many 5xx responses were present?
- What was saved to `errors.txt`?

## Notes
Write 3 things you learned in your own words:
1.
2.
3.
