# 6-13 Identifying Strings That Can Be Treated as Numbers

## 문제

~~~sql
    create view V as
    select replace(mixed,' ','') as mixed
      from (
    select substr(ename,1,2)||
           cast(deptno as char(4))||
           substr(ename,3,2) as mixed
      from emp
     where deptno = 10
     union all
    select cast(empno as char(4)) as mixed
      from emp
     where deptno = 20
     union all
    select ename as mixed
      from emp
where deptno = 30 )x
    select * from v
    
    
    
     MIXED
--------------
     CL10AR
     KI10NG
     MI10LL
     7369
     7566
     7788
     7876
     7902
     ALLEN
     WARD
     MARTIN
     BLAKE
     TURNER
     JAMES
     
     
       MIXED
    --------
          10
          10
          10
        7369
        7566
        7788
        7876
        7902

~~~



## MySQL

### Step.1

- iter.pos

~~~sql
    create view V as
    select concat(
             substr(ename,1,2),
             replace(cast(deptno as char(4)),' ',''),
             substr(ename,3,2)
           ) as mixed
      from emp
     where deptno = 10
     union all
    select replace(cast(empno as char(4)), ' ', '')
      from emp where deptno = 20
     union all
    select ename from emp where deptno = 30
~~~

### Step.2

- separator

~~~sql
select cast(group_concat(c order by pos separator '') as unsigned) as MIXED1
from (
     select v.mixed, iter.pos, substr(v.mixed,iter.pos,1) as c
     from V, ( select id pos from t10 ) iter
     where iter.pos <= length(v.mixed)
     and ascii(substr(v.mixed,iter.pos,1)) between 48 and 57
    ) y
group by mixed
order by 1
~~~

<br>
