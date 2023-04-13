## chapter7 숫자 작업

- DBMS에서 제공하는 집계 함수 사용. 
- 집계 함수가 null을 어떻게 다루는지 알고 있어야 한다.
- 집계 함수 호출 시 null 처리 방법을 항상 고려해야 한다.
- select 목록에 포함시키는 집계 함수 외의 열은 group by절에 반드시 기재되어야 한다. 

---

## NULL허용 컬럼 집계
1. NULL 허용 열 집계하기
   -  관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-12_null%20%ED%97%88%EC%9A%A9%20%EC%97%B4%20%EC%A7%91%EA%B3%84%ED%95%98%EA%B8%B0.md



---

## 집계 함수 

1. 평균 avg() 
   - null값 제외하고 평균 구함
   - null도 포함하고 싶다면, coalesce같은 null처리 함수 사용.  
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-1_%ED%8F%89%EA%B7%A0_%EA%B3%84%EC%82%B0%ED%95%98%EA%B8%B0.md

2. min(), max()
   - 함수가 지정한 열에 대해 모두 null 값이라면 null을 반환하지만, 값이 1개라도 있다면 null값이 무시된다.
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-2_%EC%97%B4%EC%97%90%EC%84%9C_%EC%B5%9C%EB%8C%93%EA%B0%92%2C%EC%B5%9C%EC%86%9F%EA%B0%92_%EC%B0%BE%EA%B8%B0.md

3. sum() 
   - sum 함수는 null을 무시하지만, null 외의 값이 없는 경우 null을 가진다. min, max와 같은 동작
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-3_%EC%97%B4%EC%9D%98%EA%B0%92_%EC%A7%91%EA%B3%84%ED%95%98%EA%B8%B0.md

4. count()
   - count 함수는 특정 열 이름을 인수로 전달하면 null을 무시하고 카운트하지만, ‘*’ 문자나 상수를 전달하면 Null값을 카운트에 포함시킨다.
   - 컬럼을 셀 때는 해당 열의 NON-NULL의 수를 계산
   - COUNT(*)에서와 같이 '별'을 카운트할 때 실제 카운트하는 것은 행
   - 관련 링크
     - https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-4_%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%98_%ED%96%89%EC%88%98_%EA%B3%84%EC%82%B0%ED%95%98%EA%B8%B0.md
     - https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-5%20Counting%20Values%20in%20a%20Column.md

5. 총계에서 백분율 계산
   - 형 변환 함수 : CAST()와 CONVERT() 
   - 참고 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-11_%EC%B4%9D%EA%B3%84%EC%97%90%EC%84%9C%EC%9D%98%20%EB%B0%B1%EB%B6%84%EC%9C%A8%20%EC%95%8C%EC%95%84%EB%82%B4%EA%B8%B0.md


---

## 윈도우 함수

1. 누계(누적 합계) 계산
   - sum(sal) over(order by sal, empno) 
   - sum over 동작방식
     - `OVER 절을 사용하면 GROUP BY 절을 사용하지 않고도 SELECT 절에서 단독으로 합계를 구할 수 있습니다.` 
     - `ORDER BY 절에는 SAL 열뿐만 아니라 총합에서 중복 값이 발생하지 않도록 EMPNO 열(기본 키)도 포함됩니다.`
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-6%20Generating%20a%20Running%20Total.md
   - 질문 
     - order by가 또 있어야 할까? 이미 누계에서 order by 된 것은 아닌지?
   ```sql
   select ename, sal,
   sum(sal) over (order by sal, empno) as running_total
   from emp
   order by 2;
   ```

2. LAG, LEAD
   - 이전 행 값, 이후 행 값
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-8%20Smoothing%20a%20Series%20of%20Values.md

3. RANK(), DENSE_RANK(), ROW_NUMBER() 비교 
   - 최빈값(주어진 값 중에서 가장 자주 나오는 값)
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-9_%EC%B5%9C%EB%B9%88%EA%B0%92%20%EA%B3%84%EC%82%B0%ED%95%98%EA%B8%B0.MD

4. CUME_DIST() 백분율을 반환
   - 중앙값 계산에 활용
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter7/7-10_%EC%A4%91%EC%95%99%EA%B0%92%20%EA%B3%84%EC%82%B0%ED%95%98%EA%B8%B0.md

