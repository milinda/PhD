Title:   Daily Journal
Author:  Milinda Pathirage
Date:    October 1, 2014

# 10/31/2014

How about using provenance to handle automatic review of output coming out of copyrighted content. But it will be partial review.

# 10/08/2014

Main task for today is to implement a evicting queue backed by NoSQL storage. This is required to implement WindowOperator in KappaQL. During a failure, we should be able to recover ordered window synopsis.


# 10/06/2014

Started to implement query operators for KappaQL. First thing is to implement WindowOperator which implements sliding window concepts. Looks like there are lot to read regarding Samza state management APIs.

## WindowOperator Implementation

*  Basic requirement is to support both sliding windows and tumbling windows.
*  How to implement synopsis based on Samza key/value store is the today's problem.



# 10/03/2014

## KappaQL DSL Implementation

Started planning the KappaQL implementation. First thing I am trying to figure out is how I am going to implement the query DSL. We need to have the concept of *STREAM*  as well as *SQL Relation* in the query language. Tentative query DSL.

```clojure
(defstream stream-name)

(create-view :view-name
    (fields [:alias :original_name] :column_2 :column_3)
    (with
        :cassandra_cluster "http://cassandra.cluster.com:3444"
        :user "user"
            :pass "pass")
    (as
        (select stream_name
            (where {})
            (group {}))))
```

One of the main requirements of this query DSL is to support windowing. But as the first step I am going to try very simple queries.

## Immediate Plan

Select very simple queries based on wikipedia data set and implement queries manually. **Main concern is whether to implement based on Samza or Storm**.

## The CQL Continuous Query Language: Semantic Foundations and Query Execution∗

### Abstract semantics

- Two data types: *streams* and *relations*
- Three classes of operators: *stream-to-relation*, *relation-to-relation*, *relation-to-stream*
- SQL for *relation-to-relation*
- Window specification lang based on SQL99 for *stream-to-relation*
- Three operators for *relation-to-stream*

#### Definitions

- **Stream**: A stream S is a (possibly infinite) bag (multiset) of *elements* ⟨s,$\tau$⟩, where s is a tuple belonging to the schema of S and $\tau$ $\epsilon$ T is the timestamp of the element.
- **Relation**: A relation R is a mapping from T to a finite but unbounded bag of tuples belonging to the schema of R. This is a *instantaneous relation*. In standard definition, *relation* is just a set of tuples. We use the term *base relation* for input relations and *derived relation* for relations produced by query operators

**Time “advances” to $\tau$ from $\tau$ − 1 when all inputs up to $\tau$ − 1 have been processed.**

*  *stream-to-relation* operators are based on *sliding window* over a stream. Three classes of sliding window operators, *time-based*, *tuple-based*, *partitioned*. (We may need *fixed*, *tumbling* and *value-based* windows)
*  Time based sliding window is defined as $S [Range T]$ which is $R(\tau )={s|⟨s,\tau ′⟩ \epsilon S ∧ (\tau ′ ≤\tau ) ∧ (\tau ′ ≥max{\tau −T,0})}$. Two special cases $S [Now]$ and $S[Range Unbounded]$ for $T = ∞$.
*  Tuple based windows use $S ~ [Rows ~ N]$ and $[Rows ~ Unbounded]$ for $N$ is infinite.
*  Not clear about partitioned windows, but we can use group by and other window types together to implement this.




# 10/02/2014

Started to working on KappaQL project which implements *Big Data Kappa Architecture* as a Clojure DSL and a REPL. I also read the [article](http://martin.kleppmann.com/2014/03/26/six-things-about-scaling.html) about problems we face when scaling web apps. It contains some interesting facts which are useful in HTRC context.

# 10/01/2014

Watched some insightful [video](https://www.youtube.com/watch?v=fU9hR3kiOK0) on Kappa Architecture where everything is a *Stream*. Also watched a [video](https://www.youtube.com/watch?v=QdkS6ZjeR7Q) about linearizability of different messaging and storage solutions. It was a presentation which makes you wonder whether you know the core distributed system design techniques and inspire others to learn about distributed systems. It mainly talks about CAP theorem and consistency guarantees. **Another important finding of the presenter is Elasticsearch can loose writes in cluster mode, due to problems in consistency handling.**

I found [Tango: Distributed Data Structures over a Shared Log](http://research.microsoft.com/apps/pubs/default.aspx?id=199947) for tomorrow's reading list. [SAMOA](http://yahoo.github.io/samoa/SAMOA-Developers-Guide-0-0-1.pdf) documentation is also for tomorrow's reading list. Also need to read [Designing Data-Intensive Applications](https://www.safaribooksonline.com/library/view/designing-data-intensive-applications/9781491903063/) in near future.

## Interesting Facts About Storm

Source [http://blog.newitfarmer.com/big_data/streams/storm/13517/repost-storm-vs-samza](http://blog.newitfarmer.com/big_data/streams/storm/13517/repost-storm-vs-samza)

Storm's at-least once semantics cause out of order message delivery. This is a problem for scenarios like [Kappa Architecture](http://radar.oreilly.com/2014/07/questioning-the-lambda-architecture.html). At-least once semantics may cause back pressure, and the graphs are mostly limited to single logical function. Possible solution is to connect graphs using intermediate spouts.

Transactional topologies are there, but limited to single stream. In contrast Samza always offers guaranteed delivery and ordering of input within a stream partition.

State management is left for the users of Storm, but Samza has in-built state management.

Samza is single job per process and process is single threaded.Storm is [different](http://www.michael-noll.com/blog/2012/10/16/understanding-the-parallelism-of-a-storm-topology/). Dynamic rebalancing is not there in Samza.
