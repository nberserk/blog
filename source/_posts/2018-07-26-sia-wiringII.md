---
title: Spring In Action - Advanced Wiring
date: 2018-07-26 17:02:54
tags: spring in action
---

# Chapter3 - Advanced Wiring

## Env and profiles
환경에 따라 다른 configuration이 필요한 것은 흔한 요구사항임. 예를 들어 local에서는 embedded database를 사용하고 prod에서는 mysql을 사용하는 식으로 말이다.

스프링은 재빌드가 필요없는 runtime 솔루션을 제공한다.

```java
@Profile("dev") // dev에서만 유효함.
```

그렇다면 어떻게 profile을 설정할 수 있지? 아래 property를 통해서 환경변수,web.xml, JVM System property 등으로 설정할 수 있음

> `spring.profiles.active` vs `spring.profiles.default` (active가 우선순위가 높음)

test시 에는 아래처럼 원하는 프로파일을 복수개 혹은 단수로 지정할 수 있음.

```java
@ActiveProfiles("dev") // active profile을 지정할 수 있게 하는 헐퍼 annotation
```

## Conditional beans

```java
@Conditional(SpecialCondition.class) // 조건이 맞을때만 bean생성
```

실제로는 @Profile도 @Conditional을 이용해서 구현되어 있다.

## Addressing ambiguity

```java
@Component
public class Cake implements Dessert{}

@Component
@Qualifier('icecream') // 
public class IceCream implements Dessert{}

@Autowired
@Qualifier('icecream') 
Dessert mine; // wiring with IceCream
```

## Scoping beans

bean scope 이라는 개념이 있는데 default는 싱글톤임.
- singleton
- prototype : inject될때마다 빈 생성
- session : session이 만들어질때 빈 생성
- request : request가 만들어질때 빈 생성

session, request의 경우는 언제 생성을 해야 할지가 모호해지는데, 이것때문에 proxy 하는 객체를 둬서 그것으로 실제 콜이 일어날때 real bean에게 콜을 한다.  

## Value Injection

value값을 외부에서 받아오고 싶을때는 property를 사용하면 된다.

```java
@Configuration
@PropertySource("classpath:/app.properties")
public class ExpressiveConfig {
  @Autowired
  Environment env;

  // env.getProperty(key);  
}
```

Spring Boot의 경우에는 이것도 자동으로 구성해주는데 `src/main/resources`에 있는 `application[-env].properties`을 찾는다.
fallback 순서는 아래 처럼 된다.
- application-env.properties
- application.properties

property를 읽어오는 순서를 [여기에](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) 명시하고 있으니 참고.

참고로 `Spring boot`의 경우가 훨씬 편함.






