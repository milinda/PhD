# Related Work

- Liquid
- Summingbird
- Druid
- RADStack
- SparkSQL
- Stream aggregation literature and window handling literature
- Trill

We have to show that all the query examples from previous streaming literature is possible in our language too without modifications to SQL.

# Paper Summaries

## Continuous Analytics Over Discontinuous Streams (Truviso)

### Main Contribution

They emphaisze that most of the stream processing related work has been heavily influenced by an assumption that data streams can be viewed as sequences of data arrived more or less in order. But they claimed that, in reallity streams are not often so well behaved and disruptions of various sorts are endemic. They propose a solution to the problem of processing streaming data that arrives arbitrarily late based. The solution, inspired by data parallel query processing such as MapReduce, preformed streaming queries over independent sub-streams with arbitrary time ranges in parallel, generating partial results. This a order-independent processing model.

Some interesting claims about existing systems/research:

- typically tolerate only small degress of delay and out-of-orderness in the arrival streaming data
- Above may incurr mandatory latencies, potentially expensive buffering
- In cases where the discontinuity/delay of the input exceeds the limits that can be handled, the incming data is often simply dropped or at best spooled to back-up storage for eventual integration into the system

Practical issues:

- streams are rarely continuous
    - in distributed environments, data from multiple sources can arrive in arbitrary order even under normal circumstances
- failure or disconnection followed by subsequent recovery of connectivity between remote sites causes even larger discontinuities (sites went down, comes up and start sending old data)
- network partitions between datacenters can cause same discontinuous behavior
- parallel execution techniques that are critical for scaling up break the sequential/in-order nature of stream processing

Above problems are common in emerging "Big Data" applications where stream processing systems are being increasingly used.

There solution uses flexibility of data parallel query processing mechanisms and uses sophisticated query rewriting techniques in order to hide the complexity of dealing with discontinuous streaming data from app developers and query writers.

Key insight is that, once stream procesing is freed from the assumption of continuous streams, many importnant features become more natural to implement, as the inherently disrupt ordering.

Order independent advantages:

- Intra-query parallelism for both multi-core and cluster based processing
- Fault tolerance based on rapid recovery in which a recovering instance can begin processing newly arriving data while it is still re-establishing its state
- High availability deployments where recovering or new instances can retrieve state information and missed data from running instances
- Bootstraping continuous queries in which queries over streams can first be run over historical data while catching up to the current stream.

Parallel partial processing is implemented based on query rewriting. If a query can operate in terms of partial aggregates and if so transform it appropriately. In next step they generate views that encapsulate deferred consolidation of partial aggregates.

Query rewrites allows queries to correctly execute over streams that are dynamically partitioned. To hide the underlying partitioning from query writers and app devs, relational views (both static and streaming) are put in place.


rollups are used to handle scenario where use is interested in computing the same aggregate at various levels. For example while computing 1 min tumbling window aggregate, do the same for 5 min or 15 min while having 1min and 5min queries.

Background reducer is used to handle large numbers of different partial state records which can occur when there are large amount of out-of-order data. Background reducer keep the partial state small by periodically performing reduce aggregate.

For serial processing (SP) streams order-independence is only used for very out-of-order data by periodically repairing any affected archived SP data.

Recovery comes for free parallel processing (PP) streams. Recovery for serial processing is a more challenging requirement because of the large amounts of runtime state managed in main memory.
three step crash recovery

- Standard db style crash recovery of all durable state
- marking all SP archives self-consistent with each other and the latest committed data from their underlying arhcive and therefore the archive of the raw stream
- rebuilding the runtime state of the various operators in the CQ executor


