http://www.mitbbs.com/article_t/JobHunting/32777529.html
http://www.learn4master.com/interview-questions/system-design/system-design面试

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


# Web Site Scalability
<p align="center">
  <img src="http://2.bp.blogspot.com/_j6mB7TMmJJY/R9F3_4qXpgI/AAAAAAAAAAs/-qYU9ZKhEpo/s320/scalable.png"/>
  <br/>
  <i><a href=http://robertgreiner.com/2014/08/cap-theorem-revisitedhttp://horicky.blogspot.com/2008/03/web-site-scalability.html>Web Site Scalability</a></i>
</p>

A classical large scale web site typically have multiple data centers in geographically distributed locations. Each data center will typically have the following tiers in its architecture
Web tier : Serving static contents (static pages, photos, videos)
App tier : Serving dynamic contents and execute the application logic (dynamic pages, order processing, transaction processing)
Data tier: Storing persistent states (Databases, Filesystems)

## Content Delivery
#### Dynamic Content
it is possible to pre-generate dynamic content and store it as static content. When the real request comes in, instead of re-running the application logic to generate the page, we just need to lookup the pre-generated page, which can be much faster
#### Static Content
CDN is an effective solution for delivering static contents. CDN provider will cache the static content in their network and will return the cached copy for subsequent HTTP fetch request. 

## Session state handling
#### Memory-based session state with Load balancer affinity
One way is to store the state in the App Server's local memory. But we need to make sure subsequent request land on the same App Server instance 
#### Persist session state to a DB
accessed by any App Server inside the pool
#### On-demand session state migration
Under this model, the cookie will be used to store the IP address of the last app server who process the client request
When the next request comes in, the dispatcher is free to forward to any members of the pool. The app server which receive this request will examine the IP address of the last server and pull over the session state from there.
#### Embed session state inside cookies
