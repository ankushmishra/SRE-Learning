# Day 15 - Kubernetes intro, failure drill, and postmortem

## Session goal

Learn what orchestration adds beyond running one container, perform a controlled
failure drill on the local Flask lab, detect and diagnose it, restore service,
and write a blameless postmortem with a timeline and follow-up actions.

Suggested time: 120 minutes or more.

## 1. Fundamentals

Kubernetes manages containerized workloads through a cluster control plane and
worker nodes. A Deployment describes the desired application replicas; the
system works to keep the actual state near the declared state. A Service gives
clients a stable way to reach a group of Pods. Kubernetes adds scheduling,
health management, service discovery, and rollout capabilities; it does not
remove the need to design the application, data layer, security, or monitoring.

A failure drill tests whether detection and recovery work as expected. A
postmortem records impact, timeline, contributing causes, response, and
preventive actions. Blameless means examining system conditions and information
available at the time, not assigning personal fault.

## 2. Practical lab

Run this only against your local Day 8/14 Flask app or disposable container.
Never stop a work or production service. Record the baseline and recovery
commands before introducing the failure.

### 2.1 Baseline

Start the stopped local container left from Day 14, then record a successful
request:

```bash
docker start sre-flask-day14
curl -i http://127.0.0.1:5000/health
docker ps
```

Use `/` instead of `/health` if the app has no health route. Note the time and
response status. Keep the container name used on Day 14.

### 2.2 Introduce and detect one safe failure

Stop only the named local learning container:

```bash
docker stop sre-flask-day14
```

Try the local request again:

```bash
curl --max-time 3 -i http://127.0.0.1:5000/health
docker ps -a
```

Record the symptom, when you noticed it, and which signal detected it. A
connection refusal or timeout is expected when the local container is stopped.

### 2.3 Diagnose and recover

Inspect the container state and its last logs:

```bash
docker inspect -f '{{.State.Status}}' sre-flask-day14
docker logs --tail 50 sre-flask-day14
```

Restart the same lab container, then verify recovery:

```bash
docker start sre-flask-day14
curl -i http://127.0.0.1:5000/health
```

If the Day 14 container was removed, recreate it from the `sre-flask-day14`
image with the same local port mapping, then repeat the drill.

### 2.4 Write a one-page blameless postmortem

Use this outline:

1. **Summary and impact:** what users of this lab experienced.
2. **Timeline:** baseline, failure introduced, detection, diagnosis, recovery.
3. **Root cause and contributing conditions:** for this drill, the local
   container was intentionally stopped; note what made the symptom visible or
   delayed detection.
4. **What went well / what was confusing:** focus on tools and process.
5. **Follow-up actions:** two specific actions with an owner or completion
   condition, such as adding a health check or documenting a recovery command.

Do not write a blame-focused account. Describe how the system behaved and what
would make the next response faster or safer.

### 2.5 Kubernetes stretch

If you already have a local Kubernetes environment such as kind or minikube,
review a Deployment and Service for the app and scale to two replicas. Do not
install or expose a cluster merely for this optional step. Record what the
orchestrator manages that a single `docker run` does not.

## 3. Review and Day 15 notes

1. What does Kubernetes add beyond running a container directly?
2. What detected the stopped app? What did the logs show?
3. What made detection slower than desired?
4. Which two follow-up actions could improve detection or recovery?
5. Write three strengths, three gaps, and a next 30-day learning plan.

- Baseline status/time:
- Failure status/time:
- Detection signal and delay:
- Root cause / contributing conditions:
- Recovery command and result:
- Follow-up actions:
  1.
  2.
- Three strengths:
  1.
  2.
  3.
- Three gaps:
  1.
  2.
  3.
- Next 30 days:

## Done when

- [ ] I completed the failure drill only on my local lab app.
- [ ] I detected the issue, checked logs/state, and restored service.
- [ ] I wrote a timeline, root cause, detection gap, and two follow-up actions.
- [ ] I can explain what Kubernetes adds beyond Docker alone.
- [ ] I wrote three strengths, three gaps, and a next 30-day learning plan.

## Further reading

- [Kubernetes overview](https://kubernetes.io/docs/concepts/overview/)
- [Google SRE: Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)

