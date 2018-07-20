---
title: Spring In Action - Wiring Beans
date: 2018-07-18 19:35:09
tags: spring in action
---

# Wiring beans

DI는 스프링의 근간이고 두 오브젝트간의 결합을 가능하게 해주는 것을 wiring이라고 부른다. 이런 wiring이 어떻게 동작하고 있는지 알아보자.

스프링은 3가지의 wiring을 지원한다. xml과 java config중 type-safe한 java config를 추천하고 있다. 

- xml
- java config
- implict bean discovery and automatic wiring

## automatic wiring

automatic wiring은 크게 2가지에 의해서 이루어진다.

- component scanning
- auto wiring

bean은 `@Component`라는 annotation 의해서 선언되어지고 향후 스캐닝시에 bean으로 생성되어져서 스프링 컨테이너로 초기화 되게 된다. bean에는 이름을 지정해 줄수도있는데, `@Component('name')`이라고 하면 되고 따로 지정을 해주지 않으면 클래스 이름이 default가 된다.

컴포넌트 스캐닝은 자동으로 이루어지지 않고 `@ComponetScan` annotation에 의해서 이루어지고 해당 패키지 밑 하위 패키지들이 스캐닝 대상이 된다. xml에서도 다음과 같이 스캐닝을 지정해 줄 수 있다.

> <context:component-scan base-package="soundsystem" />

아래처럼 복수개의 패키지및 java config를 지정할 수 있음. 

```java
@ComponentScan(basePackages="soundsystem") // 명시적으로 패키지를 지정
@ComponentScan(basePackages={"soundsystem", "video"}) // 여러개의 패키지를 지정 할 수도
@ComponentScan(basePackageClasses={CDPlayer.class, DVDPlayer.class}) // 여러개 클래스 지정
```

앞서 지정한 빈을 사용할때는 `@Autowired` annotation을 사용하면 되는데 bean의 이름으로 자동으로 매핑을 해준다. 해당 bean을 찾을 수 없을때는 초기화시에 에러가 나게 된다.

## java config

java config는 xml에 비해 powerful하고, type-safe하고 refactoring에 우위가 있다. xml의 typo때문에 한번쯤은 삽질한 경험이 있을듯 하다. 이때의 허무함이란..

`@Configuration` annotation은 컴포넌트 스캐닝 대신에 explicit하게 빈들을 생성할 수 있게 해준다. 

```java
@Bean
public CompactDisc sgtPeppers() {
  return new SgtPeppers();
}

@Bean
public CDPlayer cdPlayer() {
  return new CDPlayer(sgtPeppers());
}

@Bean
public CDPlayer anotherCDPlayer() {
  return new CDPlayer(sgtPeppers());
}
```

위의 예에서 원래의 자바 코드라면 sgtPeppers는 서로 다른 instance를 리턴하기때문에 cdPlayer와 anotherCDPlayer는 서로 다른 CompactDisc 인스턴트여야 하지만 실제로는 같은 인스턴스로 초기화 된다. @Bean때문에 behavior가 달라진 것이다.


## mixing java config and xml

아래처럼 다른 configuration을 임포트할 수도 있고, xml config도 import 할 수도 있다.

```java
@Configuration
@Import({CDPlayerConfig.class, CDConfig.class}) // import other java config
@ImportResource("classpath:cd-config.xml") // import xml config
public class SoundSystemConfig {
}
```

반대로 xml에서도 java config를 import 할 수 있다.

```xml
<beans>
  <bean class="soundsystem.CDConfig" />
  <import resource="cdplayer-config.xml" />
</beans>

```
