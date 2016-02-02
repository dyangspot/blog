title: "scalability-for-dummies"
date: 2016-01-25 20:27:58
tags: System Design
---
### Clones 
Clones： 每个server都有相同的codebase， 本地都不存用户数据，session, profile pic. 
Session可以保持到centralized data store， 
存在形式： external persistent cache or db，cache better performance。
*external* means outside application server.
AWS: create an image based on other server.

### Database

- path1: mast-slave replication, +RAM, sharding, denormalization, sql tuning
- path2: NoSQL

### Cache
key-value store. buffer layer between application and data storage
refer to in-memory cache, e.g. Memcached or Redis *never do file-based caching*
#### 2 patterns of caching data
- Cached Database Queries.
Hashed query as a key, result as value.
main issue: expiration. hard to delete a cached result when cached a complex query.
- Cached Objects.
stored the complete instance of the class or the assembed dataset in the cache ?
best part: makes asyn processing possible.
object to cache:
user sessions(never use database!)
fully rendered blog articles
activity streams
user<->friend relationships
Tools: 
Redis: can replace database
Memcached: just need cache, scale like a charm
### Asynchronism

 

