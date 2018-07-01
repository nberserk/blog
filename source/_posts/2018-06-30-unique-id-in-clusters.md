---
title: cluster wide unique ID
date: 2018-06-30 16:24:03
tags: archi
---

cluster 단위의 유닉한 ID를 만들려면 어떻게 해야 할까? 출처는 [여기](https://medium.com/@varuntayal/what-does-it-take-to-generate-cluster-wide-unique-ids-in-a-distributed-system-d505b9eaa46e)


제일 쉬운 방법으로 UUID 같은걸 생각해볼수 있다. 이론적으로 가능한 조합은 16^32 승이나 되니 좀처럼 문제가 있어 보이진 않다. 하지만 실제로는 시드 같은걸 잘사용해야 하고
그렇지 않으면 충돌 확률이 꽤 올라갈 수 있다고 한다.
> 550e8400-e29b-41d4-a716-446655440000

두번째 방법으로는 유닛한 ID를 발급하는 서버(Ticket Server)를 하나두고 여기에 클라이언트들이 받아서 받아가는 방법. 
좋은 방법이기는 하지만 클러스터가 커질수록 ticket server에 부하가 걸리고, 이 서버가 SPOF 포인트가 된다.

세번째 방법으로는 여러개의 조합으로 각 노드에서 독립적으로 생성하는 방법. Twitter의 snowFlake가 이런 방식이라고 함.

> timestamp(42bit) + nodeID(10bit) + sequential ID(12bit)


