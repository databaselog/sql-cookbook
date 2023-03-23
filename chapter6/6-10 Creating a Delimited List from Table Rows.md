# 6-10 Creating a Delimited List from Table Rows

## 문제

~~~sql
 select deptno, ename from emp order by deptno;
 
   DEPTNO EMPS
    ------ ----------
        10 CLARK
        10 KING
        10 MILLER
        20 SMITH
        20 ADAMS
        20 FORD
        20 SCOTT
        20 JONES
        30 ALLEN
        30 BLAKE
        30 MARTIN
        30 JAMES
        30 TURNER
        30 WARD
        
        
        
   DEPTNO EMPS
    ------- ------------------------------------
         10 CLARK,KING,MILLER
~~~



## Oracle

### Step.1

- Row_number() 를 사용한다.
- Count_over()를 사용하여 dept별 사람을 센다.

~~~sql
SELECT DEPARTMENT_ID, LAST_NAME,
       ROW_NUMBER() OVER (partition by DEPARTMENT_ID order by EMPLOYEE_ID) rn,
       COUNT(*) OVER (partition by DEPARTMENT_ID) cnt
FROM HR.employees c
WHERE ROWNUM < 20;
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
SELECT DEPARTMENT_ID, ltrim(SYS_CONNECT_BY_PATH(LAST_NAME, ','), ',') emps
FROM
(
	SELECT e.DEPARTMENT_ID, e.LAST_NAME,
	       ROW_NUMBER() OVER (partition by e.DEPARTMENT_ID order by e.EMPLOYEE_ID) rn,
	       count(*) over (partition by e.DEPARTMENT_ID) cnt
	FROM HR.EMPLOYEES e
	WHERE ROWNUM < 20
)
where level = cnt
start with rn = 1
connect by prior DEPARTMENT_ID = DEPARTMENT_ID and prior rn = rn-1
;
~~~



<br>

## MySQL (치트키)

- GROUP_CONCAT

~~~sql
1 select deptno,
2 group_concat(ename order by empno separator, ',') as emps
3 from emp
4 group by deptno
~~~

