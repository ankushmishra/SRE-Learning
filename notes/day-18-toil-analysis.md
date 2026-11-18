# Day 18 - Find toil before automating

## Session goal

Identify repeated operational tasks in the local lab, distinguish toil from
engineering or other work, and choose one low-risk task that could be
automated.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

Toil is operational work that tends to be manual, repetitive, automatable,
tactical, and without lasting value. A novel task that improves the system may
be engineering work rather than toil. Administrative overhead is also not
automatically toil. Classify work by what it does and how often it recurs, not
only by whether it feels tedious.

Automation can improve consistency, but it is not always the right first step.
The safest improvement may be a clearer runbook, a better design, or removing
the repeated task altogether. Human judgment still matters for unusual cases.

## 2. Practical lab

List five tasks you perform to run or check the localhost service. For each,
record the task, how often it happens, hands-on time, whether a machine could
perform it safely, and its classification: toil, engineering, or other work.

Measure one manual health check with a stopwatch. Choose one low-risk repeated
task that might be automated later; do not automate restarts or destructive
actions today. Save a small toil table in
`~/sre-followon-lab/notes/day18-toil.md`.

| Task | Frequency | Hands-on time | Classification | Reason |
| --- | --- | --- | --- | --- |
| Example: run one local curl probe | daily | 15 sec | candidate toil | repeatable and bounded |

## 3. Completion check and notes

Name one task automation should reduce and one task that still needs human
judgment. Explain the difference. Include your time estimate for the repeated
manual check.

- Candidate task:
- Manual time and frequency:
- Toil table saved at:
- Work that still needs judgment:
- Three things learned:
  1.
  2.
  3.

## Done when

- [ ] I classified five lab tasks and gave a reason for each.
- [ ] I measured the hands-on time of one repeated check.
- [ ] I chose one safe automation candidate and one human-judgment task.

## Reading

[Google SRE Book - Eliminating Toil](https://sre.google/sre-book/eliminating-toil/): Toil Defined, What Qualifies as Engineering, and Is Toil Always Bad?

