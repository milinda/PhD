Title:   Daily Journal
Author:  Milinda Pathirage
Date:    October 1, 2014

# 10/01/2014

Watched some insightful [video](https://www.youtube.com/watch?v=fU9hR3kiOK0) on Kappa Architecture where everything is a *Stream*. Also watched a [video](https://www.youtube.com/watch?v=QdkS6ZjeR7Q) about linearizability of different messaging and storage solutions. It was a presentation which makes you wonder whether you know the core distributed system design techniques and inspire others to learn about distributed systems. It mainly talks about CAP theorem and consistency guarantees. **Another important finding of the presenter is Elasticsearch can loose writes in cluster mode, due to problems in consistency handling.**

I found [Tango: Distributed Data Structures over a Shared Log](http://research.microsoft.com/apps/pubs/default.aspx?id=199947) for tomorrow's reading list. [SAMOA](http://yahoo.github.io/samoa/SAMOA-Developers-Guide-0-0-1.pdf) documentation is also for tomorrow's reading list. Also need to read [Designing Data-Intensive Applications](https://www.safaribooksonline.com/library/view/designing-data-intensive-applications/9781491903063/) in near future.

## Interesting Facts About Storm

Source [http://blog.newitfarmer.com/big_data/streams/storm/13517/repost-storm-vs-samza](http://blog.newitfarmer.com/big_data/streams/storm/13517/repost-storm-vs-samza)

Storm's at-least once semantics cause out of order message delivery. This is a problem for scenarios like [Kappa Architecture](http://radar.oreilly.com/2014/07/questioning-the-lambda-architecture.html). At-least once semantics may cause back pressure, and the graphs are mostly limited to single logical function. Possible solution is to connect graphs using intermediate spouts.

Transactional topologies are there, but limited to single stream. In contrast Samza always offers guaranteed delivery and ordering of input within a stream partition.

State management is left for the users of Storm, but Samza has in-built state management.

Samza is single job per process and process is single threaded.Storm is [different](http://www.michael-noll.com/blog/2012/10/16/understanding-the-parallelism-of-a-storm-topology/). Dynamic rebalancing is not there in Samza.
