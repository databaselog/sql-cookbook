## 8-5 Determining the Number of Seconds, Minutes, or Hours Between Two Dates
### Problem
두 날짜간 차이를 초 단위로 반환 하려고한다.
- 앨런과 애드워드의 HIREDATE의 차이를 시,분,초 단위로 반환하려고 한다.

<br>

### Solution
- 날짜 사이의 일 수를 구하면 시, 분, 초를 찾을 수 있다.
#### DB2

- DAYS 함수를 사용하여 ALLEN_HD와 WARD_HD의 차이를 찾고 시간 단위로 곱하여 찾는다.

~~~sql
select dy*24 hr, dy*24*60 min, dy*24*60*60 sec
  from (
select ( days(max(case when ename = 'WARD'
                  then hiredate
              end)) -
         days(max(case when ename = 'ALLEN'
                  then hiredate
             end)) 
        ) as dy
  from emp
       ) x
~~~



#### MySQL

- DATEDIFF 함수를 사용하여 ALLEN_HD와 WARD_HD 사이의 일 수를 반환하고 시간 단위로 곱하여 찾는다.

~~~sql
select datediff(day,allen_hd,ward_hd)*24 hr,
       datediff(day,allen_hd,ward_hd)*24*60 min,
       datediff(day,allen_hd,ward_hd)*24*60*60 sec
  from (
select max(case when ename = 'WARD'
                   then hiredate
           end) as ward_hd,
       max(case when ename = 'ALLEN'
                   then hiredate
           end) as allen_hd
  from emp
        ) x
~~~



#### Oracle, PostgreSQL

- 뺄셈을 사용하여 ALLEN_HD와 WARD_HD 사이의 일 수를 반환하고 시간 단위로 곱하여 찾는다.

~~~sql
select dy*24 as hr, dy*24*60 as min, dy*24*60*60 as sec
  from (
select (max(case when ename = 'WARD'
              then hiredate
            end) -
        max(case when ename = 'ALLEN'
              then hiredate
            end)) as dy
  from emp
       ) x
~~~

<br>

### Discription

- 아래 인라인뷰는 워드와 앨런의 HIREDATE를 반환

~~~sql
select max(case when ename = 'WARD'
                 then hiredate
           end) as ward_hd,
       max(case when ename = 'ALLEN'
                then hiredate
           end) as allen_hd
   from emp

    WARD_HD     ALLEN_HD
    ----------- -----------
    22-FEB-2006 20-FEB-2006
~~~
