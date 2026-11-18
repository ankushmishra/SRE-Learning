# SRE Follow-On Plan for Days 16 to 30

This note carries the Word plan into the existing SRE Learning notes. It sets
the shared lab, daily rhythm, safety limits, and evidence to keep. Each daily
note expands one day's reading, fundamentals, exercise, and completion check.

## Daily rhythm

Plan about two hours per study day:

1. Read only the selected Google SRE book sections for 30 minutes.
2. Write the fundamentals in your own words for 15 minutes before the lab.
3. Do the local exercise and save evidence for 60 minutes.
4. Answer the completion check without the book for 15 minutes.

If a check is unclear, repeat that session before moving on. These are study
days, not calendar dates.

## Shared lab setup

Use Ubuntu/WSL and keep this lab under the Linux home folder, not `/mnt/c`. Use
a new, empty folder. If `~/sre-followon-lab` already exists, keep its contents
and choose another folder name throughout the exercises. An existing app can
replace the sample service if it has a local health URL.

Before setup, check that `python3` and `curl` are available:

```bash
python3 --version
curl --version
```

If either is missing, pause and get setup help rather than running an unknown
installer. Create a tiny site:

```bash
mkdir -p ~/sre-followon-lab/site ~/sre-followon-lab/notes
cd ~/sre-followon-lab
printf 'ok\n' > site/health.txt
printf 'version 1\n' > site/index.html
```

In Terminal 1, start a localhost-only server and leave it running:

```bash
cd ~/sre-followon-lab
python3 -m http.server 8000 --bind 127.0.0.1 --directory site
```

If port 8000 is already in use, do not stop an unknown process. Pick another
unused port and change it in every command. In Terminal 2, take one probe:

```bash
cd ~/sre-followon-lab
curl -sS --max-time 2 -o /dev/null \
  -w '%{http_code},%{time_total}\n' \
  http://127.0.0.1:8000/health.txt >> probes.csv
tail -n 1 probes.csv
```

A normal probe should return status `200`. A missing path returns `404`. A
connection failure may record `000` along with a curl error. To simulate an
outage, press `Ctrl+C` only in Terminal 1, probe from Terminal 2, then restart
the same server command and verify a `200` response.

## Measurement and safety rules

For this lab, only HTTP 200 counts as a successful probe:

```text
success rate = successful probes / all probes * 100
```

Always record the sample size and start/end of the measurement window. These
synthetic probes are not real customer traffic or continuous uptime data. A
small sample cannot establish a monthly reliability figure or a meaningful
tail-latency percentile.

Test only this lab or an existing local app. Do not probe an employer system,
public URL, or shared service. Avoid stress tools, large request loops, and
unattended failure injection. Stop your own server with `Ctrl+C` when finished.
No cloud account, paid tools, Kubernetes, or production access is required.

Save daily notes, probe results, and check answers under
`~/sre-followon-lab/notes`. The plan aims to produce an SLO sheet, probe log,
safe-check script, release checklist, runbook, incident timeline, postmortem,
and test checklist. This is foundational practice, not a certification or a
promise of job readiness.

## Daily notes

### Days 16-20: targets, risk, toil, and monitoring

- [Day 16 - Define reliability from the user view](./day-16-slos-and-user-reliability.md)
- [Day 17 - Risk and error budgets](./day-17-risk-error-budgets.md)
- [Day 18 - Find toil before automating](./day-18-toil-analysis.md)
- [Day 19 - Design useful monitoring](./day-19-monitoring-design.md)
- [Day 20 - Collect and interpret measurements](./day-20-measurement-and-sampling.md)

### Days 21-25: automation, releases, alerts, on-call, and troubleshooting

- [Day 21 - Automate a safe check](./day-21-safe-probe-automation.md)
- [Day 22 - Make a change reversible](./day-22-release-and-rollback.md)
- [Day 23 - Alert on something actionable](./day-23-actionable-alerting.md)
- [Day 24 - Prepare for on-call](./day-24-oncall-runbook.md)
- [Day 25 - Troubleshoot with hypotheses](./day-25-troubleshooting-hypotheses.md)

### Days 26-30: recovery, incidents, postmortems, and testing

- [Day 26 - Restore service before deep analysis](./day-26-emergency-response-recovery.md)
- [Day 27 - Coordinate an incident](./day-27-incident-coordination.md)
- [Day 28 - Learn with a blameless postmortem](./day-28-blameless-postmortem.md)
- [Day 29 - Test reliability changes safely](./day-29-reliability-testing.md)
- [Day 30 - Run the end-to-end reliability drill](./day-30-end-to-end-reliability-drill.md)

