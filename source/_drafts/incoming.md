---
title: incoming
tags:
---

# news feed system

http://blog.gainlo.co/index.php/2016/03/29/design-news-feed-system-part-1-system-design-interview-questions

http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html

[big data in realtime at Twitter](https://www.slideshare.net/nkallen/q-con-3770885)

[Designing Instagram](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5673385510043648)

- data model
  - follower, user, feed,
- ranking
- publishing

# normalize/denormalize
- OLTP(Online Transaction Processing) : normalize
- OLAP(Online Alalytic Processing) : de-normalize

# Knapsack



# transaction across multiple data centers

- weak consitency: real time game, voip, 
- eventual consistency: S3, 
- strong consistency : after write, read can see it

backup -> M/S -> MM -> Paxos

# paxos

best explaination with images, http://harry.me/blog/2014/12/27/neat-algorithms-paxos/

http://blog.lastmind.io/archives/684

# simple garbage collector, mark and sweep

http://journal.stuffwithstuff.com/2013/12/08/babys-first-garbage-collector/
