# Introduction

Different targets for the paper:

1. SQL layer over Samza 
2. As a enabler for Kappa architecture or LinkedIn's Liquid
3. How to do the evaluation.
    - Compare performance against Java based applications
    - Talks about the usability improvements (Anyone who knows SQL can write a query without the knowledge of Java or Scala)
    - Quickly prototype analysis logic
    - Easy access to static/relational storage

In today’s data-driven economy, organisations – whether big or small – depends on data analytics to stay competitive. Advancements in Big Data related technologies such as Hadoop have transformed how organisations interact with data by enabling analysis of petabytes of data using com- modity clusters. Often these massive quantities of data is created by events occuring thousands to tens of thousands of times per second. We these data generation rates we not only talks about volume of data, we also talks about velocity of data. To get the most out of this data we have to act on this data as it arrives. Couple of years ago, it was impracticle to act on data as it arrives due to higher latencies of Big Data processing tools and cost of specialized software and hardware.

As a result, message queues and stream processing sys- tems like Kafka, Apache S4, Apache Storm and Apache Samza which are robust, fault tolerant and scalable were invented to take most out of the fast moving data. These systems allows us to act on data as it arrives, generating value to both organisations and users which wasn’t possible half a decade ago at this scale.

While it is important to act on data as it arrives, it is also important for these fast data processing pipelines to interact with static data or output data from batch jobs for the purpose of data enrichment or near real-time analysis. Reference architecture known as Lambda Architecture was proposed as a solution to this hybrid analytics requirements and there are implementations of this architecture such as Summingbird [1] and RADStack [2] in use today. Instead of widely used declarative language like SQL, these frame- works including Summingbird provide low level APIs in languages like Java or custom query languages specific for the framework. But custom programs written for specific purposes add maitenance overhead and often hard to reuse. And custom query languages doesn’t have the adavantages of extensions to standard SQL such as
• easy to learn if you know SQL
• clear semantics
• allows combining stored relaitons and streams in a single query

In this paper, we present SamzaSQL, an open-source SQL based streaming query engine implementation with support for interacting with non-streaming data sources on top of Apache Samza. SamzaSQL supports queries expressed in standard SQL with minor extensions to add stream produc- ing and windowing capabilities. These queries get compiled into streaming jobs executed in Samza. Depending on the query, these streaming jobs may involve access to static data sources such as stored relations and may access outputs from recurring batch jobs. SamzaSQL front-end, which does query parsing, validation and planning is built on top of Apache Calcite and supports querying stream data formats such as Avro or JSON. SamzaSQL also includes support for pluggable catalogs or schema registries based on Cal- cite’s schema factory functionality. This allows us to extend SamzaSQL to support various data sources with custom formats. SamzaSQL’s streaming query execution layer is implemented based on CQL [3] continuous query execution model.

Until now, we have used custom programs or data process- ing pipelines built on APIs provided by streaming and batch data processing frameworks or programming models such as Summingbird [1] to build Lambda or Kappa architecture style applications. General motivation of this work is to explore how we can provide a unified framework – which enables both Lambda Architecture and Kappa architecture style data processing pipelines based on well known SQL related standards and continuous query execution model presented in CQL [3].

Windowing is a core and central concept of stream query- ing due to the fact that operators like join and some of the aggregations cannot be evaluated over unbounded streams. SamzaSQL comes with a widow operator implementation with supports for
• timely and deterministic window output under unpre- dictable message delevery latencies
• and, deterministic window output with node failures and message re-delivery

TODO:
We are mainly targeting soft and near real-time systems. We need to describe this. Also we need to discuss about partition based parlelism supported.