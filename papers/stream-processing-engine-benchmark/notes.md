# Storm Development

1. One of the main question when developing using Storm is which API to use. We can use low-level Bolt/Spout based API or Trident. General rule is to use Trident when "Exactly Once" semantic is needed, otherwise use low-level API.
2.

# Common Patterns

## From [Storm Documentation](http://storm.incubator.apache.org/documentation/Common-patterns.html)

- **Streaming Joins** - normal database join has finite input and clear semantics for a join, a streaming join has infinite input and unclear semantics for what a join should be. *Field grouping from multiple streams to a joiner bolt*.
- **Batching** - Oftentimes for efficiency reasons or otherwise, you want to process a group of tuples in batch rather than individually. For example, you may want to batch updates to a database or do a streaming aggregation of some sort.
- **In-memory caching + fields grouping combo**
- Streaming Top K
-

# Papers

## A Performance Study of Event Processing Systems

This is about CEPs. Its not their intention to provide and in-depth comparison of existing CEP engines, but rather to give a first insight into the performance of current products as a way to identify bottlenecks and opportunities for improvement.

### Interesting Claims

CEP engines should be able to *continuously adapt* their execution to cope with variations and should be able to scale by *sharing computation* among similar queries.

### Questions

- What are the performance bottlenecks?
- Will performance degrade gracefully with increasing load?

To answer above run micro-benchmarks on 3 engines while varying query params like window-size, window expiration type, predicate selectivity, and data values. Asses the engines scalability with respect to number of queries.

Similar queries have widely different performance on the same or different engines and that no engine dominates the other two in all scenarios.

### Use Cases

- Fraud detection
- Traffic monitoring
- intensive care units

### Different Operational Requirements In Terms Of

- Types of events
- Queries/patterns complexity
- Throughput
- Latency
- Number of sources and sinks

### Fundamental Operations

- Filtering(Selection/Projection)
- Aggregation/Grouping
- Merging(Union)
- Join
- Sorting/Ranking
- Pattern detection
- Windowing
- Correlation/Enrichment(Join)

### Performance Depends On

- Algorithm Implementing CEP operations
- Params such as window type and size
- Predicate selectivity
- External parameters such as available resources, incoming data, and number and type of queries and rules

### Window Types

- Sliding
- Thumbling
- Landmark

# Resources

* [Near realtime, streaming, and perpetual analytics](http://radar.oreilly.com/2013/07/near-realtime-streaming-and-perpetual-analytics.html)
* [Moving from Batch to Continuous Computing at Yahoo!](http://radar.oreilly.com/2013/06/moving-from-batch-to-continuous-computing-at-yahoo.html)
* [Hokusai — Sketching Streams in Real Time](http://arxiv.org/pdf/1210.4891v1.pdf)
* [Hailstorm: Distributed Stream Processing with Exactly Once Semantics](http://www.scs.stanford.edu/14sp-cs240h/projects/dimson_ganjoo.pdf)
