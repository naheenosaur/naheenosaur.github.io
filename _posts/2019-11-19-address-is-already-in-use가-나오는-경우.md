---
#layout: post
title:  "intellij 실행 시 address is already in use가 나오는 경우"
date:   2019-11-19 21:00:00 +0900
categories: windows 
tags: [windows]
---
# intellij 실행 시 address is already in use 메세지

프로젝트를 실행 할 때, 80이나 8080포트로 실행할 경우 가끔 해당 에러 메세지가 나오는 경우

1.  `netstat -ano | find ":80"` 혹은 `netstat -ano | find ":8080"`
2.  `taskkill /F /PID 숫자`

만약 pid 4로 나오는 것이 계속 떠 있으면 iis같은 서비스 종료 해 줌  
`net stop httpd`

nginx사용중이면 nginx종료