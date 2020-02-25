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

### 允许以分号分隔多条语句，一次执行

```xml
<!--allowMultiQueries-->    
<environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1/mybatis?useUnicode=true&amp;characterEncoding=UTF-8&amp;allowMultiQueries=true"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
```



### 中文乱码  显示问号

1. 先确定程序中没有问题，参考：https://blog.csdn.net/blueheart20/article/details/52385142

2. 使用以下语句查询数据库编码：

3. ```mysql
   show variables like 'char%'
   #看character_set_server的值，如果不是utf8要改为utf8
   
   set global character_set_server = utf8 #如果不起作用则需要改配置文件，my.cnf,加入如下内容
   
   
   [client]   # 新增客户端的编码
   default-character-set=utf8
    
   [mysql]   # 新增客户端的编码，缺省
   default-character-set=utf8
   
   
   [mysqld]
   # 新增 关于character_set_server的编码设置
   init-connect='SET NAMES utf8'
   character-set-server = utf8
   
   ```

4. 



