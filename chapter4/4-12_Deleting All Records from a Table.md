# 4.12 Deleting All Records from a Table

### 퀴즈

테이블에 모든 레코드를 삭제하려고합니다. 


<br>

### DELETE

아래와 같이 `DELETE` 문만 사용하여 테이블의 모든 레코드를 삭제합니다.

~~~sql
1 delete from emp
~~~

#### 주의사항

`WHERE` 절 없이 `DELETE` 명령을 수행하면 지정된 테이블에서 모든 행이 삭제됩니다. 

**동일한 명령어인 `TRUNCATE` 도 WHERE 절을 사용하지 않고 더 빠르기 때문에 DELETE보다 선호하는편입니다.** 하지만 `Oracle` 에서는 `TRUNCATE` 를 `UNDO` 할 수 없기에 주의해야합니다. 

또한 `TRUNCATE` 와 `DELETE` 간의 성능 및 롤백 방안의 차이는 각 벤더사별로 다르기 때문에 하나씩 살펴볼 필요가 있습니다.



## DELETE vs TRUNCATE의 성능 차이와 사용성

`TRUNCATE` 는 삭제하기 전에 모든 레코드를 스캔하지 않기 때문에 `DELETE` 보다 빠릅니다.

1. `TRUNCATE` `TABLE은` **테이블에서 데이터를 제거하기 위해 전체 테이블을 잠급니다**. 따라서 이 명령도 `DELETE` 보다 적은 트랜잭션 공간을 사용합니다.

2. `DELETE` 와 달리 `TRUNCATE` 는 테이블에서 삭제된 행 수를 반환하지 않습니다. 또한 테이블 `AUTO INCREMENT` 값을 시작 값(일반적으로 1)으로 재설정합니다. (예: 테이블을 비운 후 레코드를 추가하면 ID=1이 됩니다)
3. 반대로 **Oracle**은 `TRUNCATE` 명령으로 리셋되지 않는 `SEQUENCE INCREMENT` 값을 사용합니다. (테이블을 비운 후 레코드를 추가하면 기존에 유지하던 INCREMENT 값이 사용 됩니다)
4. **MySQL 과 Oracle에선 `TRUNCATE` 수행 후 `ROLLBACK` 이 불가합니다.**
