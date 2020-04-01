---
title:  "stream과 for문의 사용"
date:   2020-02-24 21:00:00 +0900
tags: [java, stream]
key: 20200224_01
---
## stream 에서의 foreach

stream api의 foreach는 출력과 같은 단계에서 사용하는 것을 권장한다.
반복적인 로직을 수행해야 한다면 foreach보다는 반복문을 사용하는것이 객체지향적인 설계를 가능하게 한다.

foreach에서 수행하는 로직의 경우 동시성을 보장하기 어려워지고, 가독성이 떨어진다.
이와 관련해서는 [effective java의 46번째 항목](https://naheenosaur.github.io/review/book/effective-java-3#아이템-46-스트림에서는-부작용-없는-함수를-사용하라)에서도 언급되어있다.

또한 stream api는 지연연산을 하고 있어 기대하지 않은 결과가 나올 수 있다.

### 참고할 만한 글

1. [for loop를 Stream forEach로 바꾸지 말아야 할 3가지 이유](https://homoefficio.github.io/2016/06/26/for-loop-를-Stream-forEach-로-바꾸지-말아야-할-3가지-이유/)
2. [stream은 loop가 아니다](https://www.popit.kr/java8-stream은-loop가-아니다/)
