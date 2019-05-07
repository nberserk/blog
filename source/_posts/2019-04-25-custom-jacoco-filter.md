---
layout: post
title:  "implement custom jacoco filter"
tags:
 - jacoco
 - coverage
---

[Jacoco](https://www.eclemma.org/jacoco/) is popular coverage tool for Java worlds. It provides [various options](https://www.eclemma.org/jacoco/trunk/doc/agent.html) to include or exclude specific classes.

My special requirement is to generate exact coverage report for public functions of non UI classes. Jacoco doesn't support that functionality. so I want to share my experiences here because I couldn't find any solution for this.

The first thing I found was [filters](https://github.com/jacoco/jacoco/wiki/FilteringOptions). If I can implement my own filter, then that's all I need. Let's analyze it more in source code level. After some time, I've found [Filter class](https://github.com/jacoco/jacoco/blob/d3c47652b89267709aae04bec66a68bdf6df9670/org.jacoco.core/src/org/jacoco/core/internal/analysis/filter/Filters.java#L34)! It has all required filters for proper coverage reporting.

Next is implementing my own filter. Let's reference already existing source code like [AnnotationGeneratedFilter.java](https://github.com/jacoco/jacoco/blob/master/org.jacoco.core/src/org/jacoco/core/internal/analysis/filter/AnnotationGeneratedFilter.java). Its main functionality is excludding functions annotated by having 'Generated' string. 

I [forked](https://github.com/nberserk/jacoco) jacoco project and created `_customFilter` branch based on `0.8.3` tag and implemented [WhiteListFilter](https://github.com/nberserk/jacoco/blob/_customFilter/org.jacoco.core/src/org/jacoco/core/internal/analysis/filter/WhiteListFilter.java) and [its testcase](https://github.com/nberserk/jacoco/blob/_customFilter/org.jacoco.core.test/src/org/jacoco/core/internal/analysis/filter/WhitelistFilterTest.java). 

And I created binary using the following commands. jar files are located in `jacoco/target/` folder.

> mvn clean package -DskipTests=True

last thing you have to do is let gradle use those built jar files. copy those jar files to `<root>/lib` directory. and change your build.gradle file like the followings.

```build.gradle

dependencies {
    // override jacoco jars
    jacocoAgent files("$rootProject.projectDir/lib/org.jacoco.agent-0.8.3.201904130250.jar")
    jacocoAnt files("$rootProject.projectDir/lib/org.jacoco.ant-0.8.3.201904130250.jar",
            "$rootProject.projectDir/lib/org.jacoco.core-0.8.3.201904130250.jar",
            "$rootProject.projectDir/lib/org.jacoco.report-0.8.3.201904130250.jar",
            "$rootProject.projectDir/lib/asm-7.0.jar",
            "$rootProject.projectDir/lib/asm-tree-7.0.jar",
            "$rootProject.projectDir/lib/asm-commons-7.0.jar"
            )
    
}
```

then last thing you have to is annotate your target class using `@TestRequired` class.

```java
@TestRequired
class MyClass {
    public doSomething(String str) {
        // this function will be included in coverage report.
    }

    private doSomethingElse(String str) {
        // this function will NOT be included in coverage report.
    }
}
```



