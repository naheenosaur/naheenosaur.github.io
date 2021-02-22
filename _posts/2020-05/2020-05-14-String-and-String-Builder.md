---
title:  "java의 string builder와 string"
date:   2020-05-14 21:00:00 +0900
tags: [java]
key: 20200514_01
---

# String builder / String

대략 찾아보니 String을 만드는 방법은

Stirng builder를 만드는 것과 String을 만드는 것 두가지로 만들어 진다.



String builder는

```java
StringBuilder resuilt = new StringBuilder();
result.append("어쩌구");
result.append("저쩌구");
```

String은

```java
String result = "어쩌구";
result += "저쩌구";
```

혹은

```java
String result = "어쩌구";
result.concat("저쩌구");
```



둘 중 어떤것을 쓰는것이 시간이나 메모리적으로 효율적일까 찾아보니,

실제로 String 을 만들고 연산하는 과정이 String builder 연산보다 훨씬 효율성이 떨어졌는데,

java 8 부터는 단순한 String 연산은 java 자체 내에서 string builder를 만들어 사용한다고 한다.