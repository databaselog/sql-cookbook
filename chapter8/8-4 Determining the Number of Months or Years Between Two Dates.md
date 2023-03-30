## 8-4 Determining the Number of Months or Years Between Two Dates
### Problem
월 또는 연도 측면에서 두 날짜의 차이를 찾고자한다.
- 처음으로 고용한 사원과 마지막으로 고용한 사원 사이의 개월 수 를 찾고 그값을 년 수로 표현하고자 한다.

<br>

### Solution
- 1년은 항상 12개월이므로 두 날짜 사이의 개월수를 찾은 다음 12로 나누어 연도를 구한다.
```sql
HIREDATE       LAST_HIREDATE    MONTH   YEAR
17-DEC-1980    12-JAN-1983      25      3
```



#### DB2 & MySQL

~~~sql
select mnth, mnth/12
  from (
select (year(max_hd) - year(min_hd))*12 +
       (month(max_hd) - month(min_hd)) as mnth
  from (
select min(hiredate) as min_hd, max(hiredate) as max_hd
   from emp
       )x
       )y
~~~



#### Oracle

~~~sql
select months_between(max_hd,min_hd),
       months_between(max_hd,min_hd)/12
  from (
select min(hiredate) min_hd, max(hiredate) max_hd
  from emp
       ) x
~~~

#### PostgreSQL

~~~sql
select mnth, mnth/12
  from (
select ( extract(year from max_hd)
         extract(year from min_hd) ) * 12
       +
       ( extract(month from max_hd)
         extract(month from min_hd) ) as mnth
   from (
 select min(hiredate) as min_hd, max(hiredate) as max_hd
   from emp
        ) x
        ) y
~~~

#### SQL Server

~~~sql
select datediff(month,min_hd,max_hd),
       datediff(year,min_hd,max_hd)
  from (
select min(hiredate) min_hd, max(hiredate) max_hd
  from emp
       )x
~~~



<br>

### Discription

#### DB2, MySQL, PostgreSQL

- MIN_HD, MAX_HD의 연도와 월을 추출하면 MIN_HD와 MAX_HD 사이의 월과 연도를 찾을 수 있음

~~~sql
select min(hiredate) as min_hd,
       max(hiredate) as max_hd
from emp

  MIN_HD      MAX_HD
  ----------- -----------
  17-DEC-1980 12-JAN-1983
~~~

- 여기서 두 날짜 사이의 월을 찾으려면 두 사이의 연도차이에 12를 곱한 후 두 날짜 사이의 월차이를 더한다
- (1983 - 1980) * 12 + (1 - 12)

~~~sql
select year(max_hd) as max_yr, year(min_hd) as min_yr,
           month(max_hd) as max_mon, month(min_hd) as min_mon
    from (
select min(hiredate) as min_hd, max(hiredate) as max_hd
    from emp 
         )x
~~~



#### Oracle, SQL Server

- 인라인뷰는 해당 테이블에서 가장 이른 날짜와 가장 최근의 HIREDATE를 반환
- DATEDIFF, MONTHS_BETWEEN 또한 사용가능

~~~sql
 select min(hiredate) as min_hd, max(hiredate) as max_hd
      from emp
      
    MIN_HD      MAX_HD
    ----------- -----------
    17-DEC-1980 12-JAN-1983
~~~
