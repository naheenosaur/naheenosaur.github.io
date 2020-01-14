---
#layout: post
title:  "Javascript에서 timezone 사용하기"
date:   2020-01-14 21:00:00 +0900
tags: [javascript]
---

## moment-timezone 라이브러리 사용하기

```javascript 1.8
const moment = require('moment-timezone');
moment().tz('Asia/Seoul') 
```
Java에서 datetime연산을 할 때 [Joda-Time](https://www.joda.org/joda-time/)을 사용하는 것 처럼 
javascript에서는 [moment-timezone](https://momentjs.com/timezone/)을 사용한다.
