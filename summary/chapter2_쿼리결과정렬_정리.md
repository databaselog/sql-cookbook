# chapter2 쿼리 결과 정렬 정리

1. 정렬 시 `특수문자 < 문자형 숫자 < 알파벳 < 한글` 순으로 크다
  - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter2/2-1_Returning%20Query%20Results%20in%20a%20Specified%20Order.md

2. null값 포함되었을 때 정렬 
  - null 값이 포함되면 `정렬의 기준이 다양`해지고 그것은 `사용자가 정의`하는 것!
  - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter2/2-5_Dealing%20with%20Nulls%20When%20Sorting.md

---

### 문자열에 대한 함수도 소개 

3. DB는 INDEX를 0이 아닌 1부터 시작한다. 
  - substr('abc',1,1) -> 결과 : a
  - 1부터 1개 가져오는 함수 
  - 관련 링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter2/2-3%EB%B6%80%EB%B6%84%EB%AC%B8%EC%9E%90%EC%97%B4%EB%A1%9C%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0.md

4. translate, replace 차이
  - 관련링크 : https://github.com/databaselog/sql-cookbook/blob/main/chapter2/2-4_%ED%98%BC%ED%95%A9_%EC%98%81%EC%88%AB%EC%9E%90%EB%8D%B0%EC%9D%B4%ED%84%B0_%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0.md
