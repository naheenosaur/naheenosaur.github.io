---
title:  "mysql에서 event scheduler에서 에러가 있다고 나오는 경우"
date:   2019-12-21 21:00:00 +0900
tags: [mysql]
---
## mysql에서 event scheduler가 에러가 있다고 나오는 경우

Cannot proceed because system tables used by Event Scheduler were found damaged at server start
라고 나오는 경우

_/var/lib/mysql/mysql_ 에서
`mysqlcheck --auto-repair --all-databases --user 유저명 --password`
`systemctl restart mysqld`

## mysqlcheck

table을 유지 보수하는데 사용하는 클라이언트 프로그램 ( 확인, 복구, 분석의 역할 수행 )
1. 서비스가 올라와 있는 상태에서만 사용 가능
2. 작업 진행 중에 테이블은 read-only상태
- `mysqlcheck --auto-repair` : 테이블이나 기타 손상된 부분을 자동 복구
- `mysqlcheck --auto-repair _DB명_ --user 유저명 --password` : 특정 데이터베이스
- `mysqlcheck --auto-repair --all-databases --user 유저명 --password` : 전체 데이터베이스
- `mysqlcheck --optimize` : 데이터베이스 최적화

3. 이벤트 스케줄러에 문제가 있다고 나오면
- mysql_upgrade --user 유저명 --password 를 하라고 함
- 이 구문은 mysql자체가 아닌 테이블을 업그레이드 하는것
- 버전에 상관 없이 dump데이터를 쌓고나면 업스레이드 해주는 것을 권장한다. ( 버전 호환이 안되는 경우가 있기 때문 )
- mysql_upgrade를 실행하면 mysql_upgrade_info가 생겨서 "이미 업그레이드가 완료되었다." 라고 나오기 때문에  
  mysql_upgrade --force를 사용하던가 muysql_upgrade_info를 삭제하고 재 실행
    
