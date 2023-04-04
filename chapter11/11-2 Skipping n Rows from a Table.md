## 11-2 Skipping n Rows from a Table
### Problem
테이블 EMP의 다른 모든 직원을 반환 하려한다.
- 프로젝트의 시작 및 종료 날짜가 포함된 View, V에서 연속된 프로젝트만 조회한다.

~~~sql
    select sal
      from (
    select row_number() over (order by sal) as rn,
           sal
from emp )x
     where rn between 1 and 5
     SAL
    ----
     800
     950
    1100
    1250
    1250
    
    select sal
      from (
    select row_number() over (order by sal) as rn,
           sal
from emp )x
     where rn between 6 and 10
     
      SAL
    -----
     1300
     1500
     1600
     2450
     2850
~~~

<br>

### Solution

- 두 번째 또는 네 번째 또는 n번째 행을 건너뛰려면 결과 집합에 순서를 지정한후 ROW_NUMBER() OVER() 사용
- 각 행에 번호를 할당하면 ROW_NUMBER OVER함수와 함께 행을 건너뛸 수 있다.

```sql
select row_number() over (order by sal) as rn,
           sal
from emp


RN         SAL
--  ----------
1 800
2 950
3 1100
4 1250
5 1250
6 1300
7 1500
8 1600
9 2450
10 2850
11 2975
12 3000
13 3000
14 5000


    select sal
      from (
    select sal, rownum rn
      from (
    select sal
      from emp
order by sal )
           )
     where rn between 6 and 10
      SAL
    -----
     1300
     1500
     1600
     2450
     2850
```

<br>

### Description

인라인 뷰 X에서 WINDOW 함수 ROW_NUMBER OVER를 호출하면 각 행에 RANK가 할당된다(동일한 이름이 있더라도 관계 없음)

~~~sql
    select row_number() over (order by ename) rn, ename
      from emp
   
    RN ENAME
    -- --------
     1 ADAMS
     2 ALLEN
     3 BLAKE
     4 CLARK
     5 FORD
     6 JAMES
     7 JONES
     8 KING
     9 MARTIN
    10 MILLER
    11 SCOTT
    12 SMITH
    13 TURNER
    14 WARD
~~~
