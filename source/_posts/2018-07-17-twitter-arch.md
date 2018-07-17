---
title: twitter architecture
date: 2018-07-17 22:09:06
tags: cloud architecture
---

http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html 여기 있는 글을 정리한 수준.


target TPS, read: 1000 tps write: 100 tps

## data model

- user 
    - id
    - name
    - email
- tweet
    - id
    - content
    - created
    - type: retweet|reply|
- follower N:M mapping
    - 이것은 [Flock](https://github.com/twitter-archive/flockdb)이라는 Graph DB를 사용했다고 함. following, follower, block 등의 정보 저장.
    - uid
    - follow_uid

## arch

- db는 nosql(Dynamo DB, mongo DB)를 쓰거나, mysql을 sharding해서.. sharding은 timestamp로 샤딩.
- 트위터는 write보다는 read요청이 훨씬 많으니 read에 최적화를 해야 함.
    - 대량의 redis cluster로 timeline을 캐쉬해 둔다.
    - 사용자가 트윗을 올리면, follower의 redis cache로 push해 준다. follwer가 많은 유명인의 경우 상당한 시간이 걸릴수 있다. 레이디 가가나 오바마 같은 경우
- home timeline의 트윗은 최대 800개까지 redis에 저장


## ranking

- 기본적으로는 시간순으로 타임 라인을 구성할 수 있다.
- 여기에 커멘트가 많거나 공유가 많은 트윗에 가산점을 줘서 점수로 소트하여 타임라인에 표시해 줄 수 있다
- affinity, time, popularity의 팩터가 중요
- 이러한 인풋대비 결과(stickyness)와 연동하여 A/B test후 계속 개선해 나가야 함. 이를 위해서는 analytics 역시 필요.

## Publishing

- pull vs push, read가 훨씬 많기 때문에 write시에 push해서 미리 사용자들의 타임라인을 만들어 두는 것이 유리
- inactive 사용자나 트위터 홈, 서치 결과(query timeline)등은 pull based로 하고 있음.

## monitoring

## analytics


## Reference 

- http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html
- [big data in realtime at Twitter](https://www.slideshare.net/nkallen/q-con-3770885)

