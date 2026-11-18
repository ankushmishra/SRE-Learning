# Day 11 - Systems design: scalability, availability, and fault tolerance

## Session goal

Evolve a small application design and sketch a telecom catalog service. Identify
single points of failure, reason about 10x traffic, and explain which component
you would scale first and why.

Suggested time: 90-120 minutes.

## 1. Fundamentals

- **Scalability:** ability to handle increased load by adding resources or
  improving capacity.
- **Availability:** ability to serve requests when users need the service.
- **Fault tolerance:** ability to continue useful operation when components
  fail.

These goals overlap but are not interchangeable. Adding capacity may help
latency yet leave a single database as a failure point. Replication can improve
availability but introduces consistency and failover decisions.

A load balancer distributes requests across application instances. Stateless
instances are easier to add horizontally. A database stores durable state and
often needs its own backup, capacity, and availability plan. Caches can reduce
repeated reads but add invalidation and staleness concerns. Queues can smooth
bursts for asynchronous work.

## 2. Practical design exercise

### 2.1 Start with the course photo-sharing example

Follow the course's evolution of its photo-sharing service. At each stage,
write what limit the design has reached (storage, request volume, latency, or
failure recovery) and which change addresses that limit.

### 2.2 Sketch a telecom catalog service

On paper, draw at least:

```text
Clients -> Load balancer -> App instance A ---+
                             App instance B ---+-> Database
```

Add arrows and short labels to show request and data flow. Then answer:

1. What happens if one app instance fails?
2. What happens if the load balancer or database fails?
3. What would change at 10x read traffic? Consider caching, app replicas, and
   database read capacity, but choose based on measurements.
4. Which component would you scale first, and what metric would justify it?
5. Where would you add backups, health checks, timeouts, or a queue?

Mark single points of failure with `SPOF`. Add one mitigation for each, and
write a cost or complexity introduced by that mitigation.

## 3. Review and Day 11 notes

1. Define scalability, availability, and fault tolerance in one sentence each.
2. Why put a load balancer in front of multiple app instances?
3. What still fails if the database is a single instance?
4. What evidence would you gather before scaling a component?

- Diagram location or description:
- Single points of failure and mitigations:
- First component to scale at 10x and evidence:
- Three things learned:
  1.
  2.
  3.
- Interview question to practice:

## Done when

- [ ] My diagram includes a load balancer, two app instances, and a database.
- [ ] I marked single points of failure and discussed 10x traffic.
- [ ] I can explain why each component is present.

## Further reading

- Review the course module on scalability, high availability, and fault tolerance.
- [Google Cloud patterns for scalable and resilient apps](https://docs.cloud.google.com/architecture/scalable-and-resilient-apps)

