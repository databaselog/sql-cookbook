## 7.6 Generating a Running Total
### Problem
컬럼안에 있는 값의 누계를 계산하고자 한다.
- 모든 직원의 급여 누계를 계산하고자 한다.

<br>

### Solution
- SAL컬럼 기준으로 결과를 정렬
```sql
select ename, sal,
sum(sal) over (order by sal,empno) as running_total
from emp
order by 2
```

- 

~~~sql

~~~

<br>

### Discription

SUM OVER 작동 방식

1. 

