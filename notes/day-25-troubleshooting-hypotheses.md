# Day 25 - Troubleshoot with hypotheses

## Session goal

Compare a missing-page response with a stopped-service connection failure.
Record symptoms, hypotheses, tests, evidence, and fixes, restoring normal
operation between cases.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

Troubleshooting starts with observations and a testable hypothesis. Change one
thing at a time and use evidence that could confirm or disprove the hypothesis.
A negative test result is useful information. A 404 means an HTTP server
responded but did not find the requested path. A connection failure means the
client did not receive an HTTP response from the service endpoint. Restarting
the server does not fix an incorrect URL path.

## 2. Practical lab

Keep the service running. First request a path that does not exist:

```bash
curl -sS --max-time 2 -o /dev/null -w '%{http_code}\n' \
  http://127.0.0.1:8000/missing.txt
```

Expected: `404`. Record the symptom and test the hypothesis that the path is
wrong by checking the served files. The fix is to request the correct path.

Restore normal operation by requesting `/health.txt`, then stop only the local
server with `Ctrl+C` in Terminal 1. Probe again:

```bash
curl -sS --max-time 2 -o /dev/null -w '%{http_code}\n' \
  http://127.0.0.1:8000/health.txt
```

The status may be `000` with a curl connection error. Record the stopped server
as the hypothesis and the service restart as the controlled recovery test. Run
the same server command again and verify HTTP 200. Do not test any other host.

For each case, save symptom, hypothesis, test, result, and fix under
`~/sre-followon-lab/notes/day25-troubleshooting.md`.

## 3. Completion check and notes

Distinguish the 404 from a connection failure. Why is restarting the server not
the right fix for a missing URL path?

- Missing-path symptom / hypothesis / test / fix:
- Stopped-server symptom / hypothesis / test / fix:
- Recovery evidence:
- Three things learned:
  1.
  2.
  3.

## Done when

- [ ] I documented both fault cases and their different symptoms.
- [ ] I restored normal operation between the cases.
- [ ] I can explain why a 404 is not fixed by restarting the server.

## Reading

[Google SRE Book - Effective Troubleshooting](https://sre.google/sre-book/effective-troubleshooting/): Problem Report, Triage, Examine, Diagnose, and Test and Treat.

