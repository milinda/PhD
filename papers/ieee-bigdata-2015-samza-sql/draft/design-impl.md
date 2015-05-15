# Design and Implementation

We need to talk about use of Calcite and how we extend Calcite to do stream processing specific optimizations and query plan transformation. Then we need to discuss implementation details from the summary points.

**Are we going to support aborting transactions. We will have to roll back all modifications to raw and derived stream histories (at least eventually).**

**Abort is hard in streaming context due to:**

- Waiting until data is committed before processing leads to significant extra latency and defeats the purpose of streaming system
- commingling and then processing dirty uncommitted data from multiple transactions makes it hard to unwind the effects of a single transaction.

**How we handle Recovery. We have to talk about this in detail with respect to Kafka and Samza.**
