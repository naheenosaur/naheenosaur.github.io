---
title:  "blue green deployment"
date:   2020-05-15 21:00:00 +0900
tags: [deploy]
key: 20200515_01
---


blue green deployment 방식

서버가 n개가 있을 때, 각 서버에는 set1, set2 존재.

(각 셋은 배포 된 spring boot 서비스를 의미)



\* nginX는 리버스 프록시 역할을 하면서 set1은 실서버 주소, 다른 set은 스테이징 주소로 연결

LVS는 N개의 서버에 대한 로드밸런싱



서비스 배포 방법

1. 최신 spring boot 프로그램을 스테이징으로 연결된 set2에 배포 ( depcon )

2. 스테이지에 배포된 이 후 이상이 없다고 판단되면 셋 전환
   --> set1을 스테이징으로 변경, set2를 실서버로 변경 ( setra )

3. 배포 후 모니터링 이 후 이상이 없다고 판단되면 나머지 set도 배포