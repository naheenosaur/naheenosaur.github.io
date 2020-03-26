---
title:  "함수형 인터페이스와 @FunctionalInterface"
date:   2020-03-02 21:00:00 +0900
tags: [java]
key: 20200302_01
---

## 함수형 인터페이스

함수형 인터페이스란 람다 표현식을 사용하기 위한 하나의 방법이다.

```java
@FunctionalInterface
public interface Condition {
    boolean is();
}
```

```java

public class BooleanUtil {
    private BooleanUtil() {}
    public static boolean randomValue() {
        Random random = new Random();
        return value(is -> random.nextBoolean()); // value(random::nextBoolean)와 같이 람다 표현식 가능
    }
    
    private static boolean value(Condition condition) {
        return condition.is();
    }
}
```

`Condition`이라는 interface를 생성했고, Condition 인터페이스 내부에는 난 하나의 추상 메서드인 `boolean is()`가 존재한다.
BooleanUtil 클래스의 `randomValue` 함수를 확인해 보면 `value(is -> random.nextBoolean());` 라고 value 함수를 호출하는데 
value 함수의 반환 값인 Condition::is를 `Random::nextBoolean()` 을 통해 나온 값으로 미리 지정한다.

## @FunctionalInterface

@FunctionalInterface 어노테이션을 명시하지 않아도 정상적으로 람다 표현식을 사용할 수 있다. 
만약 어노테이션을 사용한다면 컴파일 단계에서 함수형 인터페이스로 사용된 interface임을 확인하고, 메서드가 2개 이상일 때 컴파일 에러를 반환해 준다.