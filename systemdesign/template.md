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
- Golden rules
  - Read-heavy system - Cache
  - Low latency - Cache, CDN
  - Write-heavy system - Message Queue or Append only logs
  - ACID compliance - RDBMS
  - Non ACID - NoSQL
  - Store videos, images, files - Object/Blob storage
  - Complex/heavy pre-computation like News feed - Message Queue & Cache
  - Search data - Search index, Tries or Search engine
  - Scale RDMS - Sharding and Partitioning
  - High availability, performance and throughput - Load balancer
  - Global reach, reliability. High availability, low latency - CDN
  - Data with nodes, edges and relationships (social network) - Graph DB
  - Improve database performance - Indexes
  - Bulk job processing - Batch processing and Message queues
  - Prevent DDOS attacks and reduce server load - Rate limiter
  - Microservices - API Gateway (Authentication, SSL termination, Routing)
  - SPOC - Add redudancy
  - Fault tolerance and durable - Data replication
  - Bi-directional communication - Websockets
  - Detect failures - Heartbeat
  - Data integity - Checksum algorithm
  - Add/Remove nodes efficiently with no hotspots - Consistent hashing with virtual nodes
  - Transfer data between servers in decentralized way - Gossip protocol
  - Location data - Quadtree or Geohash
  - High availability and Strict consistency cannot go hand in hand
  - Use Pagination and Gzip to limit response size
  - Preferred LRU eviction policy for Cache
 
## Justify
  - Throughput of each layer
  - Latency caused between each layer
  - Overall latency justification
## Logging and Monitoring
## Security
