---
title:  "Static Class와 Static method"
date:   2020-02-15 21:00:00 +0900
tags: [java]
key: 20200215_01
---

Java(혹은 객체지향 프로그래밍)에서는 주로 static method의 사용을 지양하고 있다.
하지만 프로그래밍을 하다보면 NumberUtil과 같은 식의 Static method들의 모임인 Util성 클래스를 생성하는 것이 좋을 때가 있다.

왜 Java에서는 static method를 지양는지, 그렇다면 언제 이러한 Util성 클래스를 생성하는 것이 좋은지 정리하고 싶어서 구글링을 좀 해 봤다.

## Util class
객체지향 언어에서의 static method는 권장하지 않는 프로그래밍 방식이다. static method는 인스턴스를 생성하지 않는다.
일반적으로 class는 static 메모리 영역에, 인스턴스는 heap메모리 영역에 할당되게 된다.
static method와 같이 static 선언을 해 주게 되면 static 메모리 영역에 할당되는데 이 것은 모든 객체가 이 메모리를 공유한다는 것을 의미한다.
또, static 메모리가 할당 될 때 함께 선언하기 때문에 static 변수만 사용이 가능하다는 것을 의미한다.  
util 클래스를 사용하고자 할 때는 몇가지 권장하는 사항이 있다.

### 1. 생성자를 private로 선언해 준다.
```java
class NumberUtil {
    private static final int ZERO = 0;
    private NumberUtil() {}; // 생성자를 private으로 선언
    public static int add(int num1, int num2) {
        return num1 + num2;
    }  
}
```
생성자를 private로 선언해 주면 외부에서 new를 이용한 인스턴스 생성이 불가능해 진다.  
이렇게 되면 메모리를 공유하고 있는 데이터에 대한 접근을 제한해 신뢰성을 갖게 한다.

### 2. 외부 자원에 의존하지 않을 때 사용한다.

함수를 호출 하는 경우, 인자값에 따라 언제나 동일한 결과가 반환되어야 한다.
가장 많이 쓰는 util성 class 중 하나인 Math를 생각 해 보면 Math.max(1, 10) 은 언제나 10을 반환한다.
 
외부 자원에 의존 해야 하는 경우에 대해서는 조금 더 생각이 필요한데, 좋은 자료가 있어서 공유한다.

> 변화하는 외부 자원이 있고 그에 대한 모든 상태를 객체화해서 인자로 넘겨줄 경우에는 static 모음 클래스로 만들어도 좋다.  
> 더 쉽게 예를 들면.. EmailUtils.send(String smtpServer, ....) 이런 형태는 안된다.   
> 하지만 EmailUtils.send(Smtp smtp, ...) 이런 형태는 허용된다.  
> EmailUtils의 함수 안에서는 절대로 SMTP에 접속하면 안된다. 모든 외부 자원에 대한 접속과 행위는 Smtp 클래스의 객체를 통해 이뤄져야 한다.  
> ( 참고 자료 : [언제 static 함수 모음 Class를 만들어야 할까?](http://egloos.zum.com/kwon37xi/v/4844149) )

## Static Method
Util 성 클래스와 같은 편의를 위한 method 이외의 static method는 권장하지 않는 방식이다.
stack overflow에서 [좋은 글](https://stackoverflow.com/questions/7026507/why-are-static-variables-considered-evil)을 찾았는데, 
어떤 블로그에서 잘 [번역](https://unabated.tistory.com/m/entry/%EC%99%9C-%EC%9E%90%EB%B0%94%EC%97%90%EC%84%9C-static%EC%9D%98-%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%A7%80%EC%96%91%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94%EA%B0%80)해 주어 같이 첨부한다.

static method는 static variable에만 접근할 수 있는데, 이것은 캡슐화를 중요하게 생각하는 객체지향 언어에 적합하지 않다는 것이 가장 중요한 핵심이라고 생각한다. 
또, static 메모리에 할당되었기 때문에 프로그램이 실행되는 동안 계속 접근이 가능하기 때문에 Util성 클래스가 아닌 일반적인 서비스에는 적합하지 않다는 것이다.

함께 참고해 볼만한 자료 : [Why aren't static methods considered good OO practice?](https://stackoverflow.com/questions/4002201/why-arent-static-methods-considered-good-oo-practice)

## Util class 의 사용
util성 클래스를 만드는 방법은 static method 를 사용하는 것 이외에도 singleton 형식이 있을 수 있다.
spring framework 에서 개발한다면 spring 에서 제공해주는 singleton 기반으로 Bean을 등록해서 사용하는 것이 더 좋아 보인다.