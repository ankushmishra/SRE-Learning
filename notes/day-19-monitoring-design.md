# Day 19 - Design useful monitoring

## Session goal

Design a simple monitoring sheet around user-visible behavior, internal
signals, and the four golden signals. Identify a blind spot in the local lab.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

Black-box monitoring checks the service from the outside, as a user would.
White-box monitoring uses internal information such as logs, metrics, or
application state. They answer different questions and are useful together.

The four golden signals are latency, traffic, errors, and saturation. A slow
response is a symptom; it does not prove CPU is the cause. A user-facing probe
can show that a request was slow or failed, while host measurements may help
investigate why. Monitoring should make the important service behavior visible
without pretending to measure things the lab does not collect.

## 2. Practical lab

Draw a table with one row per golden signal. Map local evidence to each:

| Signal | Possible lab evidence | Limitation |
| --- | --- | --- |
| Latency | curl elapsed seconds | one client and small sample |
| Traffic | count of probes | not real user demand |
| Errors | HTTP status or curl failure | only one URL is checked |
| Saturation | candidate CPU or memory readings | current service does not expose resource telemetry |

Use curl status/timing, the Python server's request log, and candidate host
measurements such as `top` or `free -h`. Write down which are observed today
and which would require additional instrumentation. Save the design to
`~/sre-followon-lab/notes/day19-monitoring.md`.

## 3. Completion check and notes

Explain why a slow response is a symptom rather than proof of a CPU problem.
Name one monitoring blind spot and the extra measurement that could help.

- Black-box signal:
- White-box signal:
- Golden signal and evidence:
- Blind spot and possible measurement:
- Three things learned:
  1.
  2.
  3.

## Done when

- [ ] My monitoring sheet covers all four golden signals.
- [ ] I separated observations from proposed measurements.
- [ ] I can explain one blind spot and why a symptom is not a root cause.

## Reading

[Google SRE Book - Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/): Why Monitor, Symptoms Versus Causes, Black-Box Versus White-Box, and The Four Golden Signals.

