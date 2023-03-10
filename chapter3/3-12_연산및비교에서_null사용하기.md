# 3. 다중 테이블 작업
## 3-12. 연산 및 비교에서 null 사용하기
### #Problem
Null을 허용하는 칼럼에서 반환된 값을 비교해보자.<br>
`comm` 칼럼이 `ename`=‘WARD’인 사원의 `comm` 칼럼 데이터보다 적은 모든 사원을 조회하고자 한다.

### #Solution
Null값 대체 함수인 `coalesce`를 이용한다.

```sql
select ename, comm, coalesce(comm, 0)
  from emp
where coalesce(comm, 0) < (
  select comm -- ename=‘WARD’인 사원의 comm 데이터는 null값이 없는 경우라서 그냥 조회한 것으로 추정..
  from emp 
  where ename = 'WARD'
)
```

### #Discription
- `coalesce` 함수 : 전달된 값 목록에서 null이 아닌 첫번째 값 반환