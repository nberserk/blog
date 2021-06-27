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


