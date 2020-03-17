---
title:  "반복문 내에서 array 합치기"
date:   2019-12-26 21:00:00 +0900
tags: [php]
---
## php 반복문 안에서 array 합치는 방법

php 반복문안에서 array_merge를 사용하게 되면 greedy하다는 경고를 얻게 된다.
array_push를 하면 한번씩 감싸주게 되어 원하는 결과를 얻을 수 없었는데,
php에서도 ...( spread operator )를 지원하고 있었다.
( array_push는 `$array[] = `을 사용하는 것을 권장한다.  )
```
    $users = array_chunk($users, 100);
    $result = [];
    foreach ($users as $user) {
        $result[] = $array();
    }
    $result = array_merge([], ...$result);
```

[php inspection 확인하기](https://github.com/kalessil/phpinspectionsea/blob/master/docs/performance.md#slow-array-function-used-in-loop)