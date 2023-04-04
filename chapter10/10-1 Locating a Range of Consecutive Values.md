## 10-1 Locating a Range of Consecutive Values
### Problem
연속된 프로젝트의 범위를 나타내려고 한다.
- 프로젝트의 시작 및 종료 날짜가 포함된 View, V에서 연속된 프로젝트만 조회한다.

~~~sql
    PROJ_ID PROJ_START  PROJ_END
    ------- ----------- -----------
          1 01-JAN-2020 02-JAN-2020
          2 02-JAN-2020 03-JAN-2020
          3 03-JAN-2020 04-JAN-2020
          4 04-JAN-2020 05-JAN-2020
          5 06-JAN-2020 07-JAN-2020
          6 16-JAN-2020 17-JAN-2020
          7 17-JAN-2020 18-JAN-2020
          8 18-JAN-2020 19-JAN-2020
          9 19-JAN-2020 20-JAN-2020
         10 21-JAN-2020 22-JAN-2020
         11 26-JAN-2020 27-JAN-2020
         12 27-JAN-2020 28-JAN-2020
         13 28-JAN-2020 29-JAN-2020
         14 29-JAN-2020 30-JAN-2020
         
         
    PROJ_ID PROJ_START  PROJ_END
    ------- ----------- -----------
         1  01-JAN-2020 02-JAN-2020
         2  02-JAN-2020 03-JAN-2020
         3  03-JAN-2020 04-JAN-2020
         6  16-JAN-2020 17-JAN-2020
         7  17-JAN-2020 18-JAN-2020
         8  18-JAN-2020 19-JAN-2020
        11  26-JAN-2020 27-JAN-2020
        12  27-JAN-2020 28-JAN-2020
        13  28-JAN-2020 29-JAN-2020
~~~

<br>

### Solution
- 윈도우 함수인 LEAD OVER 사용
- 현재 행 기준 **이전 행** 값을 가져오는 함수
```sql
    1 select proj_id, proj_start, proj_end
    2   from (
    3 select proj_id, proj_start, proj_end,
    4        lead(proj_start)over(order by proj_id) next_proj_start
    5   from V
    6        ) alias
    7 where next_proj_start = proj_end
```

##### How to use LEAD OVER()

~~~sql
LEAD(<expr>[,offset[,default_value]]) 
OVER ([PARTITION BY <expr>] ORDER BY <expr>)
~~~

- [..] 부분은 생략 가능
- offset : 지정시 N번째 값을 가져온다.
  \- default_value : N번째 값이 없을 경우 default_value값을 가져온다.
- PARTITION BY : 지정시 GROUP 별로 행 값을 가져온다.

![img](https://blog.kakaocdn.net/dn/bwIdvK/btqHA4munoN/7sZ8yK6Skv7w7E68vS7XeK/img.png)

~~~sql
SELECT 
    customerName,
    orderDate,
    LEAD(orderDate,1) OVER (
        PARTITION BY customerNumber
        ORDER BY orderDate ) nextOrderDate
FROM 
    orders
INNER JOIN customers USING (customerNumber);
~~~

![img](https://blog.kakaocdn.net/dn/cdOmCn/btqHCWV18aj/HUbZ6pxiuEL6KrCilK3d1k/img.png)



<br>

### Description

#### DB2, MySQL, PostgreSQL, SQL Server, and Oracle

- 윈도우 함수는 FROM과 WHERE절 이후에 수행됨
- 따라서 아래 쿼리는 inline view를 통해 내부 쿼리가 먼저 실행되어야 하므로 정확한 값 측정가능

~~~sql
    select *
      from (
           select proj_id, proj_start, proj_end,
           lead(proj_start)over(order by proj_id) next_proj_start
           from v )
     where proj_id in ( 1, 4 )
     
    PROJ_ID PROJ_START  PROJ_END    NEXT_PROJ_START
    ------- ----------- ----------- ---------------
          1 01-JAN-2020 02-JAN-2020 02-JAN-2020
          4 04-JAN-2020 05-JAN-2020 06-JAN-2020
~~~

