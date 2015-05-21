# Streaming Aggregations

AFAIK, there are two types of aggregations based on how they are expressed and implemented.

## Windowed aggregations

Explicit window definition is there in this scenario and there is a LogicalWindow operator prior to the LogicalAggregate in the logical query plan.

```
Some Operator <--- Aggregate <--- Project (Optional) <--- Window <--- Scan (with Filter)
```

## General Group-By Aggregation with Sorted (Monotonic) Input

This approach work best for **tumbling window** queries like below. Constraint is to having a monotonic column expression in the group-by clause. 

```sql
SELECT STREAM FLOOR(rowtime TO HOUR) AS rowtime,
  productId,
  COUNT(*) AS c,
  SUM(units) AS units
FROM Orders
GROUP BY FLOOR(rowtime TO HOUR), productId;
```

# Samza SQL Aggregate Operator

Because of the two different scenarios mentioned above our aggregate operator should be implemented in a way that it handles both scenarios with fault tolerance and should support replays. One thing to note is that, we have fault tolerance implemented for window operator based on window store. So we should design window and aggregate operators in a way that we can reuse window store functionality for streaming style aggregates using standard SQL constructs.

I believe, we can avoid handling fault tolerance in aggregate operator when we have a explicit window operator because faults are already handled by window operator. But this may require coordination between two operators because window operator has to know that aggregate operator was successfull. 

Lets consider the scenario where we have explicit window operator,

To understand this scenario we need to know:

* How window operator sends tuples downstream?
* How it handles faults?
* What happens when a query/node/task fails in the middle of processing a window?
* How it handles replays?
* What aggregates needs to be supported?
* What is the job of aggregate operator in case we have explicit window operator? Just incremental aggregation?

Then for aggregate only scenario,

* When to emit results?
* How to handle window expiration?
* How to handle faults?
* Can we reuse window store?
* How to handle non-streaming aggregates? May be we need punctuations?

## Aggregate Operator Logic

```
List<Grouping> groupings;
proc process(tuple, messagecollector) {
    if(instanceof WindowMessageTuple){

    } else if(instanceof IntermediateMessageTuple) {

    }
}
```







