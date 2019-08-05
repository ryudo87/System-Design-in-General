# System-Design-in-General
## How to approach a system design interview question

### Step 1: Outline use cases, constraints, and assumptions

Gather requirements and scope the problem.  Ask questions to clarify use cases and constraints.  Discuss assumptions.

* Who is going to use it?
* How are they going to use it?
* How many users are there?
* What does the system do?
* What are the inputs and outputs of the system?
* How much data do we expect to handle?
* How many requests per second do we expect?
* What is the expected read to write ratio?

### Step 2: Create a high level design

Outline a high level design with all important components.

* Sketch the main components and connections
* Justify your ideas

### Step 3: Design core components

Dive into details for each core component.  For example, if you were asked to [design a url shortening service](solutions/system_design/pastebin/README.md), discuss:

* Generating and storing a hash of the full url
    * [MD5](solutions/system_design/pastebin/README.md) and [Base62](solutions/system_design/pastebin/README.md)
    * Hash collisions
    * SQL or NoSQL
    * Database schema
* Translating a hashed url to the full url
    * Database lookup
* API and object-oriented design

### Step 4: Scale the design

Identify and address bottlenecks, given the constraints.  For example, do you need the following to address scalability issues?

* Load balancer
* Horizontal scaling
* Caching
* Database sharding

------------ ACE A SYSTEMS DESIGN INTERVIEW---------------------------






---------------------------------------
## Availability vs consistency

### CAP theorem

<p align="center">
  <img src="http://i.imgur.com/bgLMI2u.png"/>
  <br/>
  <i><a href=http://robertgreiner.com/2014/08/cap-theorem-revisited>Source: CAP theorem revisited</a></i>
</p>

#### AP - availability and partition tolerance

AP is a good choice if the business needs allow for [eventual consistency](#eventual-consistency) or 
when the system needs to continue working despite external errors.

## Consistency patterns
### Weak consistency

After a write, reads may or may not see it.  A best effort approach is taken.

This approach is seen in systems such as memcached.   real time use cases such as VoIP, video chat, and realtime multiplayer games.  

### Eventual consistency

After a write, reads will eventually see it .  Data is replicated asynchronously.

DNS and email.  Eventual consistency in highly available systems.

### Strong consistency

After a write, reads will see it.  Data is replicated synchronously.

file systems and RDBMSes.  Strong consistency works well in systems that need transactions.

* **Availability** vs **consistency**

## 1) Availability patterns

two main patterns to support high availability: **fail-over** and **replication**.

### Fail-over

#### Active-passive (master-slave failover)

With active-passive fail-over, **heartbeats** are sent between the active and the passive server on **standby**.  
If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.

The length of downtime is determined by whether the passive server is already running in 'hot' standby 
or whether it needs to start up from 'cold' standby.  Only the active server handles traffic.


#### Active-active (master-master failover)

In active-active, both servers are managing traffic, spreading the load between them.

If the servers are public-facing, the DNS would need to know about the public IPs of both servers.  
If the servers are internal-facing, application logic would need to know about both servers.


### Disadvantage(s): failover

* more hardware and additional complexity.
* There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.




### 2) Replication

#### Master-slave and master-master

* [Master-slave replication](#master-slave-replication)
* [Master-master replication](#master-master-replication)



