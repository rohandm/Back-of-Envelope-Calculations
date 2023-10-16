## Feature Expectations
- Use cases and actors
- Usage patterns
- Assumptions (scenarios not covered, black boxes)
- Volume
- Latency
- Retention
## Estimations
- Throughput (qps/wps)
- Read:Write ratio
- Memory and Storage estimations (if asked)
- Server count (if asked)
## Design Goals and Considerations
- Prioritize consistency or availability.
- Latency and Througput.
- Concurrency control.
## High Level Design
- Database Schema
- APIs
- Basic algorithm
- High level design
## Deep Dive
- Identify bottlenecks
- Scaling algorithm
- Scaling individual components:
  - Availability, Consistency and Scale story for each component
  - Consistency and availability patterns
- Think about the following components, how they would fit in and how it would help
  - DNS
  - CDN [Push vs Pull]
  - Load Balancers [Active-Passive, Active-Active, Layer 4, Layer 7]
  - Reverse Proxy
  - Application layer scaling [Microservices, Service Discovery]
  - DB [RDBMS, NoSQL]
    - RDBMS 
      - Master-slave, Master-master, Federation, Sharding, Denormalization, SQL Tuning
    - NoSQL
      - Key-Value, Wide-Column, Graph, Document
      - Fast-lookups:
        - RAM  [Bounded size] => Redis, Memcached
        - AP [Unbounded size] => Cassandra, RIAK, Voldemort
        - [Unbounded size] => HBase, MongoDB, Couchbase, DynamoDB
    - Caches
      - Client caching, CDN caching, Webserver caching, Database caching, Application caching, Cache @Query level, Cache @Object level
      - Eviction policies:
                                >> Cache aside
                                >> Write through
                                >> Write behind
                                >> Refresh ahead
    - Asynchronism
      - Message & Task queues
      - Task queues
      - Back pressure
    - Communication
      - TCP, UDP, REST, RPC, Long polling, Websockets, Server sent events, 
## Justify
  - Throughput of each layer
  - Latency caused between each layer
  - Overall latency justification
## Logging and Monitoring
## Security
