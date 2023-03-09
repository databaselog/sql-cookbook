# 3.5 Retrieving Rows from One Table That Do Not Correspond to Rows in Another

### 퀴즈

공통 키가 있지만 다른 테이블과 일치하지 않는 값을 찾을려고 합니다. 

예를 들어 사원이 없는 부서를 찾고 싶은 경우, 어떻게 해야할까요?

~~~sql
        DEPTNO  DNAME           LOC
    ----------  --------------  -------------
            40  OPERATIONS      BOSTON
~~~

<br>

### LEFT OUTER JOIN

아래와 같이 `left outer join`을 활용하여 사원이 존재하지 않는 부서만 찾을 수 있습니다.

~~~sql
select d.*
from dept d left outer join emp e
on (d.deptno = e.deptno)
where e.deptno is null
~~~

이렇게 매칭되지 않는 row를 찾으려 `left outer join` 을 활용하는 것을 `anti-join`이라고 합니다.

