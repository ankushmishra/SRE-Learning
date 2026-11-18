# Day 23 - Alert on something actionable

## Session goal

Define a supervised lab alert rule for repeated health-check failures. Record
detection delay, recovery signal, and the runbook step to take. Do not connect
the lab to a real pager or notification system.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

An alert should indicate a meaningful symptom, have a clear owner, and point to
a useful response. Black-box checks show whether the service appears usable
from the checker's point of view. A single noisy sample can be a weak signal;
thresholds and spacing trade detection speed against false alarms. A lab rule
is an exercise choice, not a universal production threshold.

## 2. Practical lab

Write the following rule in your notes:

> If three consecutive localhost health probes fail, with 10 seconds between
> probes, take a manual look at service status and follow the runbook.

Run this sequence only while watching the lab. Stop your local server with
`Ctrl+C` in Terminal 1, then in Terminal 2 run one probe at a time, waiting
about 10 seconds between checks. Use the Day 21 script or the bounded curl
command. Do not run an unattended loop.

Record the time of the first failure, the third failure (detection), the action
taken, and the first successful probe after recovery. No real paging
integration is needed. Estimate how the 10-second spacing and 3-failure
threshold affect detection delay and the chance of reacting to a transient
failure. Restart the lab service after the short drill.

## 3. Completion check and notes

Explain how your spacing and threshold affect delay and false alarms. State
clearly that this threshold is specific to this lab.

- Rule:
- First failure time:
- Detection time:
- Recovery signal/time:
- Runbook response:
- What this lab rule does not prove:

## Done when

- [ ] The rule uses three consecutive failures at 10-second spacing.
- [ ] I ran only a short, supervised sequence against localhost.
- [ ] I recorded detection and recovery evidence.
- [ ] I did not connect a real paging integration.

## Reading

[Google SRE Book - Practical Alerting](https://sre.google/sre-book/practical-alerting/): Black-Box Monitoring, Alerting, and Symptoms Versus Causes.

