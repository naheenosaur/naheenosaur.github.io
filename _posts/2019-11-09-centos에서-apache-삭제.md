---
#layout: post
title:  "centos에서 apache 삭제"
date:   2019-11-09 21:00:00 +0900
categories: linux
tags: [centos]
---

# centos에서 apache 삭제

apache가 설치되어 있을 때 nginx 설치 하려면 apache를 삭제 해 주어야 한다.  
`service apache2 stop`  
`whereis apache2` 로 나오는 것 `rm -rf`로 삭제  
`ufw app list`에 계속해서 보이면 `rm -R /etc/ufw/applications.d/apache2*`