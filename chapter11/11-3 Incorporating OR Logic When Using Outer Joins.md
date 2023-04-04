## 11-3 Incorporating OR Logic When Using Outer Joins
### Problem
부서 10과 20에 있는 모든 직원의 이름과 부서 정보를 부서 30과 40에 대한 부서 정보와 함께 반환하려고한다.

아래 예시는 이너조인으로 구성되어있으므로 DEPTNO 30 40인 부서정보가 포함되어있지 않다.

~~~sql
   select e.ename, d.deptno, d.dname, d.loc
      from dept d left join emp e
        on (d.deptno = e.deptno)
     where e.deptno = 10
        or e.deptno = 20
     order by 2


ENAME       DEPTNO DNAME          LOC
------- ---------- -------------- -----------
CLARK           10     ACCOUNTING    NEW YORK
KING            10     ACCOUNTING    NEW YORK
MILLER          10     ACCOUNTING    NEW YORK 
SMITH           20       RESEARCH      DALLAS
ADAMS           20       RESEARCH      DALLAS
FORD            20       RESEARCH      DALLAS
SCOTT           20       RESEARCH      DALLAS
JONES           20       RESEARCH      DALLAS


ENAME     DEPTNO      DNAME           LOC
------- ---------- ------------ ---------
CLARK    10          ACCOUNTING   NEW YORK   
KING     10          ACCOUNTING   NEW YORK   
MILLER   10          ACCOUNTING   NEW YORK   
SMITH    20          RESEARCH     DALLAS
JONES    20          RESEARCH     DALLAS
SCOTT    20          RESEARCH     DALLAS
ADAMS    20          RESEARCH     DALLAS
FORD     20          RESEARCH     DALLAS
         30          SALES        CHICAGO
         40          OPERATIONS   BOSTON  
~~~

<br>

### Solution
1. OR 조건을 JOIN 문 안에 넣는다.

```sql
select e.ename, d.deptno, d.dname, d.loc
 from dept d left join emp e
   on (d.deptno = e.deptno
      and (e.deptno=10 or e.deptno=20))
order by 2
```

2. EMP.DEPTNO를 인라인 뷰에서 필터 한 후 outer join을 수행한다.

~~~sql
select e.ename, d.deptno, d.dname, d.loc
from dept d
  left join
    (select ename, deptno
     from emp
     where deptno in ( 10, 20 )
    ) e on ( e.deptno = d.deptno )
order by 2
~~~

<br>
