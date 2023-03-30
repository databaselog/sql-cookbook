## 7-12. NULL 허용 열 집계하기
___
### 문제
집계의 정확성을 유지하기 위해 null을 허용하고자 함  
예) `deptno`=30인 사원의 평균 커미션 산출  

null을 무시하면 커미션을 받는 4명의 평균만 산출  
null을 포함하면 부서 전원 6명의 평균 산출

### 해법
`COALESCE()` 함수 사용
```sql
SELECT avg(coalesce(com,0)) as avg_comm
FROM emp
WHERE deptno=10
```

### 설명 
1. COALESCE() 함수  
COALESCE(`NULL을 포함하려는 열`, `NULL을 대체할 값`)  

2. 데이터 선정  
4명인지, 6명인지에 대한 의도가 있어야 함  
`NULL`을 받지 않는 사람으로 정의하는지, 수령 여부를 알 수 없는 사람으로 정의하는 지 등  
목적에 따라 활용해야 한다



