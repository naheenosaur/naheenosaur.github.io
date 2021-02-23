---
title:  "Idiomatic Python:EAFP versus LBYL"
date:   2021-01-30 22:00:00 +0900
tags: [python]
key: 20210130_03
---

# Idiomatic Python:EAFP versus LBYL

# 자연스러운 Python: EAFP와 LBYL

[원본]([https://devblogs.microsoft.com/python/idiomatic-python-eafp-versus-lbyl/](https://devblogs.microsoft.com/python/idiomatic-python-eafp-versus-lbyl/)) 의 번역문

예외처리를 고려하는 다른 프로그래밍 언어를 사용하던 사람들은 Python에서 놀라는 자연스러운 관습은 EAFP ( " It's Easier to Ask for Forgiveness than Permission, 권한보다 용서를 구하는 것이 더 쉽다" ) 이다. 짧게 말해 EAFP 는 내가 하고자 하는일을 그냥 하고, 만약 그 과정에서 예외가 발생하면 예외를 받아서 그 때 처리하는 것이다. 대부분의 사람들이 사용하는 방식은 LBYL ("Look Before You Leap, 뛰어들기 전에 한번 봐라") 이다.  EAFP와 비교해 봤을 때, LBYL은 어떤일이 성공할 지에 대해 먼저 확인을 하고, 동작 할 거라고 생각되는 것만 수행하는것이다.

만약 글로 이해가 되지 않아도 코드를 보면 금방 이해가 될 것이다. dictionary에서 특정한 키가 있을 지 없을 지 모르는 상태에서 값을 받아오는 상황이라고 생각해 보자, LBYL 에서는 일단 key 가 있는지 확인을 한다.

```python
if "key" in dict_:
    value += dict_["key"]
```

이렇게 논리적이게 발생할 수 있는 `KeyError` 예외를 방지한다. 

이 코드가 사용자에게 제공하는 것은 예외적인 케이스가 key가 true값이 아닐수도 있는 게 없을 수도 있는 게 아니고, dictionary 가 key를 갖고 있다는 것이다. 일반적인 경우에 대한 코멘트가 없으면 내가 쓴 코드가 아니라면 어떤 게 참일 지 모를 수 있다. 아니면 dictionary에 key가 있든 없든 실제로는 중요하지 않을 수 있는데 강조될 수 있다.

하지만 key가 dictionary에 있을 확률이 높고, 예외처리가 중요하지 않은 경우에는 어떨까? EAFP 같은 코드를 key 가 있는지 없는지 별로 신경 쓰지 않고 다른 방식으로 작성할 수 있게 해 준다.

```python
try:
    value += dict_["key"]
except KeyError:
    pass
```

이 코드가 의미하는 게 뭘까? key는 아마 dictionary에 있을텐데 어쩌면 없을 수도 있다고 말하는 것 같다. 만약 키가 없어도 큰 문제가 되지는 않고, 있는 경우에 함수에서 호출되어 사용될 것이다. 코드가 의미하는 것을 개발자에게 명확하게 말하는 것은 Python에서 매우 중요해서, key의 존재가 얼마나 일반적인지/드문지/중요한지 오해의 소지가 있는 LBYL스타일 보다 선호하는 이유이다.

어쩌면 사람들은 이 문서를 읽고 EAFP 버전이 LBYL보다 더 길고 명시적이라고 말할 것이다. 사실이다. 하지만, "explict is better than implicit, 명시적인것이 모호한 것보다 낫다"는 [guideline in the design of Python itself](https://www.python.org/dev/peps/pep-0020/) 의 핵심이기 때문에 이러한 명시적인 코드가 좀 더 목적에 맞다.

명시적이고 길다는 것 보다 다른사람들이 걱정하는 것은 EAFP의 효율성이다. 만약 예외를 발생시키는 것이 무리가 되는 다른 언어를 사용하다 Python을 접했다면 이러한 걱정을 하는게 마땅하다. 하지만 예외는 EAFP에서 제어의 흐름을 관리하기 위해 사용되기 때문에 Python 실행은 예외를 값싼 행위로 만들어서 코드를 짤 때 예외의 비용에 대해서는 고려하지 않아도 된다.

EAFP에 대해 한가지 하지 말아야 되는 것은 예외를 잡는데 사용되는 모든 코드에 적용되는 것으로, 너무 많은 코드를 `try` 블럭에 넣는 것을 피하는 것이다. 예를들어 아래와 같이 코드를 쓰고 싶을 수 있다.

```python
try:
    do_something(dict_["key"])
except KeyError:
    pass
```

이 코드의 문제는 내가 의도하지 않았는데 마음대로 `do_something()` 이 `KeyError` 를 발생시킬 수 있다는 것이다. 이 경우에 내가 예외 케이스에 대해 고려하고 있는 게 어떤 것인지 더 명시적으로 나타내 주어야 한다.

```python
try:
    value = dict_["key"]
except KeyError:
    pass
else:
    do_something(value)
```

내가 예외가 발생할 수 있다고 생각했던 코드블록이 있다면, `try` 블럭에서 수행하고자 하는 코드를 양껏 분리하는 것이 좋다. 만약 EAFP가 너무 명시적으로 보이는 상황에서 LBYL를 선호한다면, LBYL를 사용해도 아무 문제 없다.

```python
if "key" in dict_:
    do_something(dict["key"])
```

이 글의 핵심은 EAFP가 괜찮은 코딩 스타일이니 쓸 수 있는 곳에서는 자유롭게 사용하라는 것이다.