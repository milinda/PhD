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

The result of the above query will be a stream. At every 5-minutes Samza SQL will emit click count of ads where there was ad clicks in previous 5-minutes. Late arrivals will be handled according to the window policy and updated count will be emitted to the output stream. Samza SQL infer that it needs to emit ad click counts at every 5-minutes based on the fact that it knows *clickTime* is increasing  and also knows that round5min(clickTime to timestamp) is also increasing. For example once Samza SQL sees a row at or after 10:05:00 it knows that, it needs to emit a patial ad click count for last 5 minutes and when there is a another ad click event belongs to 10:00:00 to 10:05:00 interval it will update previous count and re-emit the counts for that time period until window close timeout is passed. 

The columns or expressions that is increasing or decreasing is know as *monotonic* column/expression. Without a monotonic expression in GROUP By query, Samza SQL can't make any progress in streaming queries and will throw a error at query validation phase. 

In scenarios like mentioned above, Samza SQL converts GROUP BY expressions from data domain to time domain if the GROUP BY contains a monotonic timestamp field and enables handling of delayed events and intcremental processing without extra effort from user.

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

# System Challenges

