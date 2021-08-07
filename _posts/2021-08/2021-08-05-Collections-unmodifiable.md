---
title:  "Collection의 위조 방지"
date:   2021-08-05 21:00:00 +0900
tags: [java, collection]
key: 20210805_01
---
## Collection의 unmodifiable

Collections.unmodifiableMap(), Collections.unmodifiablelist() 등
Collection 에서 제공하는 unmodifiable 기능은 현재의 collection을 read-only로 리턴하여 외부에서 값을 변조할 수 없도록 한다.

Java 에서 stream의 생성은 기존에 사용하고 있는 리스트와 별개의 새로운 리스트이다.
stream을 사용하지 않고 collection을 return하는 부분에서는 collection에서 제공하는 unmodifiable을 사용할 수 있다.
단, stream 이나 unmodifiablelist나 모두 내부 element는 주솟값을 찹조하고 있어 변경 가능하다.

물론 `new ArrayList<>(unmodifiableList)`를 통해 deep copy된 list는 수정 가능하다.

## 참고자료
- [collections unmodifiablelist method](https://www.geeksforgeeks.org/collections-unmodifiablelist-method-in-java-with-examples/)

- [why is it better to return unmodifiablelist from a method where list is created](https://stackoverflow.com/questions/51881801/why-is-it-better-to-return-unmodifiablelist-from-a-method-where-list-is-created)
