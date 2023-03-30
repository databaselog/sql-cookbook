## 7.7 Generating a Running Product
### Problem
숫자 열에서 누적곱(running product)을 계산하고자 한다.
- 모든 직원의 누적곱을 계산하고자 한다.

<br>

### Solution
- 윈도우 함수 SUM OVER를 사용하고 로그(logarithm)를 추가하여 곱셈을 시뮬레이션!
```sql
select empno,ename,sal,
       exp(sum(ln(sal))over(order by sal,empno)) as running_prod
  from emp
 where deptno = 10

EMPNO ENAME          SAL         RUNNING_PROD
----- ----------    ---- --------------------
 7934 MILLER        1300                 1300
 7782 CLARK         2450              3185000
 7839 KING          5000          15925000000
```

- 0보다 작거나 같은 값의 로그를 계산하는것은 SQL(수학적 접근)에서 유효하지 않다
- 만약 음수 및 0이 포함된 값으로 작업해야하는 경우 해당 방법은 적절치 않음
- 하지만 꼭 진행해야한다면 0대신 1로 대체하여 계산

>  SQL Serversms LN 대신 LOG사용

<br>

### Discription

1. 각각의 자연로그 계산
1. 로그를 합산
1. 결과를 상수의 e 거듭제곱으로 올림 (EXP 사용)

0보다 작거나 같은값은 SQL 로그의 범위를 벗어나므로 0또는 음숫값에 합은 사용할수 없음

윈도우 함수 SUM OVER의 작동방식에 대한 설명은 7.6참조

