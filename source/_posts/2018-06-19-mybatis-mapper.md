---
title: mybatis-mapper
date: 2018-06-19 18:23:13
tags: mybatis
---

 주로 spring과 함께 사용하는 mybatis. java persistence framework으로서 Hibernate 같은 미들웨어 되겠다.

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
