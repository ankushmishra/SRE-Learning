# Reference guide - From Flask app to reliable service

This note connects five references into one learning path: a Flask application,
a MySQL query plan, a Docker image, Kubernetes workload management, and a
blameless SRE postmortem. It builds on the Flask, MySQL, Docker, monitoring,
and failure-drill exercises from Days 8-15.

Suggested time: 2-3 hours, split across sessions. Use a personal local lab.

## 1. Fundamentals: how the pieces fit

```text
Client -> HTTP request -> Flask route -> MySQL query
                              |
                         Docker image
                              |
                  Kubernetes Deployment + Service
                              |
       metrics, logs, incident response, postmortem
```

### Flask: application behavior

Flask maps a URL and HTTP method to a Python function. The route function
validates input, calls application logic, and returns a response with a status
and body. A small `/health` route can help a local learner check that the app
answers. Flask's built-in server and interactive debugger are for development;
do not expose debug mode to a network or use the development server as a
production deployment.

### MySQL: durable relational data

MySQL stores structured records and relationships. The application should use
parameterized queries and an appropriately privileged database account. `EXPLAIN`
shows the optimizer's plan for a query: which tables are read, possible and
chosen keys, access method, and estimated rows, depending on output format.
An index can make some lookups faster but costs storage and adds work to writes.
The optimizer may correctly choose a table scan for a small table. Read the
plan and measure; do not add indexes just to make `EXPLAIN` show one.

### Docker: repeatable application packaging

A Dockerfile tells the builder how to make an image. Common instructions
include `FROM`, `WORKDIR`, `COPY`, `RUN`, `USER`, and `CMD`. The image is a
build artifact; a container is a running instance. Keep secrets out of the
image, use a non-root runtime user, and pin dependencies and base images when
repeatable builds matter. A local Flask development server is fine for this
learning lab only; production deployments need an appropriate WSGI server and
deployment configuration.

### Kubernetes: desired state for workloads

Kubernetes uses declarative objects and controllers to move actual state toward
the requested state. A Deployment manages a set of application Pods; a Service
provides a stable network endpoint for selected Pods. Readiness checks control
whether a Pod should receive traffic; liveness checks can trigger a restart.
Kubernetes does not include MySQL as a built-in database service and does not
choose your logging or monitoring system. Durable database storage, backups,
credentials, and recovery still need explicit design.

### Postmortem: learning from an incident

A postmortem documents what happened, its impact, the response, contributing
causes, and follow-up work. Blameless writing focuses on system conditions and
the information available at the time, rather than accusing a person. Action
items should reduce the chance or impact of recurrence and should have a clear
owner or completion condition.

## 2. Practical session

### 2.1 Start with the Flask app

Use the local app from Day 8. Confirm it has a health endpoint, or use an
existing route. From its project directory:

```bash
flask --app app run --debug --host 127.0.0.1
```

In another terminal:

```bash
curl -i http://127.0.0.1:5000/health
```

Use `/` if the app has no `/health` route. The loopback address keeps this
development exercise on the local machine. Press `Ctrl+C` to stop the server.

### 2.2 Inspect a MySQL query plan

Use the sample schema from Day 9. Choose a read-only query that filters by a
column, then inspect the plan:

```sql
EXPLAIN SELECT item_id, name
FROM catalog_items
WHERE category_id = 1;
```

Record the access type, `possible_keys`, chosen `key`, and estimated `rows` if
those fields are present. Add an index only as an experiment on the lab data,
then compare plans. `EXPLAIN` describes the plan; it is not a substitute for
measuring representative query performance.

### 2.3 Build and run a local image

Use the Day 14 Dockerfile for the Flask app. Build and run the image with a
loopback-only host port:

```bash
docker build -t flask-lab:1 .
docker run -d --name flask-lab -p 127.0.0.1:5000:5000 flask-lab:1
curl -i http://127.0.0.1:5000/health
docker logs flask-lab
```

`EXPOSE` in a Dockerfile documents a container port; `-p` publishes a host
port. Check the image for accidental secrets and avoid placing database
passwords in a Dockerfile or source control. Stop the lab container when done:

```bash
docker stop flask-lab
```

### 2.4 Describe the Kubernetes objects

Before applying anything, sketch the desired state:

- A Deployment requests two replicas of the Flask image and labels its Pods.
- A readiness probe checks `/health` before routing traffic to a Pod.
- A Service selects those Pods and gives the app a stable in-cluster endpoint.
- Configuration and secrets are provided separately from the image.
- MySQL data is stored and backed up through an explicitly designed database
  service or persistent storage plan.

If you already have a disposable local cluster, first make the `flask-lab:1`
image available to that cluster, then apply a Deployment and Service manifest
with these properties. Check `kubectl get deployments,pods,services` and confirm
two ready replicas. A local cluster does not automatically see every image
built by the host's Docker daemon. Do not apply this exercise to a shared or
production cluster.

### 2.5 Run a controlled failure drill

Use only the named local Docker container. Capture a baseline, stop it, observe
the symptom, inspect status/logs, and restore it:

```bash
docker start flask-lab
curl --max-time 3 -i http://127.0.0.1:5000/health
docker stop flask-lab
curl --max-time 3 -i http://127.0.0.1:5000/health
docker ps -a
docker logs --tail 50 flask-lab
docker start flask-lab
curl -i http://127.0.0.1:5000/health
```

Record times and responses. The stopped-container request should fail or time
out. This is a lab simulation; never stop a production service as part of this
exercise.

## 3. Write a short blameless postmortem

Use the drill and fill in:

1. **Summary and impact:** what failed and which requests were affected?
2. **Timeline:** baseline, failure, detection, diagnosis, recovery.
3. **Root cause and contributing conditions:** what state caused the symptom?
4. **Detection and response:** which signal helped, and what delayed recovery?
5. **Follow-up actions:** two specific, trackable changes with an owner or
   completion condition.

For this drill, the container was stopped intentionally. Analyze whether the
health check, logs, and runbook made detection and recovery clear. Avoid wording
that blames the learner; describe how the setup could better support the next
operator.

## 4. Review and personal notes

1. What does a Flask route do?
2. What can `EXPLAIN` tell you, and what does it not prove?
3. How do a Docker image and container differ?
4. What do a Kubernetes Deployment and Service each provide?
5. Which parts of MySQL operations does Kubernetes leave to you to design?
6. What made the simulated incident detectable and recoverable?

- Flask route and response:
- MySQL plan observations:
- Image tag and container result:
- Kubernetes diagram or object notes:
- Postmortem follow-up actions:
  1.
  2.
- Three things I learned:
  1.
  2.
  3.
- Confusing concept / resolution:

## Done when

- [ ] The local Flask route returns the expected response.
- [ ] I recorded and can explain the main fields in one MySQL `EXPLAIN` plan.
- [ ] I built and ran a local Docker image without embedding secrets.
- [ ] I can describe how a Deployment and Service relate.
- [ ] I completed the local failure drill and wrote two postmortem actions.

## References

- [Flask quickstart](https://flask.palletsprojects.com/en/stable/quickstart/)
- [MySQL 8.4: `EXPLAIN`](https://dev.mysql.com/doc/refman/8.4/en/explain.html)
- [Docker: writing a Dockerfile](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)
- [Kubernetes overview](https://kubernetes.io/docs/concepts/overview/)
- [Google SRE: Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)

