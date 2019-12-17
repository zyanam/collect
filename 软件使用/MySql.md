### 获取时间里的日期

```mysql
select date(now());
2019-12-04
```

### 以时间为条件查询

```mysql
#今天
select * from 表名 where to_days(时间字段名) = to_days(now());

#指定日期
select * from history_video_1078 where sim_id = '013100000000' and TO_DAYS(record_time) = TO_DAYS(DATE('2019-12-3'))

#昨天
SELECT * FROM 表名 WHERE TO_DAYS( NOW( ) ) - TO_DAYS( 时间字段名) <= 1

#近7天
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(时间字段名)

#本月
SELECT * FROM 表名 WHERE DATE_FORMAT( 时间字段名, '%Y%m' ) = DATE_FORMAT( CURDATE( ) , '%Y%m' )

```

