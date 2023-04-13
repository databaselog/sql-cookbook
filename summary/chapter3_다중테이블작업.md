## chapter3 다중 테이블 작업

조인, 집합연산 다루는 챕터

1. 공통키 없이 테이블 조인 
   - union (all)
   - 중복 데이터 필터하고 싶다면 union을 사용. 그러나 성능에서 좋지 못하다. 
   - union all과 distinct를 사용해 중복 데이터 필터를 할 수도 있다.
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-1_Stacking%20One%20Rowset%20atop%20Another.md
   - union, union all 차이 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-7%EB%91%90_%ED%85%8C%EC%9D%B4%EB%B8%94%EC%97%90_%EB%8F%99%EC%9D%BC%ED%95%9C_%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B0%80_%EC%9E%88%EB%8A%94%EC%A7%80_%ED%99%95%EC%9D%B8%ED%95%98%EA%B8%B0.md

2. inner join 
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-2_%EC%97%B0%EA%B4%80%EB%90%9C%20%EC%97%AC%EB%9F%AC%20%ED%96%89%20%EA%B2%B0%ED%95%A9%ED%95%98%EA%B8%B0.md

3. 유니크하지 않은 여러 컬럼으로 조인하는 경우 
   - where 조건이 and 결함으로 여러 개이다. 
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-3%EB%91%90%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%98%EA%B3%B5%ED%86%B5%ED%96%89%EC%B0%BE%EA%B8%B0.md

4. 차집합(anti-join)을 얻는 조인 
   - 차집합을 명령어로 지원하는 벤더
   - 명령어 없을 경우, 
      1) left join 과 is null조합 
      2) not exist
      3) not in : nullable할 때는 정확한 값을 얻을 수 없다.
   - 관련 링크 
     - https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-4_%ED%95%9C%ED%85%8C%EC%9D%B4%EB%B8%94%EC%97%90%EC%84%9C_%EB%8B%A4%EB%A5%B8%ED%85%8C%EC%9D%B4%EB%B8%94%EC%97%90_%EC%A1%B4%EC%9E%AC%ED%95%98%EC%A7%80%EC%95%8A%EB%8A%94%EA%B0%92_%EA%B2%80%EC%83%89%ED%95%98%EA%B8%B0.md
     - https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-5_Retrieving%20Rows%20from%20One%20Table%20That%20Do%20Not%20Correspond%20to%20Rows%20in%20Another.md
    - 스칼라 서브 쿼리를 사용하는 경우
      - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-6_%EB%8B%A4%EB%A5%B8%20%EC%A1%B0%EC%9D%B8%EC%9D%84%20%EB%B0%A9%ED%95%B4%ED%95%98%EC%A7%80%20%EC%95%8A%EA%B3%A0%20%EC%BF%BC%EB%A6%AC%EC%97%90%20%EC%A1%B0%EC%9D%B8%20%EC%B6%94%EA%B8%B0%ED%95%98%EA%B8%B0.md

5. full outer join 
   - full outer join 명령을 제공하는 벤더
   - 제공하지 않을 때는 (right outer) union (left outer)  
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-11%EC%97%AC%EB%9F%AC%ED%85%8C%EC%9D%B4%EB%B8%94%EC%97%90%EC%84%9C%EB%88%84%EB%9D%BD%EB%90%9C%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B0%98%ED%99%98%ED%95%98%EA%B8%B0.md


---

### 조인 조건 기본 원칙 
1. 데카르트 곱 방지 
   - n-1의 최소 조인 조건 
   - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-8_%EB%8D%B0%EC%B9%B4%EB%A5%B4%ED%8A%B8_%EA%B3%B1_%EC%8B%9D%EB%B3%84%EB%B0%8F%EB%B0%A9%EC%A7%80%ED%95%98%EA%B8%B0.md

2. 조인과 집계가 서로 방해되지 않도록.(집계 중복 방지)
   - 어떤 합을 집계 할 때는 특히나 값이 중복되지 않게 신경 그리고 또 신경을 써야합니다. 물론 `데이터 검증은 필수`이구요!
     - distinct 이용 
     - 윈도우함수 sum over 이용
   - 관련 링크 
     - ttps://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-9_Performing%20Joins%20When%20Using%20Aggregates.md
     - https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-10_%EC%A7%91%EA%B3%84%20%EC%8B%9C%20%EC%99%B8%EB%B6%80%20%EC%A1%B0%EC%9D%B8%20%EC%88%98%ED%96%89%ED%95%98%EA%B8%B0.md


### 참고
- 연산, 비교에서 null 사용
  - `coalesce` 이용
  - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter3/3-12_%EC%97%B0%EC%82%B0%EB%B0%8F%EB%B9%84%EA%B5%90%EC%97%90%EC%84%9C_null%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0.md