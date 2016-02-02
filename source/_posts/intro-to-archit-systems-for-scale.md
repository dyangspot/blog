title: "Introduction to Architecting Systems for Scale"
date: 2016-01-28 16:11:47
tags: System Design
---

## Load Balancing: Scalability & Redundancy

Redunancy: system isn't disrupted by loss of a server.

Large system balance load at 3 layers:
- user to web servers
- web servers to an internal platform layers
- internal platform layer to database

Question:  [Load Balance] What are the three ways to implement load balancing? 
## Smart Clients
What is smart client? 
a client detectes downed hosts and avoids sending requests.

## Hardware Load Balancers

## Software Load balancers

## Caching 

Caching will enable you to make vastly better use of the resources you already have.
Consists of: 
- 1. precalculating results. 
- 2. pre-generating expensive indexes. suggested stories based on user's click history. 
- 3. stroing copies of frequently accessed data in a fast backend. e,g memcached 
## Application Versus Database Caching

Two primary approaches: 1. application caching 2. databse caching(most systems heavily use)
### Application caching
requires explicit integration in the code. 
1. checking if a value is in the cache; 
2. if not, retrieve the value from the database
3. then write the value into the cache. 
LRU caching algorithm.

### Database caching
good side: your code don't need change.

### In Memory Caches
e.g Memcached and Redis
LRU(least recently used) 

### Content Distribution Networks
CDNs take the burden of serving static media off of your application servers .
1. a request will first ask your CDN for a piece of static media
2. the CDN will serve that content if it has it locally availble
3. if isn't available, the CDN will query your servers for the file and then cache it locally; and then serve it to the requesting user.

### Cache Invalidation 

Maintain consistency between your caches and the databse.
write-through cache: each time a value changes, write the new value into the cache, or simply delete the current value from the cache and allow a read-through cache to populate it later. 

### Off-Line Processing
because of unacceptable latency. or because needs to occur periodically

### Message Queues
processing inline request is too slow. add a message queue. e.g RabbitMQ -> quickly publish messages to the queue.

another benefit: allow you to create a separate machine poool for performing off-line processing rather than burdening web servers.


### Scheduling Periodic Tasks

Cron: job scheduler in unix system.
### Map-Reduce
large quantity of data. 

### Platform Layer

Reasons to add: your web app communicate with a platform layer which in turn communicates with your database.
Pros:
1. Separate the web applicaton and the platfrom allow to scale the pieces independently. 
2. resue your infrastructure for multiple products or interfaces: a web application, an API, iPhone app.
3. Easier to scale an organization. 
