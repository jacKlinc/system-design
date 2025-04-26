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

# Content Delivery Network (CDN)

# Availability

# Scalability

# Storage