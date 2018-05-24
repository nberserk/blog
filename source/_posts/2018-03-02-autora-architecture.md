---
layout: post
title:  "Aurora architecture"
tags: architecture aws
published: true
---

Amazon Aurora는 아마존의 managed DB service인데, 최근에 mysql이나 postgress에서 많이 갈아타는 듯 하다.
우연히 [aurora architecutre에 관한 논문](https://www.allthingsdistributed.com/files/p1041-verbitski.pdf)을 보았는데 인상깊은 부분들이 있어 여기에 리뷰를 해보려 한다.

3가지 큰 포인트는 :
1. building storage as an independent fault-tolerant and self-healing service
1. only writing redo logs
1. make backup&recovery from one time expensive operations to asynchronous operations across a large distributed fleets.

## Durability
quorum-based voting protocol!

aws에서 사용하는 az(availability zone)은 3개. Aurora는 하나의 AZ가 고장나도 failover할 수 있는것을 목표임. 그래서 각 AZ에 2개의 storage node를 두고 총 6개의 replication을 유지. 이렇게 해서 read는 3/6 quorum, write는 4/6 quorum 을 기준으로 둔다. 즉, 6개 노드중 3개의 노드가 동일한 값을 주면 그것을 성공으로 간주, write의 경우는 4개의 노드에서 같은 응답을 받으면 성공으로 간주 하게 된다. 하나의 AZ는 실패해 되게 설계를 했기에 OS나 security updates등도 쉽게 적용할 수 있게 되었다. 사용자도 모르게.

만약 AZ failure가 2개 이상의 영역에서 발행하면 어떻게 될까? 물론 적은 확률이겠지만 그것까지 고려해서 storage를 10G 단위의 Protection group(PG)로 나누어서 recovery time을 줄일 수 있음. 10Gbps망에서 10G는 10초면 복구할 수 있음.

## Log equlas DB

storage node로 전송하는 것은 redo log뿐이다. 이것이 좀 획기적인듯.

database page를 매번 처음부터 생성하는 것은 비싼 작업이기 때문에 중간중간에 snapshop을 만든다. 그래서 recovery등은 checkpointing과는 다르게 더 빨리 recovery가 가능함.

redo log는 순차적으로 증가하는 Log Sequence Number(LSN)을 가짐.

recovery시에는 VCL(Volume complete LSN)이후의 LSN은 모두 버림. 더 디테일하게 들어가서는 VCL보다 작은 LSN중에서 highest CPL(Consistency point LSN)을 VDL(Volume durable LSN)이라고 정의하고 VDL보다 큰 LSN을 모두 버린다. CPL은 transaction단위가 끝난 LSN이라고 보면 될듯.


## References

- [Amazon Aurora: Design consideration for high throughput cloud-native relational dababases](ttp://checkstyle.sourceforge.net/index.html)
- [Amazon Aurora Under the Hood: Quorum Reads and Mutating State](https://aws.amazon.com/blogs/database/amazon-aurora-under-the-hood-quorum-reads-and-mutating-state/)




