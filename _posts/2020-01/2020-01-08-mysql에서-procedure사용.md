---
title:  "mysql에서 procedure 사용"
date:   2020-01-08 21:00:00 +0900
tags: [mysql]
key: 20200108_01
---

## mysql에서 프로시저 생성

```sql
DELIMITER $$
USE `{DB명}`$$
DROP PROCEDURE IF EXISTS `{프로시저명}`$$ # 기존에 사용하고 있던 프로시저 있으면 삭제
CREATE DEFINER=`root`@`localhost` PROCEDURE `{프로시저명}`(IN _INPUT INT)
BEGIN
    {실행하고자 하는 쿼리}
END$$
DELIMITER ;
```

## mysql에서 프로시저 실행

CALL 프로시저명({입력값});