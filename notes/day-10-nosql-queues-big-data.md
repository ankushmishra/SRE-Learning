# Day 10 - NoSQL, message queues, and big data concepts

## Session goal

Choose suitable roles for a relational database, a NoSQL store, and a message
queue in a telecom ordering flow. This is a journal and diagram exercise; no
service setup is required.

Suggested time: 60-90 minutes.

## 1. Fundamentals

### 1.1 Relational database

A relational database fits structured records and relationships that benefit
from constraints and transactions. An order, its line items, and its lifecycle
can be modeled with related tables. SQL databases are useful when correctness
across related records matters.

### 1.2 NoSQL stores

NoSQL describes several data models rather than one database type. Examples
include key-value, document, wide-column, and graph stores. A document store can
fit flexible product metadata; a key-value store can fit short-lived lookup or
cache data. Pick the model based on access patterns, consistency needs, and
operational constraints, not because a system is labeled â€œNoSQL.â€

CAP is a way to reason about behavior during a network partition: a distributed
system cannot guarantee both a consistent view and successful responses on
every side of the partition at the same time. Real systems make nuanced,
operation-specific tradeoffs; CAP is not a simple ranking of databases.

### 1.3 Message queues

A queue lets one system publish work for another system to process later. It
can absorb bursts and reduce direct timing dependencies between producers and
consumers. Queues introduce their own concerns: retries, duplicate delivery,
ordering, poison messages, backlog monitoring, and idempotent consumers.

### 1.4 Big data

Big data systems process datasets that may be too large or fast-moving for one
ordinary database query. Batch processing handles bounded collections; stream
processing handles events continuously. For this exercise, identify whether a
use case needs large-scale analytics rather than adding a big-data system by
default.

## 2. Practical journal exercise

Use a telecom order-to-activation flow you understand. Draw:

```text
Customer order -> order service -> activation work -> network provisioning -> status update
```

Annotate it with one concrete example each:

1. **SQL:** store the order, customer reference, and current lifecycle state.
   State why transactions or relational constraints help.
2. **Queue:** place activation work between order intake and provisioning.
   Explain how this helps if provisioning is temporarily slower than orders.
3. **NoSQL:** store flexible device or service attributes, or a lookup/cache
   keyed by service identifier. Explain the access pattern that makes it fit.
4. **Big data (optional):** analyze historical activation events for trends;
   distinguish that analytics workload from the live transaction path.

For the queue, write what happens if a consumer fails after processing a
message but before acknowledging it. Consider retries and duplicate handling.

## 3. Review and Day 10 notes

1. What concrete data belongs in SQL in your flow, and why?
2. Which queue boundary reduces direct coupling?
3. What access pattern makes your NoSQL example suitable?
4. How will a consumer avoid harmful duplicate processing?
5. Which part is analytics rather than live transaction processing?

- Diagram or flow:
- SQL example and reason:
- Queue boundary and failure behavior:
- NoSQL example and reason:
- Three things learned:
  1.
  2.
  3.
- One unfamiliar term to revisit:

## Done when

- [ ] My notes name one concrete SQL, NoSQL, and queue use case.
- [ ] I gave a reason for each choice.
- [ ] I explained one queue failure/retry scenario.

## Further reading

- Review the course modules on NoSQL key concepts, message queues, and big data.
- For a queue example, see [Apache Kafka introduction](https://kafka.apache.org/intro).

