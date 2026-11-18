# Day 24 - Prepare for on-call

## Session goal

Write a one-page runbook for the local service, then follow it as if you were
new to the system. Identify the information a teammate would need before
supporting a real service.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

Good on-call work depends on preparation, clear ownership, escalation paths,
and manageable workload. A runbook should make routine diagnosis and recovery
clear without assuming the reader already knows the system. It should state
what is known and what still requires judgment. Never invent real contacts or
send messages as part of this solo exercise.

## 2. Practical lab

Write `~/sre-followon-lab/notes/runbook.md` with these sections:

1. **Purpose:** what the local service serves.
2. **Health URL:** `http://127.0.0.1:8000/health.txt` (adjust if you chose
   another port).
3. **Known-good result:** HTTP 200 and body `ok`.
4. **Logs:** the Terminal 1 request log and the relevant local probe CSV.
5. **Safe restart:** stop only the lab server with `Ctrl+C` in its terminal,
   then run the documented `python3 -m http.server` command again.
6. **Rollback:** restore `site/index.html` from `notes/index-v1.html` if the
   content change needs reversal.
7. **Verification:** check the page content and health URL after recovery.
8. **Escalation placeholders:** state what owner/team/contact information
   would be needed; do not invent actual people or contacts.

Ask someone unfamiliar with the lab, or pretend you have never seen it, and
follow only the runbook. Note any missing step. Do not add real contact details.

## 3. Completion check and notes

Can you restore your lab using only the runbook? What information would a
teammate still need before supporting a real service?

- Runbook path:
- Steps followed:
- Missing or ambiguous information:
- Information needed for real on-call:
- Three things learned:
  1.
  2.
  3.

## Done when

- [ ] The runbook includes service purpose, health URL, known-good result, logs, restart, rollback, verification, and escalation placeholders.
- [ ] I walked through it without relying on memory.
- [ ] I listed what is missing before real service ownership.

## Reading

[Google SRE Book - Being On-Call](https://sre.google/sre-book/being-on-call/): Life of an On-Call Engineer, Balanced On-Call, and Feeling Safe.

