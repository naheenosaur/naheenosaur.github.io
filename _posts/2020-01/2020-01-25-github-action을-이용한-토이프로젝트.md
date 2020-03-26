---
title:  "[토이프로젝트] github action을 이용한 잔디지키미"
date:   2020-01-25 21:00:00 +0900
modify_date: 2020-01-26
tags: [commit-every-single-day]
key: 20200125_01
---

[commit-every-single-day github 바로가기](https://github.com/naheenosaur/commit-every-single-day)  
[deploy된 페이지 보기](https://naheenosaur.github.io/commit-every-single-day)

## commit-every-single-day 시리즈
1. [주제정하기](https://naheenosaur.github.io/토이프로젝트-주제정하기-1)  
2. [github action 알아보기]()  
3. [node.js에서 github action 라이브러리 사용하기]()  
4. [slack webhook 사용하기](https://naheenosaur.github.io/slack-webhook)  
5. [github action에서 github page 배포하기](https://naheenosaur.github.io/deploy-github-page)  
6. [github action에서 input값에 변수 사용하기](https://naheenosaur.github.io/variables-for-github-action)  


github에 심어놓은 잔디 (github contribution graph)는   
https://ghchart.rshah.org/naheenosaur 의 식으로 그냥 가지고 올 수 있기 때문에 정적인 페이지에서 보여 주기 적합하다.
이미지로 바로 보여주고 있어 [about 페이지](https://naheenosaur.github.io/naheenosaur)에도 추가했다.

github은 공개된 내용에 한정하여 token을 발급받지 않아도 되는 API를 제공하고 있다.  
https://api.github.com/users/:username/events 에서 _public repository_ 에 해당하는 이벤트들을 가져온다.  
내가 필요한 것은 가장 마지막 commit 데이터이기 때문에 "type": "PushEvent" 인 아이들을 가져온다.  
1. 이름 : repo.name
2. 커밋 : payload.commits ( 오래된 순서대로 정렬되기 때문에 마지막을 가져온다. )
    1. 커밋 내용 : payload.commits[length-1].message
    2. 마지막 커밋 일 payload.commits[length-1].url --> commit.author.date
    
github action에서는 cron작업을 추가할 수 있는데, cron에 작성하는 시간은 UTC시간을 사용한다.
나는 밤 11시에 예약작업을 내고 싶었기 때문에 ( 0 14 * * * ) 에 예약작업을 걸었다.



