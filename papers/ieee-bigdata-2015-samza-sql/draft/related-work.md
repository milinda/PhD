# Related Work

## Samza SQL Uniqueness

* Developed on top of Stateful Stream Processing Model
* Supports standard SQL with minimal set of extensions
* Support revinding
* Support fault tolerance and checkpointing
* Data Parallel

- Liquid
- Summingbird
- Druid
- RADStack
- SparkSQL
- Stream aggregation literature and window handling literature
- CQL
- Trill
- Photon

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

# Liquid

From: http://blog.acolyer.org/2015/02/04/liquid-unifying-nearline-and-offline-big-data-integration/

Web companies such as LinkedIn are dealing with very high volumes of append-only data, “which becomes expensive to integrate using proprietary, often hard-to-scale data warehouses.” (See Mesa for Google’s solution to the data-warehousing problem). So organizations create their own data integration stacks for storing data and serving it to back-end data processing systems.

Examples of nearline systems include those that query social graphs, search data, normalize data, or monitor change. The sooner they provide results, the higher the value to end users. With multiple stages in processing pipelines, the latency per stage starts to add up.

Additional issues the authors cite with the DFS based approach include the need to support incremental processing (fine-grained access to data and explicit incremental state), and the need for resource isolation:

Liquid is designed for low latency, cost-effective, incremental processing with high availability and resource isolation. “It annotates data with metadata such as timestamps or software versions, which back-end systems can use to read from a given point. This rewindability property is a crucial building block for incremental processing and failure recovery.”

**The well-known Lambda architecture was rejected since “developers must write, debug, and maintain the same processing code for both the batch and stream layers, and the Lambda architecture increases the hardware footprint.” Likewise the Kappa architecture, while it requires only a single processing path, “has a higher storage footprint, and applications access stale data while the system is re-processing data.”**

Liquid has two cooperating layers: a messaging layer based on Apache Kafka, and a processing layer based on Apache Samza.

The layers communicate through feeds (topics in a pub/sub model). There are raw data feeds, and derived data feeds – the latter contain lineage information which is stored by the messaging layer.

Topics are divided into partitions, which are distributed across a cluster of brokers. Topics are realized as distributed commit logs – each partition is append-only and keeps an ordered, immutable sequence of messages with a unique identifier called an offset.

Consumers can checkpoint their last consumed offsets to save their progress and facilitate recovery.

The processing layer executes jobs, which embody computation over streams. Jobs can be divided into tasks that process different partitions of a topic in parallel. Jobs can communicate with each other, forming a dataflow processing graph.

**The processing layer (i) executes ETL-like jobs for different back-end systems according to a stateful stream processing model; (ii) guarantees service levels through resource isolation; (iii) provides low latency results; and (iv) enables incremental data processing. A messaging layer supports the processing layer. It (i) stores high-volume data with high availability; and (ii) offers rewindability, i.e. the ability to access data through metadata annotations.**

# StremaSQL for Spark

https://spark-summit.org/2014/talk/streamsql-on-spark-manipulating-streams-by-sql-using-spark

# Trill

# Photon and Mill Wheel

# Existing Streaming SQL Standards

We have to talk about languages proposed by following papers and why we choose our language

- CQL
- Towards a Streaming SQL Standard (http://cs.brown.edu/~ugur/streamsql.pdf)
- ATLAS: a Small but Complete SQL Extension for Data Mining and Data Streams (http://www.vldb.org/conf/2003/papers/S37P01.pdf)
