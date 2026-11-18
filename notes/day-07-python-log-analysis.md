# Day 7 - Python I: concepts and log analysis

## Session goal

Use Python to automate the Day 2 status-code analysis. Read a text log, validate
its fields, count status codes, and optionally print server-error lines. This
is a small example of reducing repetitive operational work (toil).

Suggested time: 90-120 minutes.

## 1. Fundamentals

- A variable refers to a value; a list holds ordered values; a dictionary maps
  keys to values.
- A loop processes records one at a time. A conditional selects records such
  as HTTP 5xx responses.
- A function groups reusable logic. Exceptions let a program handle expected
  problems, such as a missing file or malformed line.
- `with open(...)` closes a file reliably. `Counter` counts repeated values.
- A command-line argument lets a script work on different input files without
  editing its source code.

The Day 2 sample log is deliberately space-separated. Real access logs can use
quoted fields or different formats; inspect the format before assuming column
positions.

## 2. Practical lab

### 2.1 Prepare the lab

```bash
mkdir -p ~/sre-lab/day7
cd ~/sre-lab/day7
```

If your Day 2 sample still exists, copy it:

```bash
cp ~/sre-lab/day2/access.log .
```

If it does not, recreate the synthetic `access.log` by following the Day 2
note. Do not use work or production logs.

### 2.2 Create `logreport.py`

```python
#!/usr/bin/env python3
import argparse
from collections import Counter
from pathlib import Path


def main():
    parser = argparse.ArgumentParser(description="Count HTTP statuses in a sample log")
    parser.add_argument("logfile", type=Path, help="space-separated sample access log")
    parser.add_argument("--errors-only", action="store_true",
                        help="print complete lines with HTTP status 500-599")
    args = parser.parse_args()

    counts = Counter()
    errors = []
    try:
        with args.logfile.open(encoding="utf-8") as log:
            for line_number, line in enumerate(log, start=1):
                fields = line.split()
                if len(fields) < 8:
                    print(f"Skipping malformed line {line_number}")
                    continue
                try:
                    status = int(fields[6])
                except ValueError:
                    print(f"Skipping invalid status on line {line_number}")
                    continue
                counts[status] += 1
                if 500 <= status < 600:
                    errors.append(line.rstrip())
    except OSError as error:
        parser.error(f"cannot read {args.logfile}: {error}")

    if args.errors_only:
        print("\n".join(errors) if errors else "No server errors found.")
    else:
        print("Status counts:")
        for status, count in sorted(counts.items()):
            print(f"{status}: {count}")
        print(f"Total valid requests: {sum(counts.values())}")


if __name__ == "__main__":
    main()
```

Run it:

```bash
python3 logreport.py access.log
python3 logreport.py access.log --errors-only
```

For the Day 2 synthetic sample, expect nine 200s and one each of 304, 401,
404, 500, 502, and 503. The errors-only mode should print three lines.

### 2.3 Commit the script to your learning repository

From the root of your SRE Learning repository, copy the script into a tracked
scripts folder and review exactly what you will commit:

```bash
mkdir -p scripts
cp ~/sre-lab/day7/logreport.py scripts/logreport.py
git status --short
git add scripts/logreport.py
git diff --cached
git commit -m "Add Day 7 log report script"
git push
```

Push only to your own learning repository. Do not commit credentials, private
logs, or unrelated files. If Git asks you to configure authentication, follow
your normal GitHub setup; never paste a token into the notes or commit it.

## 3. Review and Day 7 notes

1. Why does the script use `fields[6]` for status?
2. What does `Counter` do?
3. How does the script handle a malformed line?
4. What repetitive task did this automation remove?

- Command used and output summary:
- Number of malformed lines skipped:
- Git commit and push result:
- Three things learned:
  1.
  2.
  3.
- Confusing command / resolution:

## Done when

- [ ] `python3 logreport.py access.log` prints correct counts.
- [ ] `--errors-only` prints only 5xx lines.
- [ ] I can explain the parsing and error handling.
- [ ] I reviewed, committed, and pushed the script to my learning repository.

## Further reading

- [Python tutorial: input and output](https://docs.python.org/3/tutorial/inputoutput.html)
- [Python `argparse` tutorial](https://docs.python.org/3/howto/argparse.html)

