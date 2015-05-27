# System Architecture

In response to the challenges and requirements discussed in Section \ref{sec:overview}, we developed SamzaSQL, a scalable and fault-tolerant fast data query engine capable of supporting nearline analytics task as well as replay based offline analytics tasks. SamzaSQL also allows users to query both data streams and slow-changing stored data in a single query. In this section, we outline the SamzaSQL architecture.

SamzaSQL consists of three primary architectural components as show in Figure (??). A \textit{query management layer}  (i) accepts queries from users; (ii) parse, validate and finally  generate the execution plan -- a Samza job or a set of Samza jobs -- from query; (iii) keeps track of queries and respective jobs. A \textit{processing layer} -- Samza cluster -- (i) executes job generated for queries submitted by user; (ii) handles fault-tolerance;. A \textit{messaging layer} supports the processing layer by (i) storing high-volume fast data (both input and intermediate outputs) with high-availability; and by (ii) providing replaying support.

* how processing layer and messaging layer communicate
* how models/schemas managed
* how views are maintained

# Design and Implementation



We need to talk about use of Calcite and how we extend Calcite to do stream processing specific optimizations and query plan transformation. Then we need to discuss implementation details from the summary points.

**Are we going to support aborting transactions. We will have to roll back all modifications to raw and derived stream histories (at least eventually).**

**Abort is hard in streaming context due to:**

- Waiting until data is committed before processing leads to significant extra latency and defeats the purpose of streaming system
- commingling and then processing dirty uncommitted data from multiple transactions makes it hard to unwind the effects of a single transaction.

**How we handle Recovery. We have to talk about this in detail with respect to Kafka and Samza.**
