# Chapter 1

  - [OSI Model](#osi-model)
  - [TCP and UDP](#tcp-and-udp)
  - [Domain Name System (DNS)](#domain-name-system-dns)
  - [Load Balancing](#load-balancing)
  - [Clustering](#clustering)
  - [Caching](#caching)
  - [Content Delivery Network (CDN)](#content-delivery-network-cdn)
  - [Proxy](#proxy)
  - [Availability](#availability)
  - [Scalability](#scalability)
  - [Storage](#storage)


# OSI Model

## Application
- Direct contact with user: email client or web browser
- HTTP/SMTP

## Presentation
- Also called the translation layer, it extracts and manipulates
- Transĺation, compression, encryption

## Session
- Responsible for opening and closing communications
- Ensures it stays open long enough to transfer all data but also that it gets closed in time

## Transport
- Responsible for E2E connection
- Takes data from session layer and breaks it up


# DNS
## Steps for DNS
1. Client goes to URL and the query arrives at the **DNS resolver**
2. The **resolver** recursively queries a **root nameserver**
3. The **root server** response with the Top-level Domain (**TLD**)
4. The **resolver** queries the **TLD** (.com, .ie, etc.)
5. The **TLD** responds with the domain's nameserver IP
6. The recursive **resolver** queries the domain's nameserver
7. The IP for the URL is returned to the **resolver** and then the client

## Server Types
- TODO

## Query Types
- TODO

## Record Types
- TODO

## Subdomains
- TODO

## DNS Zones
- TODO

## DNS Caching
- TODO

## Reverse DNS
- TODO

# Load Balancing
- Allows distribution of traffic across multiples resources ensuring **HA** ans reliability
- Load balancing at each layer is probably best

## Why?
- When there are thousands or millions of requests, servers need to be added
- Load balancers sit in front on the server keeping each server worked at a similar amount and redirects in the event of faulty or new servers 

## Distribution

Variations
- **Host-based**: distribute based on requested hostname
- **Path-based**: distribute based on entire URL-not just hostname
- **Content-based**: distribute based request content such as parameter

## Layers
They generally work at two layers

### Network
- Routes based on IP addresses and usually a hardware device
- Cannot do content-based routing

### Application
- Can perform content based routing and has a full understanding of the traffic
- Software

## Types

### Software
- Easier to deploy, more cost-effective and flexible
- More flexibility means more work to setup
- Usually available as a cloud service

### Hardware
- Physical, on-prem, expensive, high maintenance, fast

## Routing Algorithms

- **Round-robin**: equally distributed requests to servers in rotation
- **Weighted Round-robin**: Adds weighting to previous one such as compute and traffic handling
- **Least Connections**: the load balancer sends requests to the server(s) with the least amount of connections to the client
-  **Least response time**: sends requests to the server with a combined minimum of repsonse time and active connections
-  **Least bandwidth**: similar to least connections but using Mbps as the metric rather than number of connections
-  **Hashing**: key-based such as IP address or request URL


## Redundant Load Balancer

- Load balancers can fail too. Having a passive load balancer to kick in will make a system more **fault-tolerant**


## Desired Features
- **Autoscaling**
- **Sticky sessions**: assign client to the same resource in order to maintain session state
- **Healthchecks**: remove the resource from pool if it's performing poorly
- **Persistence connections**: server can open a connection that stays open (WebSocket)
- **Encryption**
- **Certificates**
- **Compression**: response compression
- **Caching**: Application layer load balancers can cache things
- **Logging**
- **Request tracing**:  assign unique id to each request (execution-id)
- **Redirects**
- **Fixed response**

## Examples
- AWS ELB
- Nginx

# Clustering
Two more computers that run in parallel with a common goal
- They need to be connected to a network than allows internode comms
- At least one node is the designated leader being entry point and delegating the work
- A cluster should operate as a single machine and the client should not know the difference

## Types
1. Highly available (**HA**) or fail-over
2. Load balancing
3. High-performance computing

## HA Config Types

### Active-Active vs Active-Passive
- Active-Active run at the same time and the work is divided evenly
- Active-Passive is where the passive node acts as a backup for the second

## Load balancing vs Clustering
- The servers in clusters are aware of each other and boosts redundancy, capacity and availability
- Load balancers only react to requests


## Challenges
- Installation and maintenance of the OS and it's dependencies

## Examples
- Containers: Kubernetes, Amazon ECS
- Databases


# Caching

"There are only two hard things in Computer Science: cache invalidation and naming things." - Phil Karlton

- Each block has an id of where the data was stored in the cache
- When searching, L1 is searched first and L2 after and then L3, L4

## Cache hits and misses

### Cold vs Warm vs Hot
- Hot cache: L1
- Warm cache: L2/3
- Cold cache: L3+
- Miss: search memory

## Cache Invalidation
- The system decides the entries are invalid and they are removed
- This happens when the data is modified

Three kings of caching systems:
1. Write-through cache: write to cache and storage simultaneously
   1. Pro: fast read, data consistency
   2. Con: slow write
2. Write-around cache: writes go directly to the storage
   1. Pro: can reduce latency
   2. Con: can increase cache misses due to the need for a cache miss for new data leading to slow reads
3. Write-back cache: write to cache and asynchronously write to storage on completion:
   1. Pro: reduced latency for write-intensive apps
   2. Con: risk of data loss in the event caching layer crash. Mitigated by cache replication

## Eviction policies

- FIFO
- LIFO
- Least Recently Used (LRU): discards least recently used
- Most Recently Used (MRU): discards most recently used
- Least Frequently Used (LFU): counts how often an item is used. Discards least used
- Random Replacement (RR): random replacement

### Distributed Cache

- Pools RAM over multiple nodes allowing them to access other computer's RAM
- Allows for memory limits to exceed that of a normal computer

### Global Cache

- There is only one cache for multiples nodes, as opposed to multiple RAM storage for multiple nodes


### Use Cases

- Database caching
- Content Delivery Network
- DNS caching
- API caching

**When not to**
- When caching takes as long as accessing the data source
- Low repetition because caching only reaps rewards when there is a repeated memory access pattern
- When data changes often

### Advantages

- Reduced latency
- Reduce database load
- Reduce network cost
- Increased read throughput

### Examples
- Redis
- Amazon Elasticache

# Content Delivery Network (CDN)

Group of servers that are geographically distributed to improved delivery

- Increases content availability and redundancy
- Reducing bandwidth
- Improves security

## How does it work?

- The origin server contains the master content while the edge servers are numerous and spread across an area
- To reduced latency, a cached version of the content is stored across these edges
- Once the static assets are loaded to the CDN servers, the origin is no longer used

## Types

### Push
- Receive content whenever changes occur on the server
- We are responsible for uploading directly to the CDN and rewriting URLs
- This minimises traffic but maximises storage
- Useful for sites with a small amount of traffic

### Pull
- Pulls new content from origin when the CDN does not have certain content
- This requires less maintenance 
- Useful for heavy traffic sites

## Disadvantages

- **Cost**: expensive, especially for high-traffic
- **Restrictions**: some orgs and countries block CDNs
- **Location**: if most of the users don't have a CDN in their area, it's not much use

## Examples

- Amazon CloudFront

# Proxy

- Used to filter and log requests, sometimes transforming them by removing headers or encrypting/decrypting

## Types

### Forward Proxy

- Sits in front of a group of client machines and relays their request to the web servers

**Advantages**
- Block access to certain content
- Facilitates geo restriction
- Anonymity
- Avoids browsing restrictions

### Reverse Proxy

- Sits in front of a group of web servers, relaying client requests to different servers

**Advantages**
- Security
- Caching
- SSL encryption
- Load balancing
- Scalability and flexibility

## Load Balancer vs Revers Proxy

- Load balances are only useful when we have multiple servers
- Proxies can work with one web server
- A reverse proxy can act as a load balancer but not vice versa

## Examples
- Nginx


# Availability

The time a system remains operational to perform its job in a specific period

## Nine's Availability

- Uptime (or downtime) as a percentage of time when the service is available

$$
Availability = \frac{Uptime}{(Uptime + Downtime)}
$$

- If availability, is 99.00% it's "two nines", 99.9% it's "three nines"

## Availability in Sequence vs Parallel

The overall availability of a system is dependent on whether in sequence or parallel

### Sequence

Overall availability reduces when components are in sequence

$$
Availability \space (Total) = Availability \space (Foo) * Availability \space (Bar)
$$

### Parallel

Overall availability increases when components are in parallel


$$
Availability \space (Total) = 1 - (1 - Availability \space (Foo)) * (1 - Availability \space (Bar))
$$

### Availability vs Reliability

- If a system is reliable, it is available
- If it is available, it's not always reliable


## High Availability vbs Fault Tolerance

- Fault tolerant mean a backup system is ready to go if the main breaks 
- High availability just means the main system will only be down for a certain percentage of time


# Scalability

# Storage