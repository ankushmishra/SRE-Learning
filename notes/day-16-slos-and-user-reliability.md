# Day 16 - Define reliability from the user view

## Session goal

Define a user-relevant service level indicator (SLI), set a learning SLO, and
separate an internal objective from an external agreement.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

An SLI is a quantitative measure of service behavior. An SLO sets a target for
an SLI over a stated window. An SLA is an agreement with defined consequences
or commitments; it is not another name for an internal SLO. A useful SLI
measures something users care about, such as successful requests or latency.

The local `/health.txt` probe is only a proxy for a user experience. It checks
one path from one machine, not the full application, its dependencies, or users'
network paths. State the denominator, success rule, and window whenever you
report a percentage.

## 2. Practical lab

Use the shared local service in the [Day 16-30 overview](./day-16-to-30-follow-on-overview.md).
Take a few manual probes and draft an SLO sheet. Use this learning example:

- SLI: fraction of probes that return HTTP 200.
- Window: a hypothetical set of 1,000 requests.
- Example target: 99% successful probes.
- Candidate latency SLI: response time under an explicitly chosen threshold.

For 1,000 requests, a 99% success target allows 10 unsuccessful requests. Do
not present this tiny local sample as a production objective or promise.

Save the sheet under `~/sre-followon-lab/notes/day16-slo.md` with the target,
measurement window, numerator, denominator, and known limitations.

## 3. Completion check and notes

Without looking back, explain SLI, SLO, and SLA using your local service. Why
is the localhost probe only a proxy for user experience?

- SLI and success rule:
- Window and denominator:
- Example SLO target:
- Candidate latency SLI:
- Three things learned:
  1.
  2.
  3.
- What the local probe cannot measure:

## Done when

- [ ] My SLO sheet defines the signal, target, formula, and window.
- [ ] I can distinguish an SLI, SLO, and SLA.
- [ ] I can explain the limits of the localhost probe.

## Reading

[Google SRE Book - Service Level Objectives](https://sre.google/sre-book/service-level-objectives/): Service Level Terminology, Indicators in Practice, and Defining Objectives.

