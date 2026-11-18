# Day 12 - Metrics, monitoring, SLIs, and SLOs

## Session goal

Inspect CPU, memory, and I/O signals on the Ubuntu lab, then define the four
golden signals for the Day 8 Flask app and write one measurable SLI, SLO, and
error budget.

Suggested time: 90-120 minutes.

## 1. Fundamentals

Monitoring collects signals that help operators understand service behavior.
The four golden signals are:

- **Traffic:** demand, such as requests per second.
- **Latency:** time taken to serve requests; separate successful and failed
  requests where useful.
- **Errors:** failed requests or incorrect outcomes.
- **Saturation:** how close a constrained resource is to its useful capacity.

An SLI (service level indicator) is a measured value or proportion, such as the
fraction of valid requests completed within 300 ms. An SLO (service level
objective) is a target over a stated window, such as 99% over 30 days. The
error budget is the allowed portion outside the objective: a 99% request-based
SLO allows 1% of eligible requests to miss the SLI threshold during that
window. It is a request budget, not automatically a downtime duration.

Monitoring helps answer â€œwhat is happening?â€ Logs add event detail; traces show
work across components. Alert on user impact or actionable risk, not every
noisy host metric.

## 2. Practical lab

### 2.1 Read host signals

```bash
top
```

Press `q` to exit. If available, `htop` provides an interactive view. Collect a
short system activity sample:

```bash
vmstat 1 5
```

Observe CPU idle and I/O wait, memory and swap activity, and blocked processes.
The first `vmstat` report often describes averages since boot; focus on later
reports for the short sampling interval.

### 2.2 Define signals for your Flask app

Write a small table for the Day 8 app:

| Signal | Example measure for the app |
| --- | --- |
| Traffic | Requests per minute to the shortener |
| Latency | Request duration, including a percentile or threshold |
| Errors | 5xx responses or failed shorten operations |
| Saturation | CPU, memory, worker capacity, or database connection use |

Choose one SLI and state its numerator, denominator, and measurement window.
Example: â€œEligible requests completed in under 300 ms / all eligible requests,
measured over a rolling 30 days.â€ Then define an SLO such as 99%. State the
error budget as the remaining 1% of eligible requests for that same window.

Do not claim a signal is measured if you only wrote it down. Label which values
you observed and which are proposed instrumentation.

## 3. Review and Day 12 notes

1. What is one example of each golden signal for the app?
2. How does an SLI differ from an SLO?
3. What does a 99% request-based SLO allow?
4. Which CPU, memory, or I/O signal did you observe in `vmstat`?
5. Which signals are currently measured, and which are proposals?

- Observed host signals:
- SLI (numerator / denominator / window):
- SLO target:
- Error budget:
- Three things learned:
  1.
  2.
  3.
- What incident metric did you track previously? What is new vocabulary?

## Done when

- [ ] I observed CPU, memory, and I/O activity with `top`/`htop` and `vmstat`.
- [ ] I described all four golden signals for the Flask app.
- [ ] My notes include one SLI, one SLO target, and its matching error budget.

## Further reading

- [Google SRE Book: Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)
- [Google SRE Book: Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)

