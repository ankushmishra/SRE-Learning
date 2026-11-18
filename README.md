# SRE Learning

This repository is my technical learning notes for SRE (Site Reliability Engineering) from scratch.

## Goal
Build practical understanding of Linux, networking, scripting, databases, monitoring, troubleshooting, containers, and cloud systems.

## Core technical areas
- Linux fundamentals and troubleshooting
- Git and version control
- Networking: DNS, TCP/UDP, HTTP, routing
- Python scripting and automation
- Databases: SQL, indexes, performance basics
- Systems design and reliability
- Monitoring and observability: metrics, logs, traces, alerts
- Containers and Kubernetes basics
- Cloud concepts and infrastructure automation

## Daily learning plan
### Week 1: Linux, Git, Networking
- Day 1: SRE role + Linux basics
- Day 2: Command line + log parsing
- Day 3: Linux services + permissions + logs
- Day 4: Linux processes, networking, and basic monitoring
- Day 5: DNS and UDP/TCP
- Day 6: HTTP, routing, recap

### Week 2: Programming, data, monitoring, and system thinking
- Day 7: Python concepts and log-analysis automation
- Day 8: Python and Flask web apps
- Day 9: Relational databases and MySQL
- Day 10: NoSQL and queues
- Day 11: Systems design
- Day 12: Monitoring and observability

### Week 3: Security, containers, and failure practice
- Day 13: Security fundamentals
- Day 14: Containers and Docker
- Day 15: Kubernetes, failure drill, and postmortem

## Technical notes

### Study notes
- [Notes index](./notes/README.md)
- [Day 1 - Linux fundamentals](./notes/day-01-linux-foundations.md)
- [Day 2 - Linux command line and log analysis](./notes/day-02-linux-command-line.md)
- [Day 3 - Linux services, permissions, and logs](./notes/day-03-linux-services-permissions-logs.md)
- [Day 4 - Linux processes, networking, and basic monitoring](./notes/day-04-linux-processes-networking-monitoring.md)
- [Day 5 - Networking I: DNS, UDP, and TCP](./notes/day-05-networking-dns-udp-tcp.md)
- [Day 6 - Networking II: HTTP, routing, and weekly recap](./notes/day-06-networking-http-routing.md)
- [Day 7 - Python I: concepts and log analysis](./notes/day-07-python-log-analysis.md)
- [Day 8 - Python II: web apps with Flask](./notes/day-08-python-flask-web-apps.md)
- [Day 9 - Relational databases and MySQL](./notes/day-09-relational-databases-mysql.md)
- [Day 10 - NoSQL, message queues, and big data concepts](./notes/day-10-nosql-queues-big-data.md)
- [Day 11 - Systems design](./notes/day-11-systems-design.md)
- [Day 12 - Metrics, monitoring, SLIs, and SLOs](./notes/day-12-metrics-monitoring-sli-slo.md)
- [Day 13 - Security fundamentals](./notes/day-13-security-fundamentals.md)
- [Day 14 - Containers and Docker](./notes/day-14-containers-docker.md)
- [Day 15 - Kubernetes intro, failure drill, and postmortem](./notes/day-15-kubernetes-failure-drill-postmortem.md)
- [Integrated reference - Flask, MySQL, Docker, Kubernetes, and postmortems](./notes/reference-flask-mysql-docker-kubernetes-postmortem.md)
- [Days 16â€“30 overview and shared local lab](./notes/day-16-to-30-follow-on-overview.md)
- [Day 16 - SLOs and user reliability](./notes/day-16-slos-and-user-reliability.md)
- [Day 17 - Risk and error budgets](./notes/day-17-risk-error-budgets.md)
- [Day 18 - Toil analysis](./notes/day-18-toil-analysis.md)
- [Day 19 - Monitoring design](./notes/day-19-monitoring-design.md)
- [Day 20 - Measurement and sampling](./notes/day-20-measurement-and-sampling.md)
- [Day 21 - Safe probe automation](./notes/day-21-safe-probe-automation.md)
- [Day 22 - Release and rollback](./notes/day-22-release-and-rollback.md)
- [Day 23 - Actionable alerting](./notes/day-23-actionable-alerting.md)
- [Day 24 - On-call runbook](./notes/day-24-oncall-runbook.md)
- [Day 25 - Troubleshooting hypotheses](./notes/day-25-troubleshooting-hypotheses.md)
- [Day 26 - Emergency response](./notes/day-26-emergency-response.md)
- [Day 27 - Incident coordination](./notes/day-27-incident-coordination.md)
- [Day 28 - Blameless postmortem](./notes/day-28-blameless-postmortem.md)
- [Day 29 - Reliability test checklist](./notes/day-29-reliability-test-checklist.md)
- [Day 30 - Capstone reliability drill](./notes/day-30-capstone-reliability-drill.md)

### 1. Linux
- Understand file system, permissions, process management, logs, and services.
- Use `ls`, `ps`, `top`, `systemctl`, `journalctl`, `tail -f`, `grep`, and `awk` confidently.
- Learn how to diagnose CPU, memory, I/O, and service-level issues.

### 2. Networking
- Understand DNS, IP, ports, TCP handshake, HTTP flows, and routing.
- Practice with `ping`, `nslookup`, `dig`, `curl -v`, `traceroute`, and `ss`.
- Learn how latency and errors appear at different layers.

### 3. Git and collaboration
- Use branches, commits, pull requests, merge conflicts, and release flow.
- Treat configuration and code changes as versioned, reviewable, and reversible artifacts.

### 4. Python and automation
- Write scripts to parse logs, automate repetitive checks, and reduce toil.
- Use Python for monitoring tasks, file processing, and simple service checks.

### 5. Databases and data access
- Learn relational data models, indexes, joins, and query plans.
- Understand when SQL fits, when NoSQL helps, and when queues decouple systems.

### 6. SRE mindset
- Focus on reliability, availability, scalability, and fast recovery.
- Measure with metrics, logs, and traces.
- Build alerting around user impact, not just infrastructure noise.

## Useful commands
```bash
pwd
ls -la
ps aux
top
systemctl status nginx
journalctl -xe
tail -f /var/log/syslog
curl -v https://example.com
dig example.com
ss -tunlp
python3 --version
git status
git log --oneline
```

## Next steps
1. Set up WSL2 with Ubuntu for a safe lab environment.
2. Practice Linux and networking commands every day.
3. Create a small project with logs and scripts.
4. Add monitoring and observe response patterns.
5. Containerize a simple app and run it locally.

This repository will grow with notes, scripts, and hands-on exercises as the learning progresses.

