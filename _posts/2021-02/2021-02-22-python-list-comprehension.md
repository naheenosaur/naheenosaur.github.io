---
title:  "python의 list comprehension 사용"
date:   2021-02-22 21:00:00 +0900
tags: [python]
key: 20210222_02
---

### filter + lambda 보다는 list comprehension를 사용

[https://stackoverflow.com/questions/3013449/list-comprehension-vs-lambda-filter](https://stackoverflow.com/questions/3013449/list-comprehension-vs-lambda-filter)

```
In [1]: timeit import timeit

In [2]: def f1(seq):
   ...:    return list(filter(lambda x: x, seq))

In [3]: def f2(seq):
   ...:    return [i for i in seq if i is not None]

In [4]: timeit(stmt="f1(range(100))", setup="from __main__ import f1,f2")
6.67694592476

In [5]: timeit(stmt="f2(range(100))", setup="from __main__ import f1,f2")
4.96091508865

```