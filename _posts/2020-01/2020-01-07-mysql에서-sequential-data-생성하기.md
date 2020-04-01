---
title:  "mysql에서 테이블 없이 연속 데이터 생성하기"
date:   2020-01-07 21:00:00 +0900
tags: [mysql]
key: 20200107_01
---

## recursive 를 사용하여 sequential data 생성

mysql에서 연속적인 데이터를 생성하는 방법이 있을 것 같아 검색을 해보니   
모두 임시 테이블을 만들어서 가지고 오는 방법으로 안내했다.  
[recursive](https://naheenosaur.github.io/mysql%EC%97%90%EC%84%9C-%EA%B3%84%EC%B8%B5%ED%98%95-%EA%B5%AC%EC%A1%B0-%EC%9E%AC%EA%B7%80%EC%8B%9D-%EC%B6%9C%EB%A0%A5)를 사용하는 방법으로 생성할 수 있을 것 같아서 생성해 봤다.
 
```sql
WITH RECURSIVE num_range AS ( 
	SELECT 1 AS _num
		UNION ALL 	
	SELECT _before._num + 1 AS _num
	FROM num_range AS _before
	WHERE _before._num < 10
) 
SELECT _num FROM num_range; 
```

응용해서 연속된 날짜의 범위도 간단하게 출력할 수 있다.
```sql
SET @today = NOW();
WITH RECURSIVE date_range AS ( 
	SELECT @today AS _date
		UNION ALL 	
	SELECT DATE_ADD(_before._date, INTERVAL 1 DAY) AS _date
	FROM date_range AS _before
	WHERE DATEDIFF(_before._date, @today) < 10
) 
SELECT _date FROM date_range; 
```
