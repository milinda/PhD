# Samza SQL Overview with Example

Consider a stream of ad click events, where events are encoded in Avro format with the schema:

```json
{
     "type": "record",
     "namespace": "org.apache.samza",
     "name": "AdClick",
     "fields": [
       { "name": "clickTime", "type": "long" },
       { "name": "userId", "type": "long"},
       { "name": "adId", "type": "long"}
     ]
}
```

User wants to compute, for each ad, a 5-minute windowed count of clicks on that ad, across a 5% sample of users in near realtime.

## User Experience

Its theoritically possible to using any type of messaging system to input data streams to Samza. But Kafka is the preffered and stable messaging infrastructure for Samza. Lets assume 

* events are published to a Kafka topic *adclick*, partitioned on adId
* *adclick* topic is registered in Kafka *Schema Registry* known to Samza SQL query driver

When connected to a Kafka Schema Registry instance, Samza SQL driver automatically lazy loads stream definitions into the query planner's in-memory metadata store and topic registered in that instance of Schema Registry instance will be visible to the query planner with correct type informations. So the user can write the above mentioned  query logic as a Samza SQL query:

```sql
  clickTime | userId    |  addId 
------------+-----------+---------
 10:00:00   |        30 |       2 
 10:02:43   |        10 |       1  
 10:04:23   |        20 |       1  
 10:06:10   |        10 |       3  
 10:04:56   |        40 |       2 

SELECT STREAM 
  round5min(clickTime to timestamp) as clickTime,
  COUNT(*) as clicks
FROM adclick
WHERE userId % 100 < 5
GROUP BY round5min(clickTime to timestamp), adId;

  clickTime | userId    |  addId 
------------+-----------+---------
 10:00:00   |        30 |       2 
 10:00:00   |        10 |       1  
 10:00:00   |        20 |       1  
 10:05:00   |        10 |       3  
 10:00:00   |        40 |       2 
```

The result of the above query will be a stream. At every 5-minutes Samza SQL will emit click count of ads where there was ad clicks in previous 5-minutes. Late arrivals will be handled according to the windowing policy and updated count will be emitted to the output stream. Samza SQL infer that it needs to emit ad click counts at every 5-minutes based on the fact that it knows *clickTime* is increasing  and also knows that round5min(clickTime to timestamp) is also increasing. For example once Samza SQL sees a row at or after 10:05:00 it knows that, it needs to emit a patial ad click count for last 5 minutes and when there is a another ad click event belongs to 10:00:00 to 10:05:00 interval it will update previous count and re-emit the counts for that time period until window close timeout is passed. 

The columns or expressions that is increasing or decreasing is know as *monotonic* column/expression. Without a monotonic expression in GROUP By query, Samza SQL can't make any progress in streaming queries and will throw a error at query validation phase. 

To handles aggregates like above, Samza SQL implements a relaxed version of SQL stream aggregation where rows can be out of order to accomodate late arrivals common in distributed data streams. Samza SQL window aggregates emit partial results as soon as possible and handle the out of order tuples according to windoing policy. For Samza SQL tumbling window aggregate like above, single input row can be contributed to multiple output rows due to partial aggregation which is completely different from standard SQL aggregate semantic where each input row contributes to only one output row.

If user wants this output stream to be appear in a another stream he can use INSERT INTO query to deliver the results of this query to an another stream.

```sql
INSERT INTO adclickcounts
  SELECT STREAM 
    round5min(clickTime to timestamp) as clickTime,
    COUNT(*) as clicks
  FROM adclick
  WHERE userId % 100 < 5
  GROUP BY round5min(clickTime to timestamp), adId;
```

Some other important use cases in a streaming query context are joining with stored relations and windowed joins. Samza SQL provide support for those constructs as well, but the full description on query implementaiton is outside the scope of this paper. But we note that Samza SQL supports all the basic standard SQL constructs on both streams and relations.

Samza SQL is quite different from existing systems which implements Lambda architecture and Kappa architecture mainly because Samza SQL is build based on relational algebra and it views streams as relaitons that varies with time similar to CQL. This enabled easy integration of slow-changing data into streaming queries as well as provides consistent view across both streaming and relational data sources.

# System Overview

Samza SQL first compile user query into a logical query plan and then optimize this logical query plan based on different heuristics and metrics related streaming. Then this logical query plan get converted into a physical plan consisting Samza SQL operator DAG. Each of these operators consume one or more streams of tuples and produces a stream of tuples where tuple represents a single stream element. Samza pulls tuples from input streams (sources) and push them to the operators. Elements in a stream can be formatted using a message format like Avro or JSON and Samza SQL provide plugging architecture to integrate new message formats to the framework. 

Samza SQL supports queries on both streams and tables where

- A stream is a (possibly infinite) sequence of elements partitioned by user selected key or partitioning mechanism and a partition is an ordered, immutable sequence of elements.
- A table is analogous to tables in relational databases, but not limited only to relational databases. Samza SQL provides extension mechanisms necessary to support different types of slow-changing data sources based on user preference.

The output from streaming SQL queries can be ingested into an another stream or a database for further consumption. Query results can laso be materialized into materialized views. Samza SQL encourage ingesting streaming outputs into an another stream for easily handling of partial aggregates caused by out of order arrival of data. Most important requirements and Samza SQL design decisions which caters those requirements are summerized below and discussed in the coming sections including a performance evaluation of current implementation.

### Familiar language
Samza SQL uses extended version of standard SQL which makes it easy to learn for anyone who knows SQL. As a result this results in clear semantics where we try to produce same results on a stream as if the data were in a table. For example, Samza SQL produce same result for tumbling window query 1 if there aren't any out of order tuples. But in case of out of order tuples there can be multiple partial emits until window is closed. Also using extended version of standard SQL helped us to utilize existing query parsing and optimization framework in the front-end. This reduces development time as well as it gave us lot of oppertunity focus on the streaming aspects of the system.

### Early results and incremental processing
Its important to support incremental processing in streaming query engines. Samza SQL implements all the aggregators in incremental fashion and different window policies are employed to produce partial early results in the precense of out of order arrivals. Samza SQL detect queries of blocking nature early at the query validation phase to prevent accidental infinitely blocking queries during runtime.

### Support both streams and relations
Samza SQL supports both streams and relations from slow-changing data sources like NoSQL storages and relational databases. We are utilizing Calcite capability to generate query plans consisting of scanning both streams and relations to implement this functionality for Samza SQL. This 

### Parallesim and Scalability
Initial implementation of Samza SQL supports data parallelism using Kafka's partitioning capabilities. Each partition will be handled by Samza SQL task specific to that partition. Task parallelism for scaling large queries are not supported yet.

### Fault tolerance
Samza SQL achive fault tolerance using Samza's local storage and reply capability. Local state of Samza SQL tasks will be backed up as a change stream and incase of a failure tasks will get restored with local state containing data upto last know checkpoint. In addition to that Samza SQL will checkpoint window processing into a another change stream such that it can replay tuples lost due to task failure from Kafka after restoring the local state.

### Ability to replay


### Materialized views

