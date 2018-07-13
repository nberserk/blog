---
title: spring introduction
date: 2018-07-13 09:57:49
tags: spring in action
---

Spring in Action이라는 책을 스터디를 시작했다. 일주일에 한 챕터씩 진도를 나갈 예정인데 그에 맞춰 글로 기록을 남겨볼까 한다. 빼먹지 말고 끝까지 가야할텐데..

이번 진도는 챕터 1까지인데 주로 Spring의 근간을 이루는 요소들에 대한 간략한 소개가 나온다.

Spring 은 자바 개발을 쉽게 만들어 주는데 아래 주요 4가지 장점이 있다고 주장한다.

- POJO, Spring은 스프링 관련 클래스를 extends 하거나 implement하라고 강요하지 않고 POJO를 많이 사용한다. 
- DI(Dependency Injection)
- AOP(Aspect Oriented programming)
- remove boilerplate code, 많이 사용하는 패턴들을 JdbcTemplate, RestTemplate등으로 구현해 놓았고 필요한 부분만 커스터마이징 할 수 있다.

위에서 보듯이 다 디펜던시를 줄이기 위한 방법들을 많이 사용하고 있는데 그 만큼 디펜던시가 강해지면 테스트 하기도 힘들고 유지 보수하기도 힘들어지기 때문이다. 그렇다고 커플링이 없는 모듈이란것은 아무것도 하지 않는 클래스 아니던가. 그래서 loosely coupling을 하기 위해 DI와 AOP, POJO를 들고 나온 것이다. 

이렇게 해서 많은 장점을 제공해주기 때문에 우리는 스프링을 써야 한다.
머 이런 논리인듯 하다. 따라서 러닝 커브가 좀 있는 편.
