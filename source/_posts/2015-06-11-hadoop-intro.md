---
layout: post
title:  "hadoop 설치 및 설정"
tags: hadoop hdfs yarn
---

## 소개
하둡은 hdfs + MapReduce + ResourceManager(YARN) 이라고 볼 수 있다. 하둡은 현재 사용되고 있는 모든 클라우드 기술의 근간이 되는 기술이다. 요즘 화제가 되고 있는 스파크(Spark)도 hdfs+MR 기술을 이용하여 만든 것이다.

* hdfs :  distributed file system, replication은 parallelism의 근간. 하나의 노드의 파일이 손상되어도 replication으로 보전하는 컨셉.
 * NameNode/SecondaryNameNode : file의 metadata를 기록하고 쿼리하면 리턴해주는 모듈.
* MapReduce : MR 프로그램을 돌릴 수 있는 프레임웍. 그 유명한 Map Reduce. 물리적으로 하나의 컴퓨터에서 계산이 불가능한 것을 여러대의 컴으로 Map 해서 따로 계산후 Reduce로 취합하는 개념.
* YARN : hadoop 2.x 버전 부터 Resource Manager가 독립이 되어 생긴 모듈. application scheduling 등을 하게 됨

## 설치 및 설정
hadoop 2.3 기준이고, 2.x 버전에서는 그대로 적용되는 내용일 것 같다. 우선 3대의 컴퓨터에 설치를 해보자. 각 컴퓨터의 이름과 용도를 맵핑 하면 아래처럼 된다.

```
dfs : hadoop master
dfs1 : data node
dfs2 : data node
```

* 위 컴퓨터간에 ssh를 password없이 할 수 있게 서로 ssh key 등록
* hadoop-2.3.x 버전을 master의  `<HADOOP_INSTALL_DIR>`에 untar
* /etc/hosts에 컴퓨터 이름으로 찾아 갈 수 있게 dns 등록. 예> `0.0.0.0 dfs1 0.0.0.1 dfs2`
* /etc/hadoop/masters 에 master의 컴 이름인 dfs를 입력(한줄에 하나씩)
```
dfs
```
* /etc/hadoop/salves 에 data node 를 실행시킬 컴 이름을 입력
```
dfs1
dfs2
```
* /etc/hadoop/hadoop-env.sh 에 있는 JAVA\_HOME, HADOOP\_HOME 변수의 값을 채워준다. 각 JDK및 HADOOP이 설치되어 있는 path를 적어주면 된다.
* master에 있는 hadoop 폴더를을 dfs1, dfs2에 카피. 아래 bash script를 사용하면 `HADOOP_HOME` 폴더를 싱크할 수 있어 무지 편하다.

<a name="rsync"></a>
```
for host in dfs1 dfs2
do
    rsync -rv $HADOOP_CONF_DIR/ $host:$HADOOP_CONF_DIR/
done
```

#### hdfs 설정
etc/hadoop/hdfs-site.xml의 configuration 하위에 아래 property 추가
```xml
    <property>
		<name>dfs.replication</name>
		<value>2</value>
	</property>
	<property>
        <name>dfs.name.dir</name>
        <value>/home/spark/hadoop-2.3.0/hdfs/name</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/home/spark/hadoop-2.3.0/hdfs/data</value>
    </property>
```

만약 하드 디스크가 여러개라면, dfs.data.dir에 마운트 된 경로를 추가해주면 된다. `경로1, 경로2` 이런식으로..
#### YARN 설정
etc/hadoop/yarn-site.xml의 configuration 하위에 아래 property 추가. resource manager를 어느 컴에 둘지를 설정.
```xml
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>spark</value>
</property>
```

이 외에도 log aggregation 을 enable하려면 yarn-site.xml에 아래의 property를 추가해 주면 된다.  그럼 아래와 같은 식으로 log를 모아서 한번에 볼 수 있어서 분석시에 유용하다.
`yarn logs -applicationId <app id>`

```xml
<property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
  </property>
```


## 실행

#### HDFS

- master인 dfs 컴에서 .sbin/start-dfs.sh를 실행.
- jps 실행시, 마스터에서는 NameNode,SecondaryNameNode가 그외 노드에서는 DataNode가 보이면 OK.
- master:50070 으로 접속하면 hdfs web ui를 볼 수 있다.

#### stopping specific datanode
특정 datanode만 내리고 싶을때가 있는데 그때는 아래 명령어를 통해 특정ㅎ datanode를 kill 할 수 있다.
```
./bin/hadoop-daemon.sh --config ./etc/hadoop stop datanode
```

#### YARN
- master인 dfs 컴에서 .sbin/start-yarn.sh을 실행
- jps 실행시, master에서는 ResourceManager가, 그 외 노드에서는 NodeManager process가 보이면 제대로 뜬 것임.
- master:8088 으로 접속하면 yarn web ui 를 볼 수 있다. nodes 메뉴에서 위에서 설정한 slaves들이 다 보이면 정상 동작하고 있는 것이다.

## TroubleShooting

- 우선 실행시 뜨는 콘솔의 에러내용을 보면 대락 실패원인을 알 수 있고
- 특정 노드가 안뜬다던가 하면, 그 노드의 /logs 폴더에 있는 로그를 살펴 보면 힌트를 얻을 수 있다.


## revision history
* 2015/7/27 minor update
