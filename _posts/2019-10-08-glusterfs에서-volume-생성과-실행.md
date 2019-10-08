---
#layout: post
title:  "glusterfs에서 volume 생성과 실행"
date:   2019-10-08 21:00:00 +0900
categories: etc
tags: [glusterfs]
---
# glusterfs에서 volume 생성과 실행

1.  볼륨 생성
    -   `gluster volume create dir replica 2 transport tcp 서버1:/tmp/brick-dir 서버2:/tmp/brick-dir`
    -   볼륨명과 replica를 걸려는 서버, 마운트하려는 경로 설정
2.  `gluster volume start dir`
3.  `mount.glusterfs localhost:dir /tmp/dir`