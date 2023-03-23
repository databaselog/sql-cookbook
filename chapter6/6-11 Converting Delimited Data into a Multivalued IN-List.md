# 6-11 Converting Delimited Data into a Multivalued IN-List

## 문제

~~~sql
-- EMPNO는 숫자이지만 IN 안에 값들이 문자열이여서 SQL 수행이 불가함
    select ename,sal,deptno
      from emp
     where empno in ( '7654,7698,7782,7788' )
~~~



## MySQL

### Step.1

- substring_index

~~~sql
select substring_index(
     substring_index(list.vals,',',iter.pos),',',-1) empno
from (select id.pos from t10) as iter,
     (select '7654,7698,7782,7788' as vals from t1) list
where iter.pos <=
     (length(list.vals)-length(replace(list.vals,',','')))+1
~~~

**OUTPUT**

~~~sql
DEPT,ENAME,  RN,CNT
30	Raphaely	1	5
30	Khoo	    2	5
30	Baida	    3	5
30	Tobias	  4	5
30	Himuro	  5	5
60	Hunold	  1	5
60	Ernst	    2	5
60	Austin	  3	5
60	Pataballa	4	5
60	Lorentz	  5	5
90	King	    1	3
90	Kochhar	  2	3
90	De Haan	  3	3
100	Greenberg	1	6
100	Faviet	  2	6
100	Chen	    3	6
100	Sciarra	  4	6
100	Urman	    5	6
100	Popp	    6	6
~~~

### Step.2

~~~sql
select empno, ename, sal, deptno
from emp
where empno in (
  select substring_index(
       substring_index(list.vals,',',iter.pos),',',-1) empno
  from (select id pos from t10) as iter,
       (select '7654,7698,7782,7788' as vals from t1) list
  where iter.pos <=
       (length(list.vals)-length(replace(list.vals,',','')))+1
)
~~~

<br>
