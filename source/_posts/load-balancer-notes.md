title: "load_balancer_notes"
date: 2016-01-25 11:58:56
tags: System Design
---

发送数据包过程中IP和MAC谁变谁不变？ 
src ip, dest ip 不变； 
发薪日 => 邮递员 => ... => 收信人的身份(Mac)一直在变

Load Balancer:  distribute workloads across multiple computing resources.

Load Balancer： 主+备

1. LB “对外”（external） 提供"virtual server"
2. LB "对内"（internal） 提供"virtual client"
3.  Packets转发： 源地址 <=> 目的地址解析 即Bi-directional Network Address Translation(BNAT = SNAT + DNAT)
4. based on service的 balancing(ip：port), instead of the Host(IP) => a server may have more than one service (HTTP: 80, HTTPS: 443, FTP: 21, DNS: 53)

### q1: Host is not working?
ping, time out. 
Health monitoring: 1. deamon,  ping. 2. "service ping"
### q2: How does the LB decide which server to send a connection request to ?
1. Configure each virtual server has a specific dedicated cluster of services with makes up all the available servers.
2. By *health monitoring* modifies list to make a list of *currently available servers*.
3. Load balancing algorithms to pick right the right server. e.g. round robin, random...
### How about *follow-on traffic* from a known connection? To load balance or not to load balance ?
1. TCP 长连接: Connection maintenance.
 Used for *long-lived* TCP connections
 - Keep track of open connections and the server service it belongs to.
 - Monitor the connection and update connection table when the connection closes.
  
2. TCP 短链接: Persistence (aka. Server Affinity)
Used for multiple short-lived TCP conenctions from the same server.
 困难，怎么判断是同一个用户。
- 可以通过源地址(source-address affinity)， 可能不是不准确，可能是同一个路由器下同一个子网，用同一个IP，无法区分. 也有可能是同一个用户在家里，公司，学校进行访问。 
- 用DPI（Deep Packet Inspection）技术来查看网络层以上的信息. 例如用户把信息存到cookie，通过查看cookie就能知道哪个用户在访问。

3. All above information is stored in "Flow Table".

 