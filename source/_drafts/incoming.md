---
title: incoming
tags:
---

https://github.com/fishercoder1534/Leetcode

# Youtube

- http://highscalability.com/youtube-architecture
  - Apache, mysql with sharding, lighttpd for streaming
  - ptyhon code
  - traffic에 우선순위를 매겨, 비디오 요청을 먼저 기타 소셜관련 기능을 나중에 두게 디자인 했음.
  - 많이보는 비디오는 CDN을 사용
- http://highscalability.com/blog/2012/3/26/7-years-of-youtube-scalability-lessons-in-30-minutes.html
  - 커맨트의경우 3-400 미리 정도 늦게 싱크되는데, 커멘트를 쓴 사람에게는 잘 보여지게 하고, 다른 사람에게는 늦게 보여준다. 그렇게 해도 큰 불편함이 없으니까. cheat little. 재밌는 케이스.
  - 뷰카운트 등을 싱크해서 사용하려면 costly한데, 하루에 한번 정산하고 그 값을 머신간 싱크하지는 않고 대략적으로 증가시켜서 보여준다.
  - 

# consistent hashing
- http://highscalability.com/blog/2018/6/18/how-ably-efficiently-implemented-consistent-hashing.html




## 

# news feed system

http://blog.gainlo.co/index.php/2016/03/29/design-news-feed-system-part-1-system-design-interview-questions

http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html



[Designing Instagram](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5673385510043648)




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
