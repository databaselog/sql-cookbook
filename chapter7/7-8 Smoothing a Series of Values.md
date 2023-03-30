## 7.8 Smoothing a Series of Values
### Problem
월별 판매 수치와같은 시간에 따라 나타나는 일련의 값이 있다고 할때 추세를 더 잘 식별하기 위해 이동 평균을 구하고자 한다
- 신문 가판대의 일별 판매 총액을 달러로 계산하고자 한다

~~~sql
    DATE1            SALES
    2020-01-01         647
    2020-01-02         561
    2020-01-03         741
    2020-01-04         978
    2020-01-05        1062
    2020-01-06        1072
~~~

데이터의 변동성 ß떄문에 근본적인 추세를 파악하기 어려움

<br>

### Solution
- 이동 평균은 이전 값 (n-1)을 합한 후 n으로 나누어 계산 할 수 있다.
- 윈도우 함수 LAG를 사용하여 이동평균 계산
```sql
 select date1, sales,lag(sales,1) over(order by date1) as salesLagOne,
    lag(sales,2) over(order by date1) as salesLagTwo,
    (sales
    + (lag(sales,1) over(order by date1))
    + lag(sales,2) over(order by date1))/3 as MovingAverage
    from sales
```

![](img/7-8-1.png)

#### LAG & LEAD 함수

- 현재행(Current Row) 기준으로 이전 값들과 이후 값들을 다뤄야 할 때 사용됨.

~~~sql
INSERT INTO login_data
    (id, login_date)
VALUES
    ('a20', '2022-01-03 00:00:00'),
    ('a20', '2022-04-04 00:00:00'),
    ('a20', '2022-08-07 00:00:00'),
    ('a20', '2022-12-11 00:00:00'),
    ('b11', '2020-01-03 00:00:00'),
    ('b11', '2021-08-03 00:00:00'),
    ('b11', '2022-01-17 00:00:00'),
    ('c30', '2021-03-04 00:00:00'),
    ('c30', '2021-06-01 00:00:00');
~~~

##### LAG

-  id 마다 이전 로그인 시간을 알 수 있는 prev_login Column까지 조회할 수 있음.
- 이전 값이 없을 경우 NULL 반환

~~~sql
SELECT id, login_date, LAG(login_date,1) OVER (
        PARTITION BY id
        ORDER BY login_date ) prev_login
FROM 
    login_data;
~~~

![](img/7-8-2.png)

<br>

##### LEAD

- id마다 이후 로그인 시간을 알 수 있는 next_login Column까지 조회할 수 있음.
- 이후 값이 없을 경우 NULL 반환

~~~sql
SELECT id, login_date, LEAD(login_date,1) OVER (
        PARTITION BY id
        ORDER BY login_date ) next_login
FROM 
    login_data
~~~

![](img/7-8-3.png)

<br>

