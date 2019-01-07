---
title: spark DataFrame
tags: 
- spark
---

spark의 가장 기본이 되는 데이터 타입은 아래 3가지로 요약할 수 있다.

- [RDD](/2018/07/02/spark-rdd)
- DataFrame
- DataSet

아래로 내려갈수록 type안정성이 강화된 high level API라고 생각하면 된다.





---
title: incoming
tags:
---

# scala

```scala
val list = Seq("folder1", "folder2")
spark.read.load(list: _*)
```

`_*`의 의미 : vaarg로 만들어주는 것. 

https://github.com/fishercoder1534/Leetcode

- 구글 인터뷰 한번 정독할것.

https://medium.com/m/global-identity?redirectUrl=https%3A%2F%2Fhackernoon.com%2Fgoogle-interview-questions-deconstructed-the-knights-dialer-f780d516f029

# building & submitting spark application

## spark application template



# loading s3 files on spark 

```scala
sc.hadoopConfiguration.set("fs.s3a.access.key", "xx")
sc.hadoopConfiguration.set("fs.s3a.secret.key", "xxx")

val df = spark.read.load("s3a://<bucket_name>/data.parquet")
df.show()
// load recursiviely
val df2 = spark.read.load("s3a://<bucket_name>/2018/*/*/*", "s3a://<bucket_name>/2017/*/*/*)
df2.show()
```





# SAM(serverless application model)

> pip install  aws-sam-cli



# edge computing

## AWS

https://aws.amazon.com/greengrass/

edge computing pros:

- respone events in real time
- operate in offline. kind of buffer
- secure
- aws lambda
- cost saving, coz only send needed data to cloud


## Azure

https://docs.microsoft.com/en-us/azure/iot-edge/

iot edge runtime

- security(authentication, authorization)
- loggin
- monitoring
- module mgmt
- communication between sensors, cloud, modules

iot edge edge hub

- same interface with iot hub. so make them switching easy.
- subset of iot hub functionality. proxy to real iot hub. delegate authentication to iot hub.
- connect to iot hub as one connection instead of multiple connection from all sensors.
- module input/output wiring possible

iot edge agent

- install/start module, status audit to iot cloud.

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

## 어디서 사용하고 있나
Amazon dynamoDB, Chord - distribute hash table by MIT

## how

보통 hash는 `value%mod` 에 의해서 

- http://n00tc0d3r.blogspot.com/2013/09/big-data-consistent-hashing.html
- http://highscalability.com/blog/2018/6/18/how-ably-efficiently-implemented-consistent-hashing.html
- [dynamo paper](http://s3.amazonaws.com/AllThingsDistributed/sosp/amazon-dynamo-sosp2007.pdf)

Werner vogels interview : http://www.se-radio.net/2006/12/episode-40-interview-werner-vogels/



# google search

- indexing
  - crawler : do not get into infinite loop by marking already crawled url
  - indexer : bunch of Map/Reduce, analyze page make index
    - weight incoming link
    - give points to words according to the pos. words in title vs comment
    - understanding context, `hash+table` -> CS `hash+brownies` -> cook
- getting result
  - each search word have corresponding index
  - AND|OR op for each index
  - sort by ranking

## reference

- [pagerank paper](http://infolab.stanford.edu/~backrub/google.html)
- https://softwareengineering.stackexchange.com/questions/38324/how-would-you-implement-google-search


# news feed system

http://blog.gainlo.co/index.php/2016/03/29/design-news-feed-system-part-1-system-design-interview-questions

http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html



[Designing Instagram](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5673385510043648)




# normalize/denormalize
- OLTP(Online Transaction Processing) : normalize
- OLAP(Online Alalytic Processing) : de-normalize


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

# nGrinder



# shell

sudo with password 
> echo pwd | sudo -S ls /etc

post connection test

> nc -z -w3 loadtest-sknowledgedb.cls9hmfha9jo.us-west-2.rds.amazonaws.com 3306

check listening port 

> netstat -plnt

show process command fully

> ps aux | less

# HBase

google의 bigtable논문을 바탕으로 오픈소스로 개발한 DB

row key가 있고, 각 row에는 column family, 각 column family에는 column이 있음. 그래서 채팅 데이터를 기록하는 용도로 많이 사용함.

https://www.tutorialspoint.com/hbase/hbase_overview.htm

