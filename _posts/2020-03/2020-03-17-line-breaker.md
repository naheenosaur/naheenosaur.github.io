---
title:  "text파일의 마지막에 공백 라인 추가하기"
date:   2020-03-17 21:00:00 +0900
tags: [os]
key: 20200317_01
---

## line breaker

얼마 전 코드를 정리하는 단계에서 소스코드에 마지막 줄에 생기는 공백라인이 눈에 거슬려서 모두 지운 적이 있다.
리뷰 단계에서 지적을 받게 되며 처음 알게 된 line breaker 라는 개념이다.
알고보니 내가 인지하지 못했던 것이지 
github에서 커밋 로그를 확인해 보면 빨간 금지표시로 `No newline at end of file`이라며 알려주고 있다.

[참고자료](https://stackoverflow.com/questions/729692/why-should-text-files-end-with-a-newline)

IEEE의 unix표준에 따르면 `가장 마지막에 빈 공백 라인이 없는 경우 끝나지 않는 것으로 여긴다`는 항목이 있다.
공백 라인을 추가하지 않은 파일의 경우 프로토콜에 맞지 않기 때문에 잠재적인 문제가 발생할 수 있다.
