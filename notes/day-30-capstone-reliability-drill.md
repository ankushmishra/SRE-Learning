# Day 30 â€” Capstone Reliability Drill

## Learning goals
- Combine measurement, alerting, runbooks, mitigation, and communication.
- Explain the evidence and limits of a small reliability exercise.

## Fundamentals
An operational drill is useful when it exercises a defined objective, produces evidence, and leads to learning. Separate detection, mitigation, and recovery. A short synthetic sample describes only that sample window; it is not monthly availability or a dependable p99 estimate.

## Practical session (localhost only)
1. Start your own local server and capture baseline probes in a fresh CSV.
2. Stop only that server. Detect the failure using your Day 23 check and Day 24 runbook.
3. Record the timeline and incident state; restart your own server and verify health and page content.
4. Complete a short postmortem with one or two follow-up actions.
5. Compare the observed sample with the hypothetical SLO you set on Day 16. Show sample size, window, successes, failures, and calculation; do not claim long-term uptime or a stable percentile.
6. Prepare a five-minute explanation of the drill, evidence, limitations, and one next learning gap.

Never run the drill against a public, shared, production, or employer system. Do not stop unknown processes or use load tools.

## Check your understanding
- How did you detect, mitigate, and verify recovery?
- What does your sample support, and what does it not support?
- What would you improve before a real operational exercise?

## Done when
The full local drill is documented with evidence, an honest SLO comparison, a postmortem, and a stated next learning gap.

## Reading
[Google SRE: Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)
[Google SRE: Emergency Response](https://sre.google/sre-book/emergency-response/)
[Google SRE: Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)

