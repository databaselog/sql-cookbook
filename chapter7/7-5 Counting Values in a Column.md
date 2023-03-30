## 7.5 Counting Values in a Column
### Problem
컬럼에서 NULL이 아닌 값을 찾고자 한다.
- 사원중 커미션을 받고있는 직원수를 찾고자 한다.

<br>

### Solution
- EMP 테이블의 COMM 컬럼에서 NULL이 아닌 값 row 갯수를 계산합니다.
```sql
    select count(comm)
      from emp
      
    COUNT(COMM)
    -----------
              4
```

- 

~~~sql

~~~

<br>

### Discription

1. 

