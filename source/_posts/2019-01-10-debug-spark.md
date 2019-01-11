---
title: how to debug EMR spark job
tags: 
- emr
- spark
---

aws EMR에서 띄운 spark를 디버깅 하는 팁..

1. [일단 debuggin option을 켜고](/2019/01/07/enable-debuggin-emr), emr을 론치할때 LogUri로 s3 path를 지
정함
2. 그러면 <bucket>/logs/<cluster_id>/containers/application_<timestamp>_<seq>/container_<timestamp>_xx_xx_<seq> 이런 형식으로 로그가 저장된다. 여기서 application_<seq>는 1씩 증가되면서 붙으니 자신이 돌린 잡의 seq를 확인하여 container의 첫번지 seq log를 보면 executor log가 보인다. 이것으로 모든 로그를 볼 수 있음



