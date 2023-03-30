## 7.2 Finding the Min/Max Value in a Column
### Problem
컬럼의 최대값 / 최소값을 계산하고자 한다.
- 사원중 제일 높은 급여와 가장 낮은 급여를 찾고자 한다.

<br>

### Solution
- MIN / MAX 함수를 사용한다.
```sql
select min(sal) as min_sal, max(sal) as max_sal
from emp
```

- 부서별 최저 / 최고 연봉을 찾을 때 GROUP BY 절과 함께 MIN 및 MAX 함수를 사용

~~~sql
select deptno, min(sal) as min_sal, max(sal) as max_sal
from emp
group by deptno

DEPTNO     MIN_SAL     MAX_SAL
----------  ----------  ----------
10                1300	5000
20                800		3000
30                 950	2850
~~~

<br>

### Discription

1. 전체 테이블이 그룹인 경우 group by 절을 쓰지 않고 `min`, `max` 함수를 적용 
1. MIN 및 MAX 함수는 NULL을 무시한다.

~~~sql
select deptno, comm
      from emp
     where deptno in (10,30)
     order by 1
     
         DEPTNO       COMM
     ---------- ----------
             10
             10
             10
             30        300
             30        500
             30
             30          0 
             30       1300 
             30
~~~