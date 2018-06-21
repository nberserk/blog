---
title: mybatis mapper
date: 2018-06-19 18:23:13
tags: mybatis
---

 주로 spring과 함께 사용하는 mybatis. java persistence framework으로서 Hibernate 같은 미들웨어 되겠다.

 ## Dynamic SQL

 ### trim

 ```sql
 SELECT * FROM ORDER O
<trim prefix="WHERE" prefixOverrides="AND|OR">
    <if test="searchText != null">
        AND MATCH(O.name) AGAINST(#{searchText} IN BOOLEAN MODE)
    </if>
    <if test="price != null">
        AND O.price = #{price}
    </if>
</trim>
 ```

- trim은 2가지 일을 하는데
  - trim안의 컨텐츠가 있으면 prefix를 붙여주고
  - trim의 컨텐츠 앞에 AND나 OR가 붙으면 처음 것은 삭제한다.


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
