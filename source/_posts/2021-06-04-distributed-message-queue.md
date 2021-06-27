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


