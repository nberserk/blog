---
title: spark application template
tags: spark
---

spark application tempalte을 만들어 보았다.
의외로 설정해줄것이 좀 있기 때문. 
프로젝트는 [ spark template github project](https://github.com/nberserk/sandbox/tree/master/spark) 에서 볼 수 있다.

1. [maven scala project](https://docs.scala-lang.org/tutorials/scala-with-maven.html) 임. official spark도 maven으로 되어 있기도 하고, 원래 스칼라는 sbt라는 빌드 툴을 쓰는데 아마도 속도에서 뭔가 문제가 있는 모양.
1. scala version은 2.11.12을 사용했고, spark는 [spark v2.3.2 Apache Haddop 2.7 and later](http://spark.apache.org/downloads.html) 버전을 사용함. scala version이 다르면 바이너리도 다르니 신경 써줘야 함.
1. 문서는 http://spark.apache.org/docs/2.3.2/ 이것을 보면 됨.
1. 설정을 파일로 빼기위해 [typesafe](https://github.com/lightbend/config) 를 사용함. 
1. DataFrame, DataSet등을 비교학 위해서 [spark-fast-tests](https://github.com/MrPowers/spark-fast-tests) 를 사용함.

