---
title: dockerize angular + spring api
date: 2018-08-20 17:02:54
tags: 
- angular
- docker
---

docker로 angular app을 deploy하는 방법. front와 backend의 url이 달라지기 때문에 cors 문제가 생기는데 이를 회피하는 방법은 여러가지가 있다. [CloudFront를 사용할 수도 있고](https://codeburst.io/deploy-angular-2-node-js-website-using-aws-1ac169d6bbf) 여기서는 ngnix proxy기능을 사용해서 회피하려고 한다.


# docker-compose

```
version: '3'
services:
  ng:
    container_name: ng
    image: web-nginx:0.1
    ports:
      - "80:80"
    networks:
      - app-network
  api:
    container_name: api
    image: api-backend:0.1
    ports:
      - "8088:8088"
    networks:
      - app-network
networks:
  app-network:
    driver: bridge
```

docker image 빌드한후 aws ec2 instance 에서 위와 같은 docker-compose.yml을 생성한후 `docker-compose up`을 하면 nginx와 api docker image가 실행된다. 

- bridge network을 사용한 것은 ng와 api 간의 네트웍을 가능하게 하기 위해서 이다.
- nginx를 사용한 이유는 static file은 nginx로 서빙을 하고, api는 스프링으로 실행을 하기때문에 cors이슈가 발생하는데 이것을 nginx의 proxy기능을 이용해서 cors문제를 회피하기 위해서 이다. nginx conf는 아래에 설명할 것임


## nginx conf

```
server {
    listen       80;
    server_name  localhost;
    charset utf-8;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location /backend {
	    rewrite ^/backend/(.*) /$1 break;
        proxy_pass   http://api:8088;
        #proxy_pass_request_headers on;
        #proxy_set_header Authorization $http_authorization;
        #proxy_pass_header  Authorization;
        break;
    }

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }    
}

```

위는 일반적인 nginx conf 인데, api:8080으로 proxy가 설정된 것만 유의해서 보면 될듯 하다.

- rewrite로 `/backend/xxx`로 시작하는 url을 `/xxx`로 바꿔서 api:8080으로 라우팅 해준다. 여기서 api는 docker-compose에서 설정된 api docker instance를 의미한다. 




