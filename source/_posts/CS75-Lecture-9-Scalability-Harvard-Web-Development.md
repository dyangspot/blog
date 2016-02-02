title: "CS75_Lecture_9_Scalability_Harvard_Web_Development"
date: 2016-01-25 17:29:55
tags:
---

### Scale Purpose: Serve more users
### Vertical Scaling:
- CPU
- RAM
- Disks
[硬盘、内存与闪存](http://mp.weixin.qq.com/s?__biz=MzA5NTcyMjg1Nw==&mid=402215713&idx=1&sn=c32644feddd90cefa498f7f66da9a78a&scene=5&srcid=01236siTCwxNurd5S71Y1s0K#rd)
 
 Why not always work? real world constrains
 
### Horizational scaling
Multiple servers. 
Distribute request 
Load Balancer : Round Robin
DNS（Domain Name System）: domain -> ip

```
nslookup google.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
Name:	google.com
Address: 173.194.123.41
Name:	google.com
Address: 173.194.123.33
Name:	google.com
Address: 173.194.123.37
Name:	google.com

```
Caching 
TTL: 
Sessions per server cause issue. => Share State => Share session

RAID: Save data
RAID0: parallel for performance
RAID1: mirror data
RAID5: 
RAID6
RAID10

Replication: 

Load Balancer:
Software: ELB, HAProxy, LVS
Hardware: Barracuda, F5, Cisco, Citrix

Sticky Sessions: Perserve session. visiting a website multiple times, still end up with same session object => same backend server.

Shared Storage: FC, iSCSI, MySQL, NFS,etc
Cookies: finite size(4k)
store id of server. 
Downside? 

Caching: 
Memcached
Cache getting big: expire and garbage collection. reuse space and memory. LRU

Facebook: Both read/write heavy. 

Replication: automatically copy something
Master-Slave. master copy down to slaves. 
    slaves: get copy every row of master db. master and slaves 	identical
upside: one master die, back up there becomes master
downside: take time to promote slave to master. 
Master-Master:

Partition: 

High Availablity:  



 
 
 
 