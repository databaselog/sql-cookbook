# 4. 삽입, 갱신, 삭제
## 4-3. null로 기본값 오버라이딩하기

### #Problem
기본값을 사용하는 열에 값 삽입 시, 열을 null로 설정해서 기본값을 오버라이딩하려고 한다.<br>
아래와 같은 테이블이 있을 때, **ID 칼럼값을 null값을 넣고 싶다.**

```sql
create table D (id integer **default 0,** foo VARCHAR(10))
```

### #Solution
값을 직접 명시해서 넣어주는 삽입(insert) 쿼리문에서 values 값으로 `null`을 지정할 수 있다.

```sql
insert into d (id, foo) values (null, 'Moon')
```

### #Discription
만약 `null`이니까 생략해야지! 한다면? `null`이 아닌 다른 값이 들어가는 문제가 발생한다.

```sql
insert into d (foo) values ('Moon')
```
- 결과는 `id`: 0 (기본 값) , `foo`: ‘Moon’이다.
    - `id`에는 null 값이 들어갈 것으로 예상하지만, **삽입 시 values 인자로 null을 지정해야하만 기본 값이 아닌 null로 지정할 수 있다.**
    - 따로 null을 지정하지 않으면 특정 칼럼의 기본 값이 들어간다. (예: 0)