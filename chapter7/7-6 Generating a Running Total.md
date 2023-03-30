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

ENAME              SAL  RUNNING_TOTAL
----------  ----------  -------------
    SMITH          800            800
    JAMES          950           1750
    ADAMS         1100           2850
    WARD          1250           4100 
    MARTIN        1250           5350 
    MILLER        1300           6650 
    TURNER        1500           8150 
    ALLEN         1600           9750
    CLARK         2450          12200
    BLAKE         2850          15050
    JONES         2975          18025
    SCOTT         3000          21025
    FORD          3000          24025
    KING          5000          29025
```

<br>

### Discription

SUM OVER 동작 방식

OVER 절을 사용하면 **GROUP BY 절을 사용하지 않고도 SELECT 절에서 단독으로 합계를 구할 수 있습니다.** OVER 절 내부의 ORDER BY 절의 칼럼 순서로 누적 합계가 표시되며, 조회된 결과도 해당 컬럼으로 정렬됩니다

> ORDER BY 절에 선언된 칼럼의 값에 따라서 누적 합계 표시 형식이 달라질 수 있으므로 주의해야 한다.

윈도우 Function의 SUM OVER를 사용하면 누계 계산이 간단해집니다. ORDER BY 절에는 SAL 열뿐만 아니라 총합에서 중복 값이 발생하지 않도록 EMPNO 열(기본 키)도 포함됩니다. 다음 예제의 RUNNING_TOTAL2 열에는 중복과 관련하여 발생할 수 있는 문제가있습니다

~~~sql
select empno, sal,
       sum(sal)over(order by sal,empno) as running_total1,
       sum(sal)over(order by sal) as running_total2
  from emp
 order by 2
~~~

WARD, MARTIN, SCOT 및 FORD에 대한 RUNING_TOTAL2의 급여는 두 번 이상 발생하며, 이러한 중복 항목은 합산되어 누계에 추가됩니다. 따라서 RUNING_TOTAL1에 표시된 (정확한) 결과를 생성하기 위해 EMPNO가 필요합니다.

![](img/7-6.png)

ADAMS의 경우 RUNING_TOTAL1 및 RUNING_TOTAL2에 대한 2850이 표시됩니다. WARD의 급여 1250을 2850에 더하면 4100을 받는데 RUNING_TOTAL2는 5350을 반환합니다.

 WARD와 MARTIN은 동일한 SAL을 가지고 있기 때문에, 두 개의 1250 급여를 합산하여 2500을 산출한 다음, WARD와 MARTIN 모두에 대해 5350에 도달하기 위해 2850에 추가됩니다. 순서를 지정할 열의 조합을 지정하면 중복 값이 발생할 수 없습니다 (예: SAL과 EMPNO의 조합이 고유함). 실행 총계가 올바르게 진행됩니다.
