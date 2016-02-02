title: "scalability-availability-stability-patterns"
date: 2016-02-01 16:37:20
tags: System Design
---

## Trade-offs

- Performance vs Scalability
- Latency vs Throughput
- Availability vs Consistency
## Performance vs Scalability
Performance problem? system is slow for a single user
Scalability problem? system is fast for a single user but slow under heavy load.

## Latency vs Throughput
strive for *maximal* throughput vs *acceptable* latency

## CAP Theorem
pick 2 of them from Consistency, Availability, Partition tolerance at
a given point in time.

Example1. Centralized System(RDBMS etc):  don't hve network partitons. e.g P in CAP => get Availability and Consistency.
Example2. Distributed System: will have network partitions.
 => only pick one between Availability and Consistency
 
## Availability Patterns
- Fail-over
 normal state -> failure occurs=> Failover state -> service restored. 
- Replication
	- Master-Slave
	- Tree replication
	- Master- Master
	- Buddy Replication 
Active replication - Push
Passive replication - Pull

## Scalability Patterns: State
- Partitioning
- HTTP Caching
- RDBMS Sharding
- NOSQL
- Distributed Caching
- Data Grids
- Concurrency
### HTTP Caching (p56,p57)
借助 Reverse Proxy: Varnish, Squid, rack-cache, Pound, Nginx, Apache mod_proxy, Traffic Server
借助CDN，
Proecompute Content: Homegrown + cron orQuartz, Spring Batch, Gearman, Hadoop, Google Data Protocol, Amazon Elastic MapReduce

## Service of Record
- Relational Database
scale: Partitioning & Replication
- NoSQL Database
1. key-Value db (Voldemort, Dynomite)
2. Column db (Cassandra, Vertica, Sybase IQ)
3. Document db (MongoDB, CouchDB)
4. Graph db (Neo4J, AllegroGraph)
5. Datastructure db (Redis, Hazelcast)
NoSQL in the wild: 
Google: Bigtable
Amazon: Dynamo
Amazon: SimpleDB
Yahoo: HBase
Facebook: Cassandra
Linkedin: Voldmort

### Chord & Pastry
- Distributed Hash Tables(DHT)
- Scalable
- Partitioned
- Fault-tolerant
- Decentralized
- Peer to peer
-  Popularized 1. Node ring 2. Consistent Hashing
	Node ring with consisting hashing(p80): lookup log(N) jumps
### Distributed Caching
- Write- through (p86)
- Write- behind (87)
- Eviction Policies
	1. TTL(time to live)
	2. Bounded FIFO
	3. Bounded LIFO
	4. Explicit cache invalidation
- Replication
- Peer- To- Peer(P2P)	
#### Distributed Caching Products
- EHCache
- JBoss Cache
- OSCache
- Memcached: 1. Very fast 2. Simple 3. key-value(string -> binary) 4. clients for most languages 5. Distributed 6. Not replicated 

### Concurrency(p96)
- Shared-State Concurrency(locks)
- Message- Passing Concurrency(Actor) 
- Dataflow Concurrency
- Software Transactional Memory