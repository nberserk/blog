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

## token bucket algorithm



## Reference
