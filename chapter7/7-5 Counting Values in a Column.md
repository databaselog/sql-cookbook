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

<br>

### Discription

COUNT(*)에서와 같이 '별'을 카운트할 때 실제 카운트하는 것은 행입니다 (실제 값과 관계없음). 따라서 NULL 및 NULL이 아닌 값이 포함된 행이 카운트됩니다. 그러나 컬럼을 셀 때는 해당 열의 NON-NULL의 수를 계산합니다. 위 솔루션의 COUNT(COMM)는 COMM 열의 NON-NULL의 수를 반환합니다. 
