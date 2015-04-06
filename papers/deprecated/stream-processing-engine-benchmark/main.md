# Introduction

Internet has become a central part of our daily lives and recent developments in
*Smart Phones*, *Internet of Things* and *Smart Homes* related technologies are going to make us connected through Internet more than ever. As more and more people and devices are connected together via web applications which Internet is built, these application will produce massive amount of data at rates which we haven't experienced yet. In parallel to the increase of data generation, business entities and users require this data and information extracted or created out of this data available to them as fast as possible and trending towards decreasing latency for ever.

As we have experienced in recent past, traditional relational database technologies couldn't cope up with these ever increasing data rates, volumes and ever decreasing latency. So technology giants like Google, Yahoo and Amazon invented  novel storage technologies like GFS \cite{ghemawat2003google}, HDFS, Big Table \cite{chang2008bigtable} and Amazon Dynamo \cite{decandia2007dynamo} and distributed data processing frameworks such as Map-Reduce \cite{dean2010mapreduce}. As the trend continues, these systems which follow traditional pull based approach to answering queries couldn't handle the latency requirements and throughput requirements, *Distributed Stream Processing* platforms were built on top of modern messaging infrastructures to fulfill these requirements. While most of these new distributed stream processing frameworks are inspired by Map-Reduce and related big data technologies, most of the underlying concepts were from previous work done by both academia \cite{abadi2005design} \cite{abadi2003aurora} \cite{stonebraker20058} \cite{hwang2005high} \cite{arasu2003stream} \cite{motwani2003query} and industry \cite{gedik2008spade} \cite{hirzel2014catalog}.

With this trend of moving to push based streaming processing platforms for reducing the latency and to support increasing throughput requirements, several distributed and single-node stream processing platforms were implemented and they are continue to evolve to tackle ever changing requirements. But due to different architectural decisions underlying these platforms and variations in problem classes and non-functional requirements these frameworks are trying to address, our experiments shows that these frameworks behave differently under different types of data and algorithms. In addition to that these systems support different types of programming abstractions.

These differences make the decision of selecting a specific platform hard. There is a stream data management benchmark \cite{arasu2004linear} defined almost a decade ago and there are existing results on how these platforms perform individually \cite{zaharia2012discretized}. But its hard to find comparative studies and existing benchmark application \cite{arasu2004linear} doesn't cover all aspects of modern distributed stream processing systems. In this work, we present a comprehensive evaluation of several popular distributed stream processing platforms and defines evaluation process which addresses multiple performance aspects such as *throughput*, *latency*, *scalability*, *resource utilization*, *fault tolerance* and *behavior under increasing load* across *type of streaming data* and *stream processing algorithm* dimensions. We believe that the *process*, *metrics*, *data sets* and *applications* described here make a comprehensive stream processing benchmark suite which can use to compare different distributed stream processing engines.

We implement this benchmarking suite on three popular distributed stream processing platforms - Apache Storm, Apache Samza and Apache Spark Streaming. This demonstrates the applicability of our benchmarking approach to compare and contrast distributed stream processing platforms.Our main contributions are:

- We propose number of micro-benchmarks which cover fundamental building blocks of stream processing applications such as filtering, enrichment, aggregation, join, counting, pattern detection and windowing.
- We present first comprehensive evaluation of several distributed stream processing platforms, along several dimensions including types of streaming data, stream processing applications, scalability and fault tolerance.


**Type of Streaming Data** - Event stream processing has uses in various different fields like Internet of Things, real-time analytics, fraud detection and trading. Each of these applications generate various types of data streams from time series data to streaming graphs. Then dimensionality of a tuple in these streams can varies from one to more than hundred (**Need references**).

**Stream Processing Tasks** - According to \cite{streamdrill:presentation}, we can categorize stream processing tasks on two dimensions, complexity of task and latency requirements of task. For example counting, averages and count distinct can be very simple where as fraud detection, outlier detection or predictions based on streams of data can be complex. On the other hand reporting and monitoring applications may not have strong latency requirements. Even 1 minute delay would be fine in most cases. But applications which controls some machine or device based on streaming data may require sub millisecond or sub second latency requirements.

# Data Sets and Be

# Benchmarking Framework and Methodology

## Benchmarks

**TODO* - Find streaming algorithms and references to their applications. The question is can we only use queries defined in \cite{mendes2009performance}

- Keen.io like ingest use case
- Twitter Top-K hash tags
- Its important to measure how each of these systems responds to the variations in data rate.
- Its important to measure the performance metrics when multiple continuous computations are running.

### Oracle Complex Event Processing Design Patterns

- Event Filtering
- New Event Detection
- Event Partitioning
- Event Enrichment
- Event Aggregation
- Event Correlation
- Application-time Events
- Missing Event Detection

# Experiment Setup

In this section we describe the implementation of benchmarking framework we defined in Section \ref{benchmarking-framework-and-methodology}. Rest of the section describes how stream processing platforms are selected, hardware and software configurations, stream processing algorithm related configurations and details on tuning specific stream processing platform.

Across all the stream processing platforms, we use Kafka as the data stream source and all the incoming messages are thrift encoded except for cases we have specifically mentioned. *If we need to convince the use of Kafka, [this](https://cwiki.apache.org/confluence/display/KAFKA/Powered+By) is a good resource.
