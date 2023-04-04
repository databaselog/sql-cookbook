## 10-5 Generating Consecutive Numeric Values
### Problem
피봇에 유용한 row source generator를 쿼리에서 사용하려고 한다.
~~~sql
ID 
---
1 
2 
3 
4 
5 
...
~~~

RDBMS에서 기본으로 해당 기능을 제공하는 경우, 고정된 행의 수를 사용하여 미리 피벗 테이블을 작성할 필요가 없다. 

하지만 행을 생성하기 위해 고정된 행 수(항상 충분하지 않을 수 있)가 있는 기존 피벗 테이블을 사용해야 한다면. RSG가 매우 편리해진다.

<br>

### Solution
- 날짜에 추가할 숫자를 생성하여 날짜 시퀀스를 생성

#### DB2 and SQL Server

```sql
1 with x (id)
     2 as (
     3 select 1
     4  union all
     5 select id+1
     6   from x
     7  where id+1 <= 10
8)
9 select * from x
```

#### Oracle

- MODEL() 구문사용

~~~sql
    1 select array id
    2   from dual
    3  model
    4    dimension by (0 idx)
    5    measures(1 array)
    6    rules iterate (10) (
7 8)
~~~

#### PostgreSQL

- GENERATE_SERIES 사용

~~~sql
    1 select id
    2   from generate_series (1, 10) x(id)
~~~

<br>

### Description

#### Oracle

- Iterate 절을 사용하지 않으면 DUAL의 행이 하나이므로 하나의 행만 반환

~~~sql
   select array id
      from dual
    model
      dimension by (0 idx)
      measures(1 array)
      rules ()
ID 
-- 1
~~~

- MODEL 절을 사용하면 배열에 접근하기 용이할 뿐만 아니라 테이블에 없는 행을 쉽게 "생성"하거나 반환할 수 있다.
- Oracle에선 REPATION_NUMBER 함수를 제공하기에 아래 예시처럼 ITERATION_NUMBER가 0에서 9로 출력됩니다.

~~~sql
    select 'array['||idx||'] = '||array as output
      from dual
     model
       dimension by (0 idx)
       measures(1 array)
       rules iterate (10) (
         array[iteration_number] = iteration_number+1
       )
       
    OUTPUT
    ------------------
    array[0] = 1
    array[1] = 2
    array[2] = 3
    array[3] = 4
    array[4] = 5
    array[5] = 6
    array[6] = 7
    array[7] = 8
    array[8] = 9
    array[9] = 10
~~~



#### PostgreSQL

- GENERATE_SERIES 사용
- 세개의 파라미터 사용 - 시작값, 마지막값, 스텝값(increment)
- 해당 함수는 flexible하므로 increment값을 하드코딩하지 않아도 충분히 사용 가능

~~~sql
ID
    ---
     10
     15
     20
     25
     30
     
select id
      from generate_series(
             (select min(deptno) from emp),
             (select max(deptno) from emp),
             5
) x(id)
~~~



