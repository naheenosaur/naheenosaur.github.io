---
#layout: post
title:  "intelliJ에서 war 혹은 jar 파일 열어보기"
date:   2019-11-04 21:00:00 +0900
categories: ide
tags: [intellij]
---
# jar, war 파일 gradle 추가 및 내용 보기

프로젝트 내부에 libs 폴더 생성 후 jar / war 파일 이동

```
    #build.gradle
    compile files("libs/파일이름")
```

gradle 재 빌드 하면 gradle에 추가가 되는데, 추가되면 intelliJ에서 해당 파일을 열어볼 수 있다.