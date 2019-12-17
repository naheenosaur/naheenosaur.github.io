---
#layout: post
title:  "docker의 port 마운트"
date:   2019-11-15 21:00:00 +0900
tags: [docker]
---
## docker의 port 마운트

docker 서비스를 이용하면서, 어떻게 port를 마운트 해 주는지 궁금해서 찾아봤다.

`iptables -t nat -L -n` 현재 어느 port를 listen 하고 있는지 확인할 수 있다.

-   **Chain PREROUTING** ( docker all ) / **POSTROUTING** ( MASQUERADE all ) / **OUTPUT** ( DOCKER all )
-   모든 포트를 docker가 listen 하면서 각 container에 매핑
-   **Chain DOCKER** 는 포트포워딩처럼 특정 포트로 들어오는 것을 ( ex.8000 포트 -> 도커의 80포트로 ) 변환해 주는 것
-   nginx에서 포트별로 서비스를 세팅한 경우, 동일한 port로 docker에서 실행해도 가능하지만  
    만약 docker container에서 3306과 같은 특정한 포트로 서비스 매핑이 필요한 경우 사용할 수 있다.