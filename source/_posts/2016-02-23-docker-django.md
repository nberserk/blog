---
layout: post
title:  docker로 django개발 환경 만들기
tags: docker django
---

docker 공홈에 있는 [django + postgres 의 예제](https://docs.docker.com/compose/django/)를 django + mysql로 바꾸어서 해보았다.

## mysql

docker repository [mysql로 검색](https://hub.docker.com/_/mysql/)을 해보면 여러가지가 나오는데 mysql:5.7로 해보자.
아래처럼 실행하면 3306 포트로 mysql 데몬이 뜬다. 
`docker run -e MYSQL_ROOT_PASSWORD=root mysql:5.7`


## django

Dockerfile은 아래처럼.

```txt
FROM python:2.7
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/
```

requirements.txt는 아래처럼 작성한다.

```txt
Django
psycopg2
MySQL-python
```

이렇게 하면 python:2.7 docker image에 pip로 requirements.txt에 있는 패키지를 설치해서 django ready상태가 되고 호스트의 WORKDIR을 /code로 마운트해서 장고 앱을 실행할 수 있는 상태로 만든다.
그 후 아래 명령을 실행하면 장고가 프로젝트 템플릿을 만들어 준다. MyApp 하위에

`docker-compose run web django-admin.py startproject MyApp .`

그후 mysql을 연결하는 작업을 해야 하는데 MyApp/settings.py 파일의 DATABASES 부분을 아래처럼 수정.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django',
        'USER': 'django',
        'PASSWORD': 'django',
        'HOST': 'db',
        'PORT': 3306,
    }
}

```

## docker-compose

위에서 mysql container와 django container는 준비가 되었다. 매번 이렇게 2번의 명령으로 컨테이너를 실행하면 번거러울수 있다. 컨테이너가 더 많아질수록 그렇게 될텐데.. 이런 경우를 위해서 docker-compose라는 cli를 제공하고 있다.

아래처럼 docker-compose.yml을 작성하고

```txt
db:
    image: mysql:5.7
    environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_DATABASE=django
    - MYSQL_USER=django
    - MYSQL_PASSWORD=django
web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
        - .:/code
    ports:
        - "8000:8000"
    links:
        - db
```

`docker-compose up` 명령을 내리면 두개의 docker 컨테이너를 동시에 실행할 수 있다.

잘 동작하는지 확인하려면 http://localhost:8000 포트로 접속해보면 된다. 맥이나 윈도우즈의 경우는 `docker-machine ip` 명령으로 나오는 ip의 8000 로트로 접속해보면 장고 초기화면이 나온다. 나의 경우는 http://192.168.99.100:8000/ 이었다. 


### revision history
* 2016/2/23 draft
