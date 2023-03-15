# 4.8 Modifying Records in a Table

### 퀴즈

테이블에 하나 이상의 행의 값을 수정하려고 합니다. 

예를 들어 부서 아이디 20에 있는 모든 직원의 급여를 10% 인상하려고 합니다.

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

### UPDATE

아래와 같이 `UPDATE` 문을 사용하여 데이터베이스 테이블의 기존 행을 수정합니다.

~~~sql
1 update emp
2 set sal = sal*1.10
3 where deptno = 20
~~~

#### 주의사항

업데이트할 행을 지정하려면 `WHERE` 절과 함께 UPDATE 문을 사용해야합니다. 

`WHERE` 절을 제외하면 모든 행이 업데이트됩니다. 아래의 `SAL`*`1.10`  수식은 10% 인상된 급여를 SELECT로 반환합니다.

~~~sql
select deptno,
           ename,
           sal      as orig_sal,
           sal*.10  as amt_to_add,
           sal*1.10 as new_sal
      from emp
     where deptno=20
     order by 1,5
     
DEPTNO ENAME  ORIG_SAL AMT_TO_ADD  NEW_SAL
------ ------ -------- ----------  -------
20     SMITH       800         80      880
20     ADMAS      1100        110     1210
20     JONES      2975        298     3273
20     SCOTT      3000        300     3300
20      FORD      3000        300     3300
~~~

대량 업데이트를 수행해야할때 위 쿼리로 결과를 미리 볼 수 있습니다.
