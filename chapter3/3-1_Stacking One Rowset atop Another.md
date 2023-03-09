# 3.1 Stacking One Rowset atop Another

### 퀴즈

아래 '-----' 를 기준으로 동일한 데이터 유형이 있습니다. 공통 키가 없지만 두개의 데이터를 결합하려면 어떻게 해야할까요?

~~~sql
    ENAME_AND_DNAME      DEPTNO
    ---------------  ----------
    CLARK                    10
    KING                     10
    MILLER                   10
    ----------
    ACCOUNTING               10
    RESEARCH                 20
    SALES                    30
    OPERATIONS               40
~~~

<br>

### UNION ALL

아래와 같이 `union all`을 활용하여 공통 키 없이 테이블을 조인 할 수 있습니다.

~~~sql
select ename as ename_and_dname, deptno
  from emp
  where deptno = 10
  union all
select '----------', null
  from dual
  union all
select dname, deptno
  from dept;
~~~

#### 주의사항

`UNION ALL`을 사용하게 될 경우 중복 데이터를 조심해야 합니다. 중복된 데이터를 필터하고 싶은 경우, `UNION`을 사용하셔야합니다.

~~~sql
select deptno
  from emp
  union
select deptno
  from dept
  
       DEPTNO
    ---------
           10
           20
           30
           40
~~~

아래와 같이 `Distinct`로 중복을 제거하는것도 좋은 방법입니다:

~~~sql
select distinct deptno
  from (
    select deptno
    from emp
      union all
    select deptno
    from dept 
  )

       DEPTNO
    ---------
           10
           20
           30
           40
~~~

이처럼, `DISTINCT`는 꼭 중복을 제거할때만 사용하는것처럼 `UNION`도 같은 원리입니다. 정말 필요한 순간이 아니면 `UNION ALL ` 대신 사용하지 마십시오.
