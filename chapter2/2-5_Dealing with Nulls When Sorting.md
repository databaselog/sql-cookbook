## 2.5 Dealing with Nulls When Sorting

### 문제

EMP 테이블에서 null 값이 포함된 `COMM` 컬럼을 기준으로 결과를 정렬하려고합니다.

null 값이 포함되면 정렬의 기준이 다양해집니다:

1. null 값과 non-null 값을 오름차순으로 정렬할지 
2. null 값과 non-null 값을 내림차순으로 정렬할지 
3. null 값을 제외하고 나머지를 내림차순으로 정렬할지
4. null 값을 제외하고 나머지를 오름차순으로 정렬할지

등등.. 수많은 케이스들을 생각하며 쿼리를 작성해야합니다.

DBMS마다 `NULL`을 정렬하는 방법이 다르고, 데이터가 어떻게 보여질지는 **사용자가 정의하는것이기 때문에** 각 케이스 별로 쿼리를 어떻게 작성 해야할지 알아봅시다.

&nbsp;

### Flag 활용

> DB2, MySQL, PostgreSQL, SQL Server

`CASE`문을 활용하여 `NULL`값에 ***Flag*를 설정합니다**. 

*Flag* 를 설정한다는것은:

- 값이 `NULL`일 경우 숫자 `0`을 임시로 치환하고 
- 값이 `NON-NULL`일 경우 `1`을 사용합니다.

~~~sql
select ename,sal,comm,
    case when comm is null then 0 else 1 end as is_null
  from emp;

ENAME    SAL       COMM     IS_NULL
------ ----- ---------- ----------
SMITH    800                     0
ALLEN   1600       300           1
WARD    1250       500           1
JONES   2975                     0
MARTIN  1250      1400           1
BLAKE   2850                     0
CLARK   2450                     0
SCOTT   3000                     0
KING    5000                     0
TURNER  1500         0           1
ADAMS   1100                     0
JAMES    950                     0
FORD    3000                     0
MILLER  1300                     0
~~~

`is_null` 컬럼으로 인해 `NULL`을 먼저 정렬할건지, 숫자를 정렬할건지 순서를 보다 쉽게 정할 수 있습니다.

&nbsp;

#### Case 1. Non-Null 값 오름차순 정렬 + NULLS LAST

~~~sql
select ename,sal,comm
  from (
    select ename,sal,comm,
      case when comm is null then 0 else 1 end as is_null
      from emp
  ) x
  order by is_null desc,comm
  
    ENAME              SAL        COMM
    ----------  ----------  ----------
    TURNER            1500           0
    ALLEN             1600         300
    WARD              1250         500
    MARTIN            1250        1400
    SMITH              800
    JONES             2975
    JAMES              950
    MILLER            1300
    FORD              3000
    ADAMS             1100
    BLAKE             2850
    CLARK             2450
    SCOTT             3000
    KING              5000
~~~



#### Case 2. Non-Null 값 내림차순 정렬 + NULLS LAST

~~~sql
select ename,sal,comm
  from (
    select ename,sal,comm,
      case when comm is null then 0 else 1 end as is_null
      from emp
  ) x
  order by is_null desc,comm desc


    ENAME     SAL        COMM
    ------  -----  ----------
    MARTIN   1250        1400
    WARD     1250         500
    ALLEN    1600         300
    TURNER   1500           0
    SMITH     800
    JONES    2975
    JAMES     950
    MILLER   1300
    FORD     3000
    ADAMS    1100
    BLAKE    2850
    CLARK    2450
    SCOTT    3000
    KING     5000
~~~

#### Case 3. Non-Null 값 오름차순 정렬 + NULLS FIRST

~~~sql
select ename,sal,comm
  from (
    select ename,sal,comm,
      case when comm is null then 0 else 1 end as is_null
      from emp
  ) x
 order by is_null,comm
 
    ENAME              SAL        COMM
    ----------  ----------  ----------
    TURNER            1500           0
    ALLEN             1600         300
    WARD              1250         500
    MARTIN            1250        1400
    SMITH              800
    JONES             2975
    JAMES              950
    MILLER            1300
    FORD              3000
    ADAMS             1100
    BLAKE             2850
    CLARK             2450
    SCOTT             3000
    KING              5000
~~~

#### Case 4. Non-Null 값 내림차순 정렬 + NULLS FIRST

~~~sql
select ename,sal,comm
  from (
    select ename,sal,comm,
      case when comm is null then 0 else 1 end as is_null
      from emp
   )x
 order by is_null,comm desc   
   
   
    ENAME              SAL        COMM
    ----------  ----------  ----------
    SMITH              800
    JONES             2975
    CLARK             2450
    BLAKE             2850
    SCOTT             3000
    KING              5000
    JAMES              950
    MILLER            1300
    FORD              3000
    ADAMS             1100
    MARTIN            1250      1400
    WARD              1250       500
    ALLEN             1600       300
    TURNER            1500         0
~~~

&nbsp;

### Oracle - Nulls Last

Oracle에서만 사용할 수 있는 `NULLS LAST` 구문을 통해 문제를 더 쉽게 해결 할 수 있습니다. 해당 구문은 `NON-NULL` 값들과 별개로 정렬이 되기 때문에 추가의 컬럼 생성은 불필요 합니다.

~~~sql
select ename,sal,comm
  from emp
  order by comm nulls last;
  
    ENAME    SAL       COMM
    ------  ----- ---------
    TURNER   1500         0
    ALLEN    1600       300
    WARD     1250       500
    MARTIN   1250      1400
    SMITH     800
    JONES    2975
    JAMES     950
    MILLER   1300
    FORD     3000
    ADAMS    1100
    BLAKE    2850
    CLARK    2450
    SCOTT    3000
    KING     5000
~~~

&nbsp;