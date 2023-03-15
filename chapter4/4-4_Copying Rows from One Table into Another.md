# 4.4 Copying Rows from One Table into Another

### 퀴즈

한 테이블에서 다른 테이블로 행을 복사하려고 합니다. 

예를 들어, DEPT 테이블에서 DEPT_EAST 테이블로 행을 복사하려고 합니다. DEPT_EAST 테이블은 이미 DEPT와 동일한 구조(동일한 열 및 데이터 유형)로 생성되었으며 현재 비어 있습니다.

~~~sql
    DEPTNO ENAME             SAL
    ------ ---------- ----------
        20 SMITH             800
        20 ADAMS            1100
        20 JONES            2975
        20 SCOTT            3000
        20 FORD             3000
~~~

<br>

### INSERT

아래와 같이 `INSERT` 문을 사용하여 dept_east로 기존 행을 삽입합니다.

~~~sql
1 insert into dept_east (deptno,dname,loc)
2 select deptno,dname,loc
3   from dept
4  where loc in ( 'NEW YORK','BOSTON' )
~~~

#### TIP

**소스 테이블에서 모든 행을 복사하려면 쿼리에서 WHERE 절을 제외하면됩니다**. 대상 컬럼을 지정하지 않으면 모든 컬럼에 데이터를 삽입해야 하며, 4.1에서 언급된 `SELECT` 순서에 유의해야 합니다.
