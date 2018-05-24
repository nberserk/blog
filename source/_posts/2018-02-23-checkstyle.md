---
layout: post
title:  "enforce codestyle"
tags: checkstyle
published: true
---

소스코드 퀄러티를 높이기 위한 일환으로 code style을 맞추는 작업을 maven을 사용하는 java 프로젝트에서 사용하는 방법이다.

code style을 강제하는 방법은 report를 생성하여 공유하거나, 빌드 단계에서 체크하여 스타일이 맞지 않으면 에러를 내게 하는 방법등이 있을 수 있지만 개인적으로는 후자를 추천한다.
에러가 나면 업스트림에 커밋을 할 수 없고 그 법칙을 지킬수 밖에 없기 때문이다. 전자의 경우 누군가 나서서 리포트를 공유하고 고쳐 달라고 핑을 해야 하는데 여러모로 소모적인 프로세스이기 때문이다.

## checkstyle

가장 많이 사용되는것이 [checkstyle](http://checkstyle.sourceforge.net/index.html)

[maven checkstyle plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/index.html)이 있기 때문에 이것을 사용하면 maven과 쉽게 연동할 수 있다.

```xml

      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version>3.0.0</version>
        <configuration>
          <configLocation>checkstyle.xml</configLocation>
        </configuration>
      </plugin>
```

위처럼 <configLocation> tag를 사용하면 custom rules을 사용할 수 있다. [google rule](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml) 을 참고하여 룰을 가감하면 된다.

또한 빌드 단계에서 체크를 하고 싶으면 아래 처럼 executions tag로 순서를 바꿔줄 수 있다. 이렇게 하면 checkstyle룰을 어기면 빌드가 실패하게 된다.

```xml
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>3.0.0</version>
                <dependencies>
                    <dependency>
                        <groupId>com.puppycrawl.tools</groupId>
                        <artifactId>checkstyle</artifactId>
                        <version>8.8</version>
                    </dependency>
                    <dependency>
                        <groupId>com.samsung.knowledge.billing</groupId>
                        <artifactId>build-tools</artifactId>
                        <version>0.1.0</version>
                    </dependency>
                </dependencies>
                <executions>
                    <execution>
                        <id>validate</id>
                        <phase>validate</phase>
                        <configuration>
                            <configLocation>checkstyle.xml</configLocation>
                            <encoding>UTF-8</encoding>
                            <consoleOutput>true</consoleOutput>
                            <failsOnError>true</failsOnError>
                            <failOnViolation>true</failOnViolation>
                            <violationSeverity>warning</violationSeverity>
                        </configuration>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

## checkstyle in multi module project

 만약 multi-module project 라면 같은 설정을 sub module 마다 해줘야 하기 때문에, checkstyle을 실행하는 모듈을 만들고 다른 모듈이 이것을 사용하게 디펜던시를 걸면 공용화 할 수 있다.
 요기에 예시가 있으니 참고.
 <https://maven.apache.org/plugins/maven-checkstyle-plugin/examples/multi-module-config.html>




## References

- [checkstyle](ttp://checkstyle.sourceforge.net/index.html)

## revision history

- 2018/4/25, checkstyle in multi module project added



