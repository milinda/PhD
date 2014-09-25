# Introduction

Internet has become a central part of our daily lives and recent developments in
*Smart Phones*, *Internet of Things* and *Smart Homes* related technologies are going to make us more connected through Internet more than ever. As more and more people and devices are connected together via web applications which Internet is built, these application will produce massive amount of data at rates which we haven't experienced yet. In parallel to the increase of data generation, business entities and users require this data and information extracted or created out of this data available to them as fast as possible and trending towards decreasing latency for ever.

As we have experienced in recent past, traditional relational database technologies couldn't cope up with these ever increasing data rates, volumes and ever decreasing latency. So technology giants like Google, Yahoo and Amazon invented  novel storage technologies like GFS \cite{ghemawat2003google}, HDFS, Big Table \cite{chang2008bigtable} and Amazon Dynamo \cite{decandia2007dynamo} and distributed data processing frameworks such as Map-Reduce \cite{dean2010mapreduce}. As the trend continues, these systems which follow traditional pull based approach to answering queries couldn't handle the latency requirements and throughput requirements, *Distributed Stream Processing* platforms were built on top of modern messaging infrastructures to fulfill these requirements. While most of these new distributed stream processing frameworks are inspired by Map-Reduce and related big data technologies, most of the underlying concepts were from previous work.\textcolor{Red}{Add references and fix this.}

With this trend of moving to push based streaming processing platforms for reducing the latency and to handle increasing throughput requirements, several distributed and single-node stream processing platforms were implemented and they are continue to evolve to tackle ever changing requirements. But due to different architectural decisions underlying these platforms and variations in problem classes and non-functional requirements these frameworks are trying to address, these frameworks behave differently under different types of data and algorithms. \textcolor{Red}{Need references.} Also these systems support different types of programming abstractions. These variations make the decision of selecting a specific platform hard. Although there are existing results on how these platforms perform individually \textcolor{Red}{(Need references.)}, but its hard to find comparative studies. In this paper, we present a comprehensive framework for analyzing and comparing distributed stream processing platforms. Further, we implement benchmark algorithms from our framework on several popular stream processing platforms and present our findings evaluating their performance, scalability, fault tolerance and ease of use.

We envision that, having defined set of benchmark applications and a benchmarking process will help stream analysts and developers who requires to use these systems to make informed decisions. However functional and non-functional characteristics  of stream processing systems depends mainly on nature of streaming data, streaming analysis algorithm, stream processing platform and the other supporting infrastructure used (**Need references to validate these claims**). Due to amount of various combinations in supporting infrastructure, we propose to keep the infrastructure differences minimal across all the stream processing platforms. So our performance evaluation framework is defined across following dimensions.

**Type of Streaming Data** - Event stream processing has uses in various different fields like Internet of Things, real-time analytics, fraud detection and trading. Each of these applications generate various types of data streams from time series data to streaming graphs. Then dimensionality of a tuple in these streams can varies from one to more than hundred (**Need references**).

**Stream Processing Tasks** - According to \cite{streamdrill:presentation}, we can categorize stream processing tasks on two dimensions, complexity of task and latency requirements of task. For example counting, averages and count distinct can be very simple where as fraud detection, outlier detection or predictions based on streams of data can be complex. On the other hand reporting and monitoring applications may not have strong latency requirements. Even 1 minute delay would be fine in most cases. But applications which controls some machine or device based on streaming data may require sub millisecond or sub second latency requirements.

**Stream Processing Platform** - There are various different stream processing platforms used to address variety of functional and non-functional requirements. *Need information about different types of platforms available today with brief description about their differences. Good way is to define categorization of these platforms*

Linear Road \cite{arasu2004linear} benchmark proposed almost a decade ago is still the de-facto standard for benchmarking stream processing applications. **Need to say this not enough to cover todays platforms and current distributed platforms use different set of benchmarks. Also the fact that use of benchmarks in publications is just for showing their platform is better at something. These are not strong, unbiased comparisons.**


Described what we are trying to don in this paper.

Then our contributions.

# Benchmarking Framework and Methodology

## Benchmarks

**TODO* - Find streaming algorithms and references to their applications. The question is can we only use queries defined in \cite{mendes2009performance}

- Keen.io like ingest use case
- Twitter Top-K hash tags


# Experiment Setup

In this section we describe the implementation of benchmarking framework we defined in Section \ref{benchmarking-framework-and-methodology}. Rest of the section describes how stream processing platforms are selected, hardware and software configurations, stream processing algorithm related configurations and details on tuning specific stream processing platform.

Across all the stream processing platforms, we use Kafka as the data stream source and all the incoming messages are thrift encoded except for cases we have specifically mentioned. *If we need to convince the use of Kafka, [this](https://cwiki.apache.org/confluence/display/KAFKA/Powered+By) is a good resource.
