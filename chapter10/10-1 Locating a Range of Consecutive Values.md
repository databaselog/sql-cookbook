## 10-1 Locating a Range of Consecutive Values
### Problem
연속된 프로젝트의 범위를 나타내려고 한다.
- 프로젝트의 시작 및 종료 날짜가 포함된 View, V에서 연속된 프로젝트만 조회한다.

~~~sql
    PROJ_ID PROJ_START  PROJ_END
    ------- ----------- -----------
          1 01-JAN-2020 02-JAN-2020
          2 02-JAN-2020 03-JAN-2020
          3 03-JAN-2020 04-JAN-2020
          4 04-JAN-2020 05-JAN-2020
          5 06-JAN-2020 07-JAN-2020
          6 16-JAN-2020 17-JAN-2020
          7 17-JAN-2020 18-JAN-2020
          8 18-JAN-2020 19-JAN-2020
          9 19-JAN-2020 20-JAN-2020
         10 21-JAN-2020 22-JAN-2020
         11 26-JAN-2020 27-JAN-2020
         12 27-JAN-2020 28-JAN-2020
         13 28-JAN-2020 29-JAN-2020
         14 29-JAN-2020 30-JAN-2020
         
         
    PROJ_ID PROJ_START  PROJ_END
    ------- ----------- -----------
         1  01-JAN-2020 02-JAN-2020
         2  02-JAN-2020 03-JAN-2020
         3  03-JAN-2020 04-JAN-2020
         6  16-JAN-2020 17-JAN-2020
         7  17-JAN-2020 18-JAN-2020
         8  18-JAN-2020 19-JAN-2020
        11  26-JAN-2020 27-JAN-2020
        12  27-JAN-2020 28-JAN-2020
        13  28-JAN-2020 29-JAN-2020
~~~

<br>

### Solution
- 
```sql
    1 select proj_id, proj_start, proj_end
    2   from (
    3 select proj_id, proj_start, proj_end,
    4        lead(proj_start)over(order by proj_id) next_proj_start
    5   from V
    6        ) alias
    7 where next_proj_start = proj_end
```



<br>

### Description

1. 

