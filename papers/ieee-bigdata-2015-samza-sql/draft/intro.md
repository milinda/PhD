# Introduction

Different targets for the paper:

1. SQL layer over Samza 
2. As a enabler for Kappa architecture or LinkedIn's Liquid
3. How to do the evaluation.
    - Compare performance against Java based applications
    - Talks about the usability improvements (Anyone who knows SQL can write a query without the knowledge of Java or Scala)
    - Quickly prototype analysis logic
    - Easy access to static/relational storage

In today’s data-driven economy, enterprises – whether big or small – depends on data analytics to provide better services as well as to carry out most of the day to day business functions. Driven by unprecedented growth of mobile and Internet of Things (IoT) related technologies, ever increasing rate of data growth has changed how enterprises manage and interact of data. As a result Big Data related technologies such as Hadoop became accessible to broad spectrum of users -- from small to big enterprises -- enabling analysis of petabytes of data on commodity clusters.

Often these massive quantities of data is created by events occuring thousands to tens of thousands of times per second as a result of web interactions, remote sensors reacting to change of environment, etc. With these data generation rates we not only talks about volume of data, we also talks about velocity of data. And as the data-driven economy evolves, enterprises also realized the importance of interacting with the data (raw events) as it arrives to create new business oppertunities. 

Couple of years ago, it was impracticle to act on high volumes of data as it arrives due to higher latencies of Big Data processing tools and cost of specialized software and hardware. As a result, distributed message queues and stream processing systems like Kafka, Apache S4, Apache Storm and Apache Samza which are robust and scalable enough to handle massive volumes of fast moving data were invented.These systems allows us to act on data as it arrives, generating value to both organisations and users which wasn’t possible half a decade ago at this scale.

-- 11:23AM, 05-23-2015

As people realize the value of data in motion is equal or greater than the data at rest, new scalable and fault-tolerant data management architectures such as \textit{Lambda Architecture (LA)} was proposed tackle wide range of requirements from low latency analytics to high latency analytics. While there are several imlementaitons of LA \cite{boykin2014summingbird} \cite{metamarkets:radstack}, opponents of Lambda Architecture argued \cite{questions-la-kreps} that its possible to achieve same using only a stream processing system by retaining the input data unchanged as proposed in LA and reprocessing by increasing the parallelism and replaying historical data as fast as possible. The main advantage of this new way of processing data is that you don't have to manage two entirely different systems (a batch processing system such as Hadoop and stream processing system such as Storm or Samza). Fernandez et al. recently presented in \cite{fernandezliquid} an improved version of this new architecture deployed at LinkedIn which is built on top of Apache Samza and Apache Kafka.  

Instead of widely used and standard declarative language like SQL, implementations of architectures mentioned above provide APIs in languages like Java or custom query languages specific for the framework. But custom programs written for specific purposes add maintenance overhead and often hard to reuse. And custom query languages doesn't have the advantages of extensions (for stream processing specific scenarios) to standard SQL such as
• easy to learn if you know SQL
• clear semantics

And also its worth noting that often these fast data processing pipelines access data at rest that can be mapped to relations and use of SQL allows us to combine stored data expose to us as relations with streams in a single query.

In this paper, we present SamzaSQL, an open-source SQL based streaming query engine implementation with support for interacting with non-streaming data sources on top of Apache Samza. SamzaSQL supports queries expressed in standard SQL with minor extensions to add stream produc- ing and windowing capabilities. These queries get compiled into streaming jobs executed in Samza. Depending on the query, these streaming jobs may involve access to static data sources such as stored relations and may access outputs from recurring batch jobs. SamzaSQL front-end, which does query parsing, validation and planning is built on top of Apache Calcite and supports querying stream data formats such as Avro or JSON. SamzaSQL also includes support for pluggable catalogs or schema registries based on Cal- cite’s schema factory functionality. This allows us to extend SamzaSQL to support various data sources with custom formats. SamzaSQL’s streaming query execution layer is implemented based on CQL [3] continuous query execution model.

Until now, we have used custom programs or data process- ing pipelines built on APIs provided by streaming and batch data processing frameworks or programming models such as Summingbird [1] to build Lambda or Kappa architecture style applications. General motivation of this work is to explore how we can provide a unified framework – which enables both Lambda Architecture and Kappa architecture style data processing pipelines based on well known SQL related standards and continuous query execution model presented in CQL [3].

Windowing is a core and central concept of stream query- ing due to the fact that operators like join and some of the aggregations cannot be evaluated over unbounded streams. SamzaSQL comes with a widow operator implementation with supports for
• timely and deterministic window output under unpre- dictable message delevery latencies
• and, deterministic window output with node failures and message re-delivery

TODO:
We are mainly targeting soft and near real-time systems. We need to describe this. Also we need to discuss about partition based parlelism supported.