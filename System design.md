## Redundancy

**Build redundancy into your application, to avoid having single points of failure**

System is scalable (horizontally) when you can add a machine and it will increase capacity of the system overall.

Contrary if one machine is removed from such system the load will distribute across remaining machines. The important point - system should not fail if one of the machines is removed. This attribute is typically referenced to as redundancy.

To properly distribute the load we can utilize load balancers. They sit in front of the web servers and route requests depending on the defined rules and strategies. It could be round-robin, least connections, random, random with weighting.
#### References
- [Introduction to architecting systems for scale](https://lethain.com/introduction-to-architecting-systems-for-scale/)
- [Make all things redundant - Azure Application Architecture Guide | Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/guide/design-principles/redundancy)


## Replication
Replicas are just the copies that typically help to offload the read traffic.

**Replication allows to maintain durability and availability as errors occur.**

Replication doesn't come for free, there is a problem associated with data synchronization. It's called an eventual consistency. So it's a tradeoff between consistency on the one hand and latency and availability on the other.

For asynchronous or synchronous replication a **master-slave** scheme can be used. Changes can only be accepted by one replica (the master) or, in a **update anywhere** (_multi-master_) approach, every replica can accept writes.

RDBMs typically use eager replication, i.e. synchronous, to achieve consistency. NoSQL use lazy, i.e. asynchronous, replication.

Geo-replication can protect the system against complete data loss and improve read latency for distributed access from clients.

## High availability

Depending on the uptime in percentage of the system or component we can tell if it's highly available or not.

The main idea behind high availability is to identify single point of failure and eliminate it.

> E.g. if there is a server serving all the requests that goes down, the whole system gets unavailable. So we can add a redundant server to split the load and make sure system will still work even if one of them goes out of service. Load balancer in top of them will be responsible for distributing the load.
> But in this case load balancer becomes a single point of failure.

A single point of failure is any component that can cause a service interruption if it gets unavailable. Redundancy comes to the rescue when we need to overcome this issue. On top of that there is one more level that is responsible for monitoring health of system and taking actions when a component becomes unavailable.

By it's nature this additional level becomes by itself a single point of failure.
So better approach would be to introduce a cluster where the nodes are interconnected and are all capable of failure detection and recovery.

> In the case with the load balancer we can introduce another load balancer and organize them into a cluster. When one of them goes offline we can direct traffic to second one. Floating IP address can be used in such a schema. During DNS resolution domain will always resolve to the same IP, but the IP will be reassigned between load balancers depending on which one is available.

## Load Balancing

Say the user connects directly to the web server, at `domain.com`. If this single web server goes down, the user will no longer be able to access the website. In addition if multiple users are accessing service simultaneously they might experience slow response or might be unable to connect at all.

There is a way to solve this issue by adding multiple servers and putting a load balancer in front of them. The user accesses load balancer, which forwards request to one of the servers, which then responds directly to the user’s request.

Its purpose is two fold:
1. Distribute the load evenly
2. Make sure that user request is not sent to server which is down
### Health Checks
Load balancer constantly monitors the pull of servers by sending health check requests.

Health checks regularly attempt to connect to servers using the protocol and port defined by the forwarding rules to ensure that servers are listening
### Balancing algorithms
1. **Round Robin**
2. **Least Connections**
3. **Source** - load balancer will select which server to use based on a hash of the source IP of the request. User will consistently connect to the same server. Accessing the same server can also be achieved with **sticky sessions**.

### Redundant Load Balancers
Single load balancer can become a single point of failure. To mitigate that another load balancer can be added. In such setup they monitor health of each other. If one of them fails DNS must take users to the second one.
#### References
- [What is Load Balancing?](https://www.digitalocean.com/community/tutorials/what-is-load-balancing)
## Partition
**There are many ways to partition a system**. Databases are one obvious candidate for partitioning, but also consider storage, cache, queues, and compute instances.

- Partition a database to avoid limits on database size, data I/O, or number of concurrent sessions.    
- Partition a queue or message bus to avoid limits on the number of requests or the number of concurrent connections.

A database can be partitioned _horizontally_, _vertically_, or _functionally_.

- In **horizontal partitioning, also called sharding**, each partition holds data for a subset of the total data set. The partitions share the same data schema. For example, customers whose names start with `A–M` go into one partition, `N–Z` into another.
- In **vertical partitioning**, each partition holds a subset of the fields for the items in the data store. For example, put frequently accessed fields in one partition, and less frequently accessed fields in another. A vertical partition involves splitting a database table on columns. An example could be breaking a single User data table into several different tables like personal information and address/location data. A good signal that you should vertically partition a table is when you notice lots of queries only requesting a few of the columns at a time.
- In **functional partitioning**, data is partitioned according to how it is used by each bounded context in the system. For example, store invoice data in one partition and product inventory data in another. The schemas are independent.
### Why partition data?

-  **Improve scalability**. When you scale up a single database system, it will eventually reach a physical hardware limit. If you divide data across multiple partitions, each hosted on a separate server, you can scale out the system almost indefinitely.

-  **Improve performance**. Data access operations on each partition take place over a smaller volume of data. Correctly done, partitioning can make your system more efficient. Operations that affect more than one partition can run in parallel. 

- **Improve security**. In some cases, you can separate sensitive and nonsensitive data into different partitions and apply different security controls to the sensitive data.

- **Provide operational flexibility**. Partitioning offers many opportunities for fine-tuning operations, maximizing administrative efficiency, and minimizing cost.
> For example, you can define different strategies for management, monitoring, backup and restore, and other administrative tasks based on the importance of the data in each partition.

- **Match the data store to the pattern of use**. Partitioning allows each partition to be deployed on a different type of data store, based on cost and the built-in features that data store offers.
> For example, large binary data can be stored in blob storage, while more structured data can be held in a document database. See [Choose the right data store](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-overview).

- **Improve availability**. Separating data across multiple servers avoids a single point of failure. If one instance fails, only the data in that partition is unavailable. Operations on other partitions can continue.

### Sharding explanation

Sharding is a technique that allows splitting a big database into smaller ones, so each can store a subset of original data.

It allows replicating the schema across (typically) multiple instances or servers, using some kind of logic or identifier to know which instance or server to look for the data. An identifier of this kind is often called a "Shard Key".

This allows for applications to scale far beyond the constraints of a single traditional database.

> **Database Sharding vs Database Partition**
Partitioning is more a generic term for dividing data across tables or databases. 
Sharding is one specific type of partitioning, part of what is called horizontal partitioning.

> Important note about sharding, that makes it different from clustering or replication is that **shards know about each other**.

Databases are sharded for 2 main reasons:
	- **Replication**
	 - **Handling large amounts of data**

Sharding allows for replication because we can copy each shard of data onto multiple servers, which makes the application more reliable.
Big Data requires sharding for the simple fact that at large scale a single machine can't hold the entire dataset.

A side effect of having all those small servers means the app is more resilient to failure.

Because your data is broken into smaller pieces, queries only have to search smaller amounts of data. This speeds up database performance and response times.

**Sharding comes at a price, which is the additional complexity of dealing with having your data spread around all those servers.** Sharding should always be a last resort when it comes to scaling your database, other alternatives like read replicas and caching should be implemented first because they are much easier to implement. [Source](https://dev.to/renaissanceengineer/database-sharding-explained-2021-database-scaling-tutorial-5cej)

Keeping **data consistent** across nodes is one example of additional complexity that comes with sharding.

There are three basic distribution techniques
1. **Range**
2. **Hash**
3. **Entity group**

#### 1. Range
To make efficient scans possible, the data can be partitioned into ordered and contiguous value ranges by **range-sharding**. However, this approach requires some coordination through a master that manages assignments. To ensure elasticity, the system has to be able to detect and resolve hotspots automatically by further splitting an overburdened shard.

#### 2. Hash
In **Hash-sharding** every data item is assigned to a shard server according to some hash value built from the primary key. The goal of hashing function is to split data evenly.

> `serverid = hash(id) % servers` -  this hashing scheme requires all records to be reassigned every time a new server joins or leaves, because it changes with the number of shard servers (servers).

This approach does not require a coordinator and also guarantees the data to be evenly distributed across the shards, as long as the used hash function produces an even distribution.

A downside of this strategy is the need to remap data to hash values when servers are added or removed. **For this reason elastic systems use consistent hashing instead.**

#### 3. Entity Group
It is a data partitioning scheme with the goal of enabling single-partition transactions on co-located data.

#### 4. Lookup service
This sharding strategy works by implementing a lookup table that sits in front of the sharded databases. The service tracks the current partitioning scheme and maps to the locations of each shard.
#### Notes:

1. **Carefully select the partition key to avoid hotspots**. If you partition a database, but one shard still gets the majority of the requests, then you haven't solved your problem. Ideally, load gets distributed evenly across all the partitions. For example, hash by customer ID and not the first letter of the customer name, because some letters are more frequent. The same principle applies when partitioning a message queue. Pick a partition key that leads to an even distribution of messages across the set of queues. For more information, see [Sharding](https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding).
2. It’s possible to have a small number of power users who create much, much more data than others. Sometimes it's reasonable to allocate them a shard of their own. Random assignments are usually easiest, but are by no means the only option. For example users can be assigned to shards randomly - except for user with `ID: 367823`, who is assigned to shard `15`, which no other user is assigned to. [Source: Systems design for advanced beginners | Robert Heaton](https://robertheaton.com/2020/04/06/systems-design-for-advanced-beginners/)
#### References
- [Partition around limits - Azure Application Architecture Guide | Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/guide/design-principles/partition)
- [Data partitioning guidance - Azure Architecture Center | Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning)
- [Database Sharding Explained- 2021 Database Scaling Tutorial](https://dev.to/renaissanceengineer/database-sharding-explained-2021-database-scaling-tutorial-5cej)
- [NoSQL Databases: a Survey and Decision Guidance](https://medium.baqend.com/nosql-databases-a-survey-and-decision-guidance-ea7823a822d)
- [Sharding pattern - Azure Architecture Center | Microsoft Learn](https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding)

