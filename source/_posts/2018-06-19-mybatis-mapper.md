---
title: mybatis mapper
date: 2018-06-19 18:23:13
tags: mybatis
---

 주로 spring과 함께 사용하는 mybatis. java persistence framework으로서 Hibernate 같은 미들웨어 되겠다.

 ## Dynamic SQL

 ```sql
 SELECT * FROM ORDER O
<trim prefix="WHERE" prefixOverrides="AND|OR">
    <if test="searchText != null">
        AND MATCH(O.name) AGAINST(#{searchText} IN BOOLEAN MODE)
    </if>
    <if test="price != null">
        AND O.price = #{price}
    </if>
    <if test="searchText != null">
        AND pattern like CONCAT('%',#{searchText},'%')
    </if>
</trim>
 ```

 - concat example
 - conditional query with trim and if

여기서 trim은 2가지 일을 하는데 trim안의 컨텐츠가 있으면 prefix를 붙여주고, trim안에 sql문에 `prefixOverrides`에 있는 단어가 있으면 삭제해준다. 여기서는 AND나 OR가 된다.
마찬가지로 `suffixOverrides`는 끝에 붙는 단어를 삭제할 수 있다.
-


 ## 하나의 mapper에 여러개의 sql문 사용하기

 하나의 mapper에 아래처럼 복수개의 sql문을 사용할 수 있을까? 결론은 YES!

 ```xml
 <delete id="deleteById" parameterType="Integer">
        DELETE FROM ORDER WHERE product_id=#{value};
        DELETE FROM PRODUCT WHERE id = #{value}
    </delete>
 ```

 이것을 하려면 JDBC URL에 `allowMultiQueries=true`를 사용하면 된다. 아래처럼.

 > db.url=jdbc:mysql://127.0.0.1:33/orderdb?useSSL=false&useUnicode=true&characterEncoding=utf-8&autoReconnect=true&serverTimezone=UTC&allowMultiQueries=true

## 로깅

mybatis 쿼리가 제대로 동작안될때 Mapper안에 브레이크를 걸거나 할 수없기 때문에 디버깅이 어려워서 간단한 실수로 시간낭비를 하는 케이스가 꽤 있다.
이럴때 로깅 옵션을 enable해서 보면 도움이 된다.

logback 의 옵션은 아래처럼 mapper의 package name르 아래처럼 적어주면 된다.

```xml
<logger name="mapper.package.name" level="DEBUG"/>
```