# 4.16 Deleting Duplicate Records

### 퀴즈

테이블에서 중복된 레코드를 삭제하려고 합니다.

~~~sql
 create table dupes (id integer, name varchar(10))
    insert into dupes values (1, 'NAPOLEON')
    insert into dupes values (2, 'DYNAMITE')
    insert into dupes values (3, 'DYNAMITE')
    insert into dupes values (4, 'SHE SELLS')
    insert into dupes values (5, 'SEA SHELLS')
    insert into dupes values (6, 'SEA SHELLS')
    insert into dupes values (7, 'SEA SHELLS')
    
 select * from dupes order by 1;
            ID NAME
    ---------- ----------
             1 NAPOLEON
             2 DYNAMITE
             3 DYNAMITE
             4 SHE SELLS
             5 SEA SHELLS
             6 SEA SHELLS
             7 SEA SHELLS
~~~

<br>

### IN + SUBQUERY + AGGREGATE

`MIN` 과 같은 집계 함수와 함께 서브쿼리를 사용하여 유지할 ID를 남겨두고 삭제합니다.

~~~sql
delete from dupes
where id not in ( select min(id)
                     from dupes
                    group by name )
~~~

#### MySQL

MySQL의 경우 행 삭제 시 동일한 테이블을 두 번 참조할 수 없기 때문에 다른 Syntax를 사용해야 합니다.

~~~sql
delete from dupes
where id not in (
  select min(id)
  from (select id,name from dupes) tmp
  group by name
)
~~~

### TIP

중복 항목을 제거 할 때 가장 먼저 해야 할 일은 두 행이 서로 "중복된다"라는 것이 정확히 무엇을 의미하는지 정의하는 것입니다.

~~~sql
select min(id)
from dupes
group by name

        MIN(ID)
    -----------
              2
              1
              5
              4
~~~

해당 예시에서 "중복"의 정의는 **두 레코드의 NAME 열에 동일한 값이 포함되어 있다는 것입니다**. 

**핵심은 중복된 값(예시에선 NAME 기준)으로 그룹화한 다음 집계 함수를 사용하여 유지할 하나의 키 값만 남겨두는것입니다**. 위 서브쿼리는 삭제대상이 아닌 각 NAME에 대해 가장 작은 ID를 반환합니다.

~~~sql
select name, min(id)
from dupes
group by name

    NAME          MIN(ID)
    ---------- ----------
    DYNAMITE            2
    NAPOLEON            1
    SEA SHELLS          5
    SHE SELLS           4
~~~

그런 다음, `DELETE` 는 서브 쿼리에서 반환되지 않은 테이블의 모든 ID(위 경우 ID 3, 6 및 7)를 삭제합니다. 쿼리가 어떻게 동작하는지 확인 하고 싶은 경우 NAME을 포함하여 서브 쿼리를 실행해보세요!
