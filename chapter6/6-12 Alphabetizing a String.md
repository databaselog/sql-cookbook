# 6-12 Alphabetizing a String

## 문제

~~~sql
 with t10 as
 (select ename from emp)
 
    ENAME
    ----------
    ADAMS
    ALLEN
    BLAKE
    CLARK
    FORD
    JAMES
    JONES
    KING
    MARTIN
    MILLER
    SCOTT
    SMITH
    TURNER
    WARD
    
    
   OLD_NAME   NEW_NAME
    ---------- --------
    ADAMS      AADMS
    ALLEN      AELLN
    BLAKE      ABEKL
    CLARK      ACKLR
    FORD       DFOR
    JAMES      AEJMS
    JONES      EJNOS
    KING       GIKN
    MARTIN     AIMNRT
    MILLER     EILLMR
    SCOTT      COSTT
    SMITH      HIMST
    TURNER     ENRRTU
    WARD       ADRW
~~~



## MySQL

### Step.1

- iter.pos

~~~sql
with t10 as
(
select '1' as id, 'KING' as name from dual union all
select '2' as id, 'KING' as name from dual union all
select '3' as id, 'KING' as name from dual union all
select '4' as id, 'KING' as name from dual union all
select '5' as id, 'KING' as name from dual union all
select '6' as id, 'KING' as name from dual
)
select ename, substr(a.ename,iter.pos,1) c
from emp a,( select id as pos from t10 ) iter
where iter.pos <= length(a.ename);
~~~

**OUTPUT**

~~~sql
+--------+------+
| ename  | c    |
+--------+------+
| SMITH  | H    |
| SMITH  | T    |
| SMITH  | I    |
| SMITH  | M    |
| SMITH  | S    |
| ALLEN  | N    |
| ALLEN  | E    |
| ALLEN  | L    |
| ALLEN  | L    |
| ALLEN  | A    |
| WARD   | D    |
| WARD   | R    |
| WARD   | A    |
| WARD   | W    |
~~~

### Step.2

- group_concat

~~~sql
with t10 as
(
select '1' as id, 'KING' as name from dual union all
select '2' as id, 'KING' as name from dual union all
select '3' as id, 'KING' as name from dual union all
select '4' as id, 'KING' as name from dual union all
select '5' as id, 'KING' as name from dual union all
select '6' as id, 'KING' as name from dual
)
select ename, group_concat(c order by c separator '')
from (
  select ename, substr(a.ename,iter.pos,1) c
  from emp a,( select id as pos from t10 ) iter
  where iter.pos <= length(a.ename)
 ) x
 group by ename;
~~~

<br>
