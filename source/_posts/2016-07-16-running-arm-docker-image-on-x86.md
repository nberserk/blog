---
layout: post
title:  "running arm dockerimage on x86"
tags: docker
---

docker v1.12 버전 부터는 container에 qemu가 설치만 되어 있으면 Mac(x86)에서 arm docker image를 실행할 수 있다.

좋구나.
이전에는 qemu-arm-static trick을 사용했어야 했다. https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/ 참고.
https://docs.docker.com/docker-for-mac/multi-arch/
