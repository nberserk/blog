---
layout: post
title:  "implement fallback in sql script"
tags: mysql sql fallback
published: true
---

안드로이드에서 설정된 로케일이 en-US 일때 리소스를 찾는 순서는 en-US -> en -> default 이런식의 [fallback logic](https://developer.android.com/guide/topics/resources/multilingual-support.html) 이 있다. 그래서 최대한 specific 한 리소스를 먼저 보여주고 없으면 그다음 그다음 후보를 찾아서 표시해주는 식이 된다. 이런 fallback logic은 비교적 잘 알려져 있고, 다국어 표기시에 많이 쓰이는 방식이다.

이 fallback logic을 서버 단에서 한다고 생각하면 어떻게 구현할 수 있을까. 이것을 정리 해보자.

## schema

스키마는 단순화하여 아래처럼 정의 하고..

![schema]({{site.url}}/assets/fallback-erd.png)

[sql 스크립트]({{site.url}}assets/fallback.sql) 도 첨부.


## test data

아래처럼 테스트를 위한 데이터를 입력.

```sql
INSERT INTO LOCALE(locale) VALUES ('en'), ('en-US'), ('ko-KR'), ('ko');
INSERT INTO STRING(fqdn) VALUES ('store.hello');
SET @sid = LAST_INSERT_ID();
select * from STRING;
INSERT INTO LOCALIZED_STRING(string_id, locale_id, `i18n_string`) VALUES (@sid, 1, 'hello-en'), (@sid,2,'hello en-US'), (@sid,3,'안녕하세요 ko-KR'), (@sid,4, 'hello ko');
```

## fallback logic

sql 문의 union 문을 이용해서 fallback을 아래처럼 구현할 수 있다. 포인트는 `sequence` 변수로 우선순위를 준다는 사실. 아래에서 ORDER 부분을 빼고 실행하면 2줄이 나오는데 `LIMIT 1`으로 우선순위가 높은 것남 남기는 것임.

```sql
SELECT 1 as seq, LS.`i18n_string` FROM STRING S
  INNER JOIN LOCALIZED_STRING LS ON S.id=LS.string_id
INNER JOIN LOCALE L ON LS.locale_id = L.id
WHERE L.locale='en-US' AND S.fqdn='store.hello'
UNION
SELECT 2 as seq, LS.`i18n_string` FROM STRING S
  INNER JOIN LOCALIZED_STRING LS ON S.id=LS.string_id
  INNER JOIN LOCALE L ON LS.locale_id = L.id
WHERE L.locale='en' AND S.fqdn='store.hello'
ORDER BY seq ASC LIMIT 1;
```


## fallback logic - using GROUP BY

위의 경우는 UNION 을 사용했는데 `GROUP BY`를 사용할 수도 있다.

| id | locale | i18n_string |
|-|-|-|
|1 | NULL | dog |
|1 | ko-KR | 강아지 |
| 2 | NULL | sky |
| 2 | ko-KR | 하늘 |

위의 row에서 각 string의 ko-KR 을 먼저 사용하고 없으면 default(NULL)로 fallback 한다고 하면 아래처럼 sql 문을 작성할 수 있다. 이렇게 하면 우선순위가 떨어지는 locale(NULL)은 없어지고 가장 specific한 ko-KR의 번역된 스트링만 남게 된다.

```sql
SELECT id, locale, i18n_string
FROM
(SELECT S.id, L.locale, LS.i18n_string
FROM STRING S
INNER JOIN LOCALIZED_STRING LS ON S.id=LS.string_id
INNER JOIN LOCALE L ON LS.locale_id = L.id ORDER BY L.locale DESC) as SUB
GROUP BY id
```



## References





