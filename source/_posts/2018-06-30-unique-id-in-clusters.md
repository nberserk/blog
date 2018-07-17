---
title: cluster wide unique ID
date: 2018-06-30 16:24:03
tags: archi
---

cluster 단위의 유닉한 ID를 만들려면 어떻게 해야 할까? 출처는 [여기](https://medium.com/@varuntayal/what-does-it-take-to-generate-cluster-wide-unique-ids-in-a-distributed-system-d505b9eaa46e)


제일 쉬운 방법으로 UUID 같은걸 생각해볼수 있다. 이론적으로 가능한 조합은 16^32 승이나 되니 좀처럼 문제가 있어 보이진 않다. 하지만 실제로는 시드 같은걸 잘사용해야 하고
그렇지 않으면 충돌 확률이 꽤 올라갈 수 있다고 한다.
> 550e8400-e29b-41d4-a716-446655440000

두번째 방법으로는 유닛한 ID를 발급하는 서버(Ticket Server)를 하나두고 여기에 클라이언트들이 받아서 받아가는 방법. mysql의 auto increment index를 사용할 수도 있고, 2대로 한대에는 홀수개 생성, 나머지 한대는 짝수개로 생성하게 해서 이중화 할 수도 있다. 아니면 REDIS로 유닉한 값을 계속 만들어 사용할수도 있으나 persistent 하지는 않다. 이를 보완할 방법이 필요함.n
좋은 방법이기는 하지만 클러스터가 커질수록 ticket server에 부하가 걸리고, 이 서버가 SPOF 포인트가 된다.

세번째 방법으로는 여러개의 조합으로 각 노드에서 독립적으로 생성하는 방법. Twitter의 snowFlake가 이런 방식이라고 함. snowFlake인 이유는 눈송이의 모양은 하나 하나 모두 다르다고 함. 눈송이와 그 동작방식이 유사하여 이런 네이밍을 했다는데 잘 어울린다.

> timestamp(42bit) + nodeID(10bit) + sequential ID(12bit)

위와 같은 공식으로는 하나의 노드는 밀리세컨드당 2^12 즉 4096개의 유닉한 ID를 생성할 수 있다. 클러스터 전체로는 2^22 즉, 4194304 4백만 개를 생성 할 수 있다. 이 정도면 왠만한 시스템은 다 커버할 수 있을 듯 하다.

이 시스템의 capa 즉 [4096개가 넘는 요청이 들어오면 어떻게 하는가하고 찾아보니](https://charsyam.wordpress.com/2012/12/26/%EC%9E%85-%EA%B0%9C%EB%B0%9C-global-unique-object-id-%EC%83%9D%EC%84%B1-%EB%B0%A9%EB%B2%95%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A0%95%EB%A6%AC/) 다음 밀리세컨드까지 기다린다고 한다. 이것을 잘 모니터링 하다가 다음 버전을 준비하면 될듯 하다.