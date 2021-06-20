---
layout: post
title:  "distribute message queue"
tags:
 - design
---

## DB selection
https://www.youtube.com/watch?v=cODCpXtPHbQ

- Structured data
  - yes
    - Need ACID
      - RDBMS(yes): mysql, orcle, sql server, postgres
  - no
    - Ever increasing data && + finite queries -> Columnar DB: Cassandra, HBase
    - ++ Data types && ++ queries -> document DB: mongoDB, Couch Base

interesting example: At ecommerece site, we shouldn't sell more that the remaining quantity. it should support ACID. so we can use SQL before placing order. but once order is created, then you can use MongoDB to save the data.

