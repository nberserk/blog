---
layout: post
title:  "elastic search and kibana"
tags: elasticsearch kibana
published: true
---

[ELK 스택](https://www.elastic.co/webinars/introduction-elk-stack)으로 많이 알려져있고, Elastic search(이하 ES) + LogStash + Kibana 의 조합으로 많이 사용한다. ES는 document DB이고, logStash는 로그를 정제하여 ES에 전달하는 역할, Kibana는 로그들을 필터링 및 조합하여 visualization을 하는 역할을 한다. 특히 타임 series 데이터를 보여주는데 적합하고 멋들어지게 보여줄 수 있어 선호되고 있다.

# 설치
AWS에서는 [Elasticsearch Service](https://aws.amazon.com/elasticsearch-service/)로 따로 존재하고 비교적 최신 버전의 ES와 Kibana를 설치되어 있어 사용하기 편리하다.

아니라면, [official site](https://www.elastic.co/products)에서 ES와 Kibana를 따로 다운 받아서 쉽게 돌려볼수 있다. JAVA만 설치되어 있다면 별다른 어려움은 없다.

# index

index는 RDB의 table개념이고 table의 shcema는 mapping이라고 불린다. 각각의 row는 document라고 한다.

index 생성 및 추가는 모두 rest api 방식으로 할 수 있고, PUT으로 index 생성, POST로 data를 추가할 수 있다.

아래처럼 PUT으로 `<server>/twitter` 로 생성해주면, `<server>/twitter` POST로 데이터를 추가할 수 있다.

```json
PUT twitter
{
  "mappings": {
    "tweet": {
      "properties": {
        "message": { "type": "text" },
        "user": { "type":"text" },
        "created": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss||epoch_millis"
            }
      }
    }
  }
}
```

# Kibana

위에서 들어온 데이터를 Kibana로 visualize해주면 되는데 kibana의 직관적인 UI가 훌륭하다. 리프레쉬 간격도 설정 가능하고 시간간격도 자동으로 조절해준다. 그래프 수준은 훌륭하고 손쉽게 다양한 옵션으로 그래프를 그릴수 있다.

![kibana time line]( {{site.url}}/assets/kibana-time.png)

전체적인 흐름은 Management 에서 index설정을 하고, Discover에서 실험을 한후, visualize에서 시각화 설정을 해주고, dashboard에서 시각화한 요소들을 조합해서 한번에 볼수 있게 해주면 된다. 아래는 visual builder로 2개의 값을 비교해서 시간 축으로 나타낸 것이다.

![kibana time line]( {{site.url}}/assets/kibana-visualbuilder.png)


## query string


```
worker:seoul*    : worker filed가 seoul* 를 만족하는 쿼리

```













