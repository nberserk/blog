---
title: optimistic locking
date: 2018-11-21 09:53:41
tags: dynamo
---


# optimistic locking

https://docs.aws.amazon.com/ko_kr/amazondynamodb/latest/developerguide/DynamoDBMapper.OptimisticLocking.html

dynamo db가 사용하는 optimistic locking
use case가 그럴듯 하여 정리.

dynamo db는 RDB가 아니니 당연 트랜잭션을 사용할수 없음. 그래서 아쉬운 경우가 많음. 이때 optimistic locking을 사용하여 간단히 트랜잭션 처리를 할 수 있어 유용함.
컬럼에 version field를 추가하고 user id(partition key)/version(sort key)로 설정하여 중복된 version이 생성될 수 없게 constraint를 걸어줌. 

플로우는 이러함.
1. row get
1. 값 변경하고, version +1 증가
1. 저장. 이때 integrity exception이 발생하면 1부터 다시 시작함.


