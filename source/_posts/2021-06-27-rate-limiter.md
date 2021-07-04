---
layout: post
title:  "Rate limiting service"
tags:
 - design
---

## Requirements

functional:
- allowRequest(request)

non functiona:
- low latency
- accurate
- scalable
- ~~highly available~~ since we can assume that default is allow the request if the throttling service is not available.

## request processing
![image](https://user-images.githubusercontent.com/900639/124370124-a29e0680-dc28-11eb-9ca0-039aa7db62bd.png)


## token bucket algorithm
one of the popular algorithm for rate-limiting algorithm.

![image](https://user-images.githubusercontent.com/900639/124370140-c8c3a680-dc28-11eb-914e-8541dcf0f632.png)

## interfaces and classes
- JobSceduler: fetch rules from RuleService periodically
- RulesCache: store token bucket objects
- ClientIdentifier: identify client form the request 
- RateLimiter: provide `allowRequest()` api

## Distributed world
as we move to distributed world, we need to share local remaining tokens to other hosts. 

### message boarcasting
possible approaches:
- Tell everyone everything: not scalable, message grows quadratically
- gossip communication
- distributed cache cluster
- coordination service: one host takes coordination leader role. Paxos, Raft
- random leader selection

![image](https://user-images.githubusercontent.com/900639/124373547-eeac7380-dc47-11eb-8fb3-1b6be446ed7f.png)

### TCP VS UDP
TCP: slow, accuracy, order guaranteed
UDP: fast, not reliable, order not guaranteed

### How to integrate all this with the service?

![image](https://user-images.githubusercontent.com/900639/124373585-46e37580-dc48-11eb-9efe-b70fef4cb38c.png)

## final look
![image](https://user-images.githubusercontent.com/900639/124373721-adb55e80-dc49-11eb-8643-d77ec2b938c3.png)



## Reference
- https://www.youtube.com/watch?v=FU4WlwfS3G0&t=815s
