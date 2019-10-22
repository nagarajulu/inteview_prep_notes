# Distributed Tracing, Monitoring, Logging

## Distributed Tracing

### Reference talks

* Canopy: [Facebook's distributed tracing framework](https://www.infoq.com/presentations/canopy-scalable-tracing-analytics-facebook/)


### Fundamentals

* Distributed tracing is key to identifying performance of our systems end to end.

* Typically have common trace request ID (eg: UUID) which is used by all participating systems

* Have some common basic data points apart of application specific / custom interests in performance
..* Trace ID
..* Host ID
..* Endpoint ID
..* Application ID
..* Latency/Duration

### Key components of tracing framework

* Network - layer used by collector (eg: most companies build custom framework on top of UDP)
* Collector - collects traces from all services (eg. Kafka streams)
* Store - store all traces (eg: Cassandra, ElasticSearch)
* Search - search trace (aggregate a trace from Store) (eg: API aggregator)
* Web UI - visualize trace  (Front End/UI)

### Protocols used

* Most rely on UDP rather than TCP for speed trace data collection.

### Widely used frameworks 

* [Zipkin](https://zipkin.io/)- twitter
* [Dapper](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/36356.pdf) - Google
* [Jaeger](https://www.jaegertracing.io/) - Uber 
* [Canopy](https://research.fb.com/wp-content/uploads/2017/10/sosp17-final14.pdf?)- facebook
* [opentracing.io](https://opentracing.io/)

### Sampling

Many of the distributed tracing frameworks use Sampling method to trace requests. Sampling means we don't need to trace every request received; instead few of requests would be sampled for tracing.

### Sampling Strategies

1. Constant - trace all or none
2. Probablistic - random basis (eg/ 1 in 10)
3. Rate Limiting - rate of traces/second. (eg: 2 traces per second)
4. Remote / custom - consult agent or use adaptive sampling

Reference [Jaeger strategies](https://www.jaegertracing.io/docs/1.14/sampling/)

## Distributed Logging

* [Distributed Log at Twitter](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2015/building-distributedlog-twitter-s-high-performance-replicated-log-servic.html)

..* BookKeeper - LogStorage at Twitter - Stores them in Journal/Ledger format - append-only


## Monitoring tools

* [Graphite](http://graphiteapp.org/)