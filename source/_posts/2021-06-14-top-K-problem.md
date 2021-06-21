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
![image](https://user-images.githubusercontent.com/900639/122692932-788e2280-d1ec-11eb-9cfb-0249ec71d401.png)

- build hashTable<Key, Count> 
  - sort the whole list: O(nLog(n))
  - use heap: O(nLog(k))

but it's not scalable.

## Hash table, multiple hosts
![image](https://user-images.githubusercontent.com/900639/122692782-d5d5a400-d1eb-11eb-9bf4-00d09955517a.png)

you can scale previous approach by using data paritioner. scalability and througput has been addressed.

But streaming data is not bounded. It has infinite data. what if we need to calculate top K for a day or a week?

## count-min sketch, multiple hosts
There is well-known data structure called count-min sketch, kind of approximation algorithm, which guarantees fixed memory usage. basically it tradedoff between accuracy and memory. 

![image](https://user-images.githubusercontent.com/900639/122708501-2c090e00-d211-11eb-93de-435e82166873.png)


count-min sketch uses 2 dimensional array, each row is different hash function, column is count. whenever new event comes in, calculate hash value for each row, and increment the value of the cell by 1. This means that it could have collision, that's why we take the smallest value as a result.

But this doesn't give us accurate top K lists. if constraints don't allow inaccuracy, then we need a different approach.

## high level design

![image](https://user-images.githubusercontent.com/900639/122709631-80ad8880-d213-11eb-8601-41b433e54634.png)





## Reference
- https://www.youtube.com/watch?v=kx-XDoPjoHw
- http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html





