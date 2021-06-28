---
layout: post
title:  "distribute message queue"
tags:
 - design
---


## requirements

Functional
- sendMsg(messageBody)
- receiveMessage()

Non-functional
- scalable(handles load increases, more queues, and messages)
- highly avaiable(survive hardware/network failures)
- performant(single digit latency for main operations)
- durable(once submitted, data is not lost)

## High-level architecture

![image](https://user-images.githubusercontent.com/900639/123561015-d2499c00-d75a-11eb-9c83-28613d939fe2.png)

## VIP and Load balancer
VIP can be SPOF. so VIP partitioning is required.

![image](https://user-images.githubusercontent.com/900639/123561052-1d63af00-d75b-11eb-8788-d3c35f2371e6.png)

## FrontEnd Service
- a lightweight web service
- stateless service deployed across several data centers

Functions
- request validation
  - required parameters are present
  - data falss within an acceptable range
- Authentication/Authorization
  - validating identity of a user of a service
  - 
- TLS(SSL) termination
  - SSL on the load balancer is expensive
  - termination is 
- Server-side encryption
- Caching
- Rate limiting(Throttling)
  - leaky bucket algorithm
- request dispatching
  - circuit breaker pattern prevents an application from repeately trying to execute an opertion that will be likely to fail
  - bulkhead pattern helps to isolate elements of an application into pools so that if one fails, the other will continue to function.
- request depulication
  - may occur when a successful sendMessage fails to reach a client.
- usage data collection
  - billing/ realtime usage

## Metadata service
- caching layer between frontend and a storage
- many read, little writes
- strong consistency storage preferred

![image](https://user-images.githubusercontent.com/900639/123562894-58b7ab00-d766-11eb-825c-a607022ddf6f.png)

## backend service

- where and how do we store message? -> RAM and local disk
- how do we replicate data?
- how does FrontEnd select a backend host to send data to? Metadata service
- how does frontend know where to retrive data from? Metadata service

### Option A: Leader-follower relationshiop
![image](https://user-images.githubusercontent.com/900639/123564263-9cfa7980-d76d-11eb-93f6-cae05e3e7262.png)

### OPtion B:
![image](https://user-images.githubusercontent.com/900639/123564280-b3083a00-d76d-11eb-9ede-73dc47643b62.png)

comparions OPtion A/B :

|  | in-cluster manager | out-cluster manager  |  |  |
|-|-|-|-|-|
|  | manages queue assignment within the cluster  | managers queue assignment among clusters  |  |  |
|  | maintains a list of hosts in the cluster | maintains a list of cluters|  |  |
|  | monitors heartbeats from hosts | monitos each cluster health |  |  |
| | deals with leader and follower failures | deals with overheated clusters | |
| | split queue between cluster nodes(partitioning)| splits queue between clusters | |

## What else is important
- Queue creation and deletion
- message deletion
 - do not delete message. it can be deleted by batch job
 - consumer needs to call deleteMessae
- message replication
 -  async replication: low latency. how to sync when one host is down?
 -  sync replication: high latency. hit consistency
 -  hard to achieve `exactly once delivery`
-  push vs pull
-  FIFO. doesn't guarantee the strict order of the message
-  security: encrypte messages
-  monitoring

## final look
![image](https://user-images.githubusercontent.com/900639/123564724-5279fc80-d76f-11eb-8a57-129d319cb3ae.png)


## DB selection
https://www.youtube.com/watch?v=cODCpXtPHbQ

- Structured data
  - yes
    - Need ACID
      - RDBMS(yes): mysql, orcle, sql server, postgres
  - no
    - Ever increasing data && + finite queries -> Columnar DB: Cassandra, HBase
    - ++ Data types && ++ queries -> document DB: mongoDB, Couch Base

interesting example: At ecommerece site, we shouldn't sell more than the remaining quantity. it should support ACID. so we can use SQL before placing order. but once order is created, then you can use MongoDB to save the data.

## Reference


