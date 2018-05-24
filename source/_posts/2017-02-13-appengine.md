---
layout: post
title:  "Google appengine으로 무료로 웹 어플리케이션 사용하기"
tags: google appengine
published: false
---

Google App Engine(GAE)은 Google Cloud에서 제공하는 확장 가능한 웹어플리케이션 프레임웍이다.

크게 [Standard Environment와 Flexible Environment](https://cloud.google.com/appengine/docs) 의 두가지 환경을 선택할 수 있는데, 전자는 제약이 있는 반면에 자동화를 더 제공하고, 후자는 자유도를 더 주는 환경이다. 전자는 라이브러리 버전이나 선택에 제한이 있다.

현재는 Java, Python, PHP, Go언어 등을 지원하고, python은 flask, Java는 servlet등으로 구현할 수 있게 되어 있다.

나는 Standard Environment를 사용할 것인데, 그 이ㅠ는 서비스의 트래픽/데이터 quote를 넘지 않으면 무료로 사용할 수 있기 때문이다. 대부분의 토이 프로젝트에는 충분한 양이 될 것이다.

## Architecture



