## 7.4 Counting Rows in a Table
### Problem
테이블의 행 갯수를 계산하고자 한다.
- 총 직원수와 각 부서별 직원 수를  계산하고자 한다.

<br>

### Solution
- COUNT 함수와 Asterisk(*)를 사용한다.
```sql
select count(*)
from emp

      COUNT(*)
    ----------
            14
```

- 여러 부서의 직원수를 구할때는 GROUP BY & COUNT 함께 사용
- 예) 부서별 직원 수

~~~sql
select deptno, count(*)
from emp
group by deptno

        DEPTNO    COUNT(*)
    ----------  ----------
            10           3
            20           5
            30           6
~~~

<br>