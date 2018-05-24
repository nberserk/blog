---
layout: post
title:  "maven assembly plugin으로 패키징 해서 배포하기"
tags: maven mvn assembly
---

메이븐으로 프로젝트를 진행하다가 이 것을 배포해야 할때 configuration 파일들이나 헬퍼 스크립트 등을 디렉토리로 구성해서 zip/tar.gz으로 배포해야 할 필요가 있다. 그런것을 메이븐 상에서 가능하게 해주는 것이 maven assembly plugin 이다. 


## mvn configuration
우선 assembly plugin을 사용하기 위해서 build/plugins 하위에 명시해 준다.

```xml
<plugin>
  <artifactId>maven-assembly-plugin</artifactId>
  <version>2.5.5</version>
  <configuration>
    <descriptors>
      <descriptor>src/assemble/distribution.xml</descriptor>
    </descriptors>
  </configuration>
  <executions>
    <execution>
      <id>make-assembly</id> <!-- this is used for inheritance merges -->
      <phase>package</phase> <!-- bind to the packaging phase -->
      <goals>
        <goal>single</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

- configuration 하위에 어떻게 assemble을 할지를 기술하는 assembly descriptor를 명시할 수 있다. 물론 복수개도 가능.
- 디폴트로 지원하는 descriptor도 있음. 예를 들면 jar-with-dependencies 같은 경우. 이것을 명시하면 fat-jar를 만들어 줌.
- assembly:single goal만 유효함. 이것을 package phase에 실행하려면 위처럼 execution tag 하위에 phase와 goal을 명시하여 주면 됨.

## assembly descriptor

moduleSet은 메이븐 모듈의 관점에서 처리를 하는 것이고. 소스, 바이너리등의 처리를 할 수 있다. 아래처럼 하면 소스들을 archive로 카피해 준다. 

```xml
<moduleSet>
  <useAllReactorProjects>true</useAllReactorProjects> <!--이것은 모든 모듈들에 대해서 처리를 하고 싶을때 사용한다. -->
  <includes>
    <include>com.nberserk:core</include>
  </includes>
  <sources>
    <outputDirectoryMapping>
      ${module.basedir.name}
    </outputDirectoryMapping>    
  </sources>
</moduleSet>
```

반면 dependencySet은 각 모듈의 산출물과 디펜던시를 가지는 것의 산출물들을 어떻게 가져 올것 인지에 대해서 기술한다. 

```xml
<dependencySet>
  <includes>
    <include>com.nberserk:core</include> <!--core를 루트 드렉토리로 카피-->
  </includes>
</dependencySet>
<dependencySet>
  <outputDirectory>lib</outputDirectory>
  <excludes>
    <exclude>com.nberserk:core</exclude> <!--core가 refer하는 모든 jar들을 lib으로 카피-->
  </excludes>  
</dependencySet>
```

이 외에도 fileSet 을 사용해서 특정파일이나 폴더를 패키징할 수도 있다.

```xml
<fileSets>
  <fileSet>
    <directory>../</directory>
    <includes>
      <include>*.md</include>   <!--*.md 파일을 루트로 복사-->
    </includes>
  </fileSet>
  <fileSet>
    <directory>../bin</directory> <!--bin디렉토리에 있는것을 bin으로 복사-->
    <directoryMode>0755</directoryMode>
    <fileMode>0755</fileMode>   <!--파일 모드를 변경해줌-->
  </fileSet>
  <fileSet>
    <directory>../conf</directory>
  </fileSet>
</fileSets>
```

## Reference
- [assembly descriptor reference](http://maven.apache.org/plugins/maven-assembly-plugin/assembly.html)
- [Maven:complete reference](http://books.sonatype.com/mvnref-book/reference/index.html)
- 실제로 동작하는 프로젝트 [github](https://github.com/nberserk/sandbox/tree/master/mvn-assembly)에 공유 되어 있음.


## revision history
* 2015/7/32 initial draft

