# 3.9 Performing Joins When Using Aggregates

### 퀴즈

집계를 수행하려는 쿼리에 여러 테이블이 관련되어 있고, 조인 수행 시 집계가 방해되지 않도록 예방하려 합니다.

예를 들어 부서 10번 사원의 1) **급여 합계**와  2) **보너스 합계**  를 구하려고합니다. 

아래 `emp_bonus`라는 테이블이 주어졌을때, 타입에 따라 보너스 퍼센트가 달라집니다.

이에 따라 총 급여 **합계를 구해봅시다**.

~~~sql
    EMPNO RECEIVED          TYPE
    ----- ----------- ----------
     7934 17-MAR-2005          1
     7934 15-FEB-2005          2
     7839 15-FEB-2005          3
     7782 15-FEB-2005          1
~~~

<br>

### CASE WHEN

아래와 같이 `CASE`문을 활용하여 보너스 타입마다 퍼센트를 명시하여 보너스를 구할 수 있습니다.

~~~sql
select deptno,
       sum(sal) as total_sal,
       sum(bonus) as total_bonus
    from (
    select e.empno,
           e.ename,
           e.sal,
           e.deptno,
           e.sal*case when eb.type = 1 then .1
                      when eb.type = 2 then .2
                              else .3
                      end as bonus
      from emp e, emp_bonus eb
     where e.empno  = eb.empno
     and e.deptno = 10 )x
     group by deptno
     

    DEPTNO   TOTAL_SAL  TOTAL_BONUS
    ------ -----------  -----------
        10       10050         2135
~~~

하지만 어쩐지 TOTAL_SAL의 합이 안맞는거같죠?

~~~sql
 select sum(sal) from emp where deptno=10;
      
      SUM(SAL)
    ----------
          8750
~~~

위처럼 TOTAL_SAL이 안맞는 이유는 위 쿼리는 `MILLER` 사원의 급여가 중복되는걸 예방하지 못했기 때문입니다.

~~~sql
    select e.ename,
           e.sal
      from emp e, emp_bonus eb
     where e.empno  = eb.empno
       and e.deptno = 10
       
    ENAME             SAL
    ---------- ----------
    CLARK            2450
    KING             5000
    MILLER           1300
    MILLER           1300
~~~

<br>

### DISTINCT

이처럼 어떤 합을 집계 할 때는 특히나 값이 중복되지 않게 신경 그리고 또 신경을 써야합니다. 물론 데이터 검증은 필수이구요!

아래의 예시처럼 `DISTINCT` 함수로 급여가 중복되는것을 방지 할 수 있습니다.

~~~sql
select deptno,
        sum(distinct sal) as total_sal,
        sum(bonus) as total_bonus
   from (
     select e.empno,
     e.ename,
     e.sal,
     e.deptno,
     e.sal * case when eb.type = 1 then .1
             when eb.type = 2 then .2
             else .3
             end as bonus
     from emp e, emp_bonus eb
     where e.empno = eb.empno
     and e.deptno = 10
        ) x
  group by deptno;
~~~



### WINDOW FUNCTION (ORACLE)

오라클의 윈도우 함수중 하나인 `SUM OVER()`를 활용하여 집계를 할 수도 있습니다.

~~~sql
select e.empno,
       e.ename,
       sum(distinct e.sal) over
       (partition by e.deptno) as total_sal,
       e.deptno,
       sum(e.sal*case when eb.type = 1 then .1
                      when eb.type = 2 then .2
                      else .3 end) over (partition by deptno) as total_bonus
from emp e, emp_bonus eb
where e.empno  = eb.empno
and e.deptno = 10

EMPNO ENAME        TOTAL_SAL   DEPTNO  TOTAL_BONUS
----- ----------  ----------   ------  -----------
7934  MILLER            8750       10         2135
7934  MILLER            8750       10         2135
7782  CLARK             8750       10         2135
7839  KING              8750       10         2135
~~~

