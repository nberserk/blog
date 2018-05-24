---
layout: post
title: ssh tunneling on AWS
tags: ssh aws tunneling
---

AWS EMR를 사용하다보면 보안등의 이유로 로컬로만 웹서비스등을 오픈해 놓았다. 그래서 web ui는 접속이 되더라도 제대로 동작하지 않다. YARN의 Resource Manager등도 개별 노드로 가는 노드들은 다 링크를 찾지 못한다. 각 링크들이 private ip로 되어 있기 때문이다. 제플린도 마찬가지로 웹은 뜨지만 제플린 서버로 접속을 하지 못해서 disconnted상태로 뜬다. 그래서 이럴땐 ssh tunneling을 사용하면 fully 접근하는 길이 열리게 된다.

아마존에서는 아래처럼 이렇게 가이드를 해놓았다.


> Hadoop, Ganglia, and other applications publish user interfaces as websites hosted on the master node. For security reasons, these websites are only available on the master node's local webserver (http://localhost:port) and are not published on the Internet



## ssh tunneling
나의 경우는 아래처럼 구성되어 있기 때문에 bastion으로 한번 터널링을 하고 bastion에서 다시 EMR master로 터널링을 해줘야 한다.

```
+----------+         +---------+        +------------+
|localhost | <---->  |bastion  |  <---> | EMR master |
+----------+         +---------+        +------------+
```

Zeppelin을 예로 들어서 터널링을 해보자. Zeppelin의 경우 emr-4.1에서 8890포트로 설정이 되어 있어서 우선 내 로컬의 8890포트를 bastion의 8890포트로 포워딩 시켜준다.


```
ssh -i <your.pem>  -L 8890:localhost:8890  ec2-user@bastion
```    
    
이렇게 하면 bastion으로 ssh 접속이 되었을 것이고 다시 EMR master로 8890 포트를 포워딩 한다.


```
ssh -i <your.pem> -p 22 -L 8890:localhost:8890 hadoop@ec2-??-??-??-???.us-west-2.compute.amazonaws.com

```

이렇게 하면 이제 EMR master로 ssh 접속이 된 상태일 것이고. 그러고 난 후 http://localhost:8890 에 접속을 하면 제플린 웹에 접속할 수 있게 된다. 오른쪽 상단에 'connected'라고 되어 있을 것이다.

### options
ssh 명령을 줄때 다음 옵션들을 경우에 맞게 사용하면 아주 유용.

* -v : verbose. 뭔가 잘 안될때 이 옵션을 사용하자
* -N : ssh 접속을 하지 않는다.


## revision history

* 2015/10/30 initial draft

        

