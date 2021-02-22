---
title:  "python에서 append를 변수화"
date:   2021-02-22 21:00:00 +0900
tags: [python]
key: 20210222_01
---

### append를 변수화
python에서 append는 강력하게 느껴진다.
list에서 append를 사용하는 부분에서 성능 개선을 위해 고민하던 중, append를 변수화 하여 사용하면 효율이 더 좋다는 이야기에 확인해 보았다.

```
In [1]: import dis
In [2]: from timeit import timeit
In [3]: def f1():
   ...:     l = []
   ...:     for i in range(3):
   ...:         l.append(i)
   ...:
In [4]: def f2():
   ...:     l = []
   ...:     append = l.append
   ...:     for i in range(3):
   ...:         append(i)
   ...:
In [5]: dis.dis(f1)
  2           0 BUILD_LIST               0
              3 STORE_FAST               0 (l)  3           6 SETUP_LOOP              33 (to 42)
              9 LOAD_GLOBAL              0 (range)
             12 LOAD_CONST               1 (3)
             15 CALL_FUNCTION            1
             18 GET_ITER
        >>   19 FOR_ITER                19 (to 41)
             22 STORE_FAST               1 (i)  4          25 LOAD_FAST                0 (l)
             28 LOAD_ATTR                1 (append)
             31 LOAD_FAST                1 (i)
             34 CALL_FUNCTION            1
             37 POP_TOP
             38 JUMP_ABSOLUTE           19
        >>   41 POP_BLOCK
        >>   42 LOAD_CONST               0 (None)
             45 RETURN_VALUE
In [6]: dis.dis(f2)
  2           0 BUILD_LIST               0
              3 STORE_FAST               0 (l)  3           6 LOAD_FAST                0 (l)
              9 LOAD_ATTR                0 (append)
             12 STORE_FAST               1 (append)  4          15 SETUP_LOOP              30 (to 48)
             18 LOAD_GLOBAL              1 (range)
             21 LOAD_CONST               1 (3)
             24 CALL_FUNCTION            1
             27 GET_ITER
        >>   28 FOR_ITER                16 (to 47)
             31 STORE_FAST               2 (i)  5          34 LOAD_FAST                1 (append)
             37 LOAD_FAST                2 (i)
             40 CALL_FUNCTION            1
             43 POP_TOP
             44 JUMP_ABSOLUTE           28
        >>   47 POP_BLOCK
        >>   48 LOAD_CONST               0 (None)
             51 RETURN_VALUE
In [7]: timeit(stmt='f1()', setup='from main import f1, f2')
0.600948095322
In [8]: timeit(stmt='f2()', setup='from main import f1, f2')
0.536416053772

```