## 2.1 Returning Query Results in a Specified Order

### 퀴즈

부서 ID 10의 직원들의 이름, 직책 및 급여를 낮은 순서에서 높은 순서로 표시하려고 합니다. **아래와 같이 결과를 만들려면 어떻게 해야할까요?**

~~~sql
ENAME      JOB           SAL
--------   --------- 	   ------
CLARK      MANAGER       1300 
KING       PRESIDENT     2450
MILLER     CLERK         5000
~~~

&nbsp;

### Order By

`order by` 구문을 통해 해결 할 수 있습니다. 정렬 순서를 명시를 안해주면 **기본적으로 `ASC` 순서대로 정렬**이 되기 때문에, 반대로 정렬하고 싶다면 `DESC` 옵션을 뒤에 붙혀줘야 합니다. 

~~~sql
select ename,job,sal
from emp
where deptno = 10
order by sal desc
~~~

**컬럼명을 제외하고 쿼리를 수행해도 정렬이 가능**합니다.

~~~sql
select ename,job,sal
from emp
where deptno = 10
order by 3 desc
~~~

&nbsp;

### 문자의 정렬 순서

알파벳이라면 `A`가 가장 작은 값이고 `Z`가 가장 큰 값입니다. 한글이라면 `ㄱ`이 가장 작고 `ㅎ`이 가장 큰 값입니다.

***그렇다면 영어와 한글간에는 어떤 값이 큰 값일까?***

***그렇다면 특수문자와 숫자들은...?!***

![](https://wikidocs.net/images/page/131182/%EA%B7%B8%EB%A6%BC_3_2_1_1.png)

DBMS나 언어 셋, 환경에 따라 다를 수 있지만, 문자 데이터는 대체로 위 **네 그룹을 기준으로 정렬 순서를 갖습니다.**

&nbsp;

#### 궁금하면 테스트 해보세요!

~~~sql
with T as (
	select '1' as col from dual union all
	select 'a' as col from dual union all
	select 'ㄱ' as col from dual union all
	select '!' as col from dual union all
	select 'な' as col from dual
) select col from T order by col;
~~~

***질문의 늪 :*** ***그렇다면 특수문자끼리는...?!***

**남규 : ASCII Code..?**

![](https://qph.cf2.quoracdn.net/main-qimg-40eb504e356af0832441ef95091eba8e)