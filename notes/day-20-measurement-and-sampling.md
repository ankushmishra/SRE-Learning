# Day 20 - Collect and interpret measurements

## Session goal

Collect a small, labeled probe sample, calculate its success rate, inspect
individual latency values, and explain why the result is not a monthly
availability or tail-latency claim.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

An average can hide slow or failed requests. The sampling frequency, number of
observations, and measurement window affect what a result can support. A
percentile such as p99 needs enough representative observations; a dozen
measurements cannot establish a stable p99. Always keep the raw observations,
sample size, and start/end time with a summary.

For the local exercise, success means HTTP 200. Each probe counts once. This
simple binary success rate is not the same as continuous service availability.

## 2. Practical lab

Use a fresh CSV so this sample is distinct from earlier work. Choose a new
filename if `probes-day20.csv` already exists:

```bash
cd ~/sre-followon-lab
: > probes-day20.csv
```

Take 10 probes while the server is running, each in a separate command or
supervised run of the bounded probe from the overview. Then stop the server
with `Ctrl+C` in Terminal 1, take 2 failed probes, and restart the same server
command. Do not run an unattended loop.

Inspect every row. Count the successful `200` results and unsuccessful
responses, calculate the rate, and note the start/end time. If all probes behave
as planned, the sample is 10/12 = 83.33% success. Save the raw CSV and a short
summary under `~/sre-followon-lab/notes/day20-measurement.md`.

## 3. Completion check and notes

Explain why 10/12 does not establish monthly availability or stable p99 latency.
What sample size and time window did you actually observe?

- Start/end time:
- Sample size:
- Successful / unsuccessful:
- Success rate:
- Latency observations:
- What this sample cannot establish:

## Done when

- [ ] I saved a fresh sample with 10 successful and 2 failed probes, if the lab behaved as planned.
- [ ] I calculated the success rate from the actual rows.
- [ ] I recorded the sample size and measurement window.
- [ ] I can explain why this does not prove a monthly figure or stable p99.

## Reading

[Google SRE Book - Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/): Collecting Ideas from The Four Golden Signals, Worrying About Your Tail, and Choosing an Appropriate Resolution for Measurements.

