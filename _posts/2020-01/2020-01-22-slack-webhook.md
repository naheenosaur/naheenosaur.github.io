---
title:  "slack webhook 사용하기"
date:   2020-01-22 21:00:00 +0900
tags: [commit-every-single-day, slack]
key: 20200122_01
---

서비스 로직에서 나온 결과에 따른 slack message를 전송하기 위해 slack webhook 기능을 이용했다.

slack 개발자 모드에 들어가게 되면 자세하게 설명이 나와있다. 
이상하게 내 local에서 curl로 요청을 보내면 정상적으로 message가 발송되는데, 
github action의 container 상태에서 curl발송 시 권한 에러가 발생한다. 

일부 서버에서의 발송에 대해 slack에서 서버단에서 차단을 했을 것이라고 예측된다.

그래서 incoming webhooks을 만든 이후에, `8398a7/action-slack@v2` 을 사용했다.

```
# slack incoming webhook 생성하기
1. Search 'Incoming WebHooks'
https://{{your_workspace}}.slack.com/apps/
2. Add to slack
3. Custom your WebHook information
4. Get WebHook URL

```
