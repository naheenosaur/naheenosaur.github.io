---
#layout: post
title:  "dockerfile에서 사용하는 run, cmd, entrypoint, cmd의 차이"
date:   2019-11-23 21:00:00 +0900
categories: docker 
tags: [docker]
---
# run / cmd / entrypoint / 커맨드 라인

1.  run
    
    -   실행 시점 : image 생성 단계에서 실행
    -   특징 : 보통 설치나 환경변수 설정을 한다.
    -   예제 : `RUN apt-get update`
2.  cmd
    
    -   실행 시점 : docker run 단계에서 컨테이너에서 실행
    -   특징
        -   기본세팅을 할 수 있다.
        -   커맨드 라인으로 오버라이딩할 수 있다.
        -   한 줄 만 사용 가능하다. ( 마지막 명령만 실행 )
    -   예제 : `CMD ["/bin/echo", "service started"]`
3.  entrypoint
    
    -   실행 시점 : docker run 단계에서 컨테이너에서 실행
        
    -   특징
        
        -   커맨드 라인으로 오버라이딩 할 수 없어 의도하지 않은 실수를 하지 않도록 무조건 실행해야 하는 것에 사용
    -   예제
        
        ```
              # DOCKER FILE
              ADD entrypoint.sh /entrypoint.sh
        
              # entrypoint.sh
              echo "service started"
        ```
        
4.  커맨드 라인
    
    -   docker run 단계에서 실행
    -   특징
        -   cmd 실행 내용을 오버라이드 할 수 있다.
        -   서비스 별로 커스텀 가능한 설정값에 이용한다.