---
layout: post
title:  "Top K problem"
tags:
 - system design
---

## Requirements
functional
- topK(k, startTime, endTime)
- scalable
- available
- highly performant: few ten milliseconds to return top 100 list
- accurate

## single host approach

- build hashTable<Key, Count> 
  - sort the whole list: O(nLog(n))
  - use heap: O(nLog(k))

but it's not scalable.

## Hash table, multiple hosts
you can scale previous approach by using data paritioner. scalability and througput has been addressed.

But streaming data is not bounded. infinite. 



## Reference
- http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html





