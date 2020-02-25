### Setting XML

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

### Mapper XML

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```



### 全局配置

```xml
        <settings>
            <!--启用驼峰映射-->
            <setting name="mapUnderscoreToCamelCase" value="true"/>
            <!--启用日志-->
            <setting name="logImpl" value="STDOUT_LOGGING" />
            <!--启用延迟加载-->
            <setting name="lazyLoadingEnabled" value="true"/>
        	<setting name="aggressiveLazyLoading" value="false"/>
        </settings>
```

### 嵌套结果集

```xml
    <resultMap id="AssociationEmp" type="com.yanam.bean.Employee">
        <id column="id_employee" property="idEmployee"></id>
        <result column="last_name" property="lastName"></result>
        <result column="gender" property="gender"></result>
        <result column="email" property="email"></result>
        <association property="department" javaType="com.yanam.bean.Department">
            <id column="id_department" property="idDepartment"/>
            <result column="department_name" property="departmentName"/>
        </association>
    </resultMap>
    <select id="getEmpAndDeptByID" resultMap="AssociationEmp">
        SELECT * FROM tbl_employee e,tbl_department d
        WHERE e.id_department = d.id_department
        AND e.id_employee = #{idEmployee}
    </select>
```

### 延迟加载

```xml
    <resultMap id="Employee" type="com.yanam.bean.Employee">
        <id column="id_employee" property="idEmployee"/>
        <result column="last_name" property="lastName"/>
        <result column="gender" property="gender"/>
        <result column="email" property="email"/>
        <association select="com.yanam.dao.DepartmentMapper.getDepartmentByID"
                     column="id_department"
                     property="department">
            <id column="id_department" property="idDepartment"/>
            <result column="department_name" property="departmentName"/>
        </association>
    </resultMap>
    <select id="getEmployeeByStep" resultMap="Employee">
        SELECT * FROM tbl_employee WHERE id_employee = #{id}
    </select>
```

### 对象中包含集合

```xml
    <resultMap type="com.yanam.bean.Department" id="departmentWithEmployees">
        <id column="id_department" property="idDepartment"/>
        <result column="department_name" property="departmentName"/>
        <collection property="employees" ofType="com.yanam.bean.Employee">
            <id column="id_employee" property="idEmployee"></id>
            <result column="last_name" property="lastName"></result>
            <result column="gender" property="gender"></result>
            <result column="email" property="email"></result>
        </collection>
    </resultMap>

    <select id="getDepartmentWithEmployees" resultMap="departmentWithEmployees">
        select * from tbl_department d
        left join tbl_employee e
        on d.id_department = e.id_department where d.id_department = #{idDepartment}
    </select>
```

### 对象中包含集合-分步

```xml
    <resultMap id="DepartmentStep" type="com.yanam.bean.Department">
        <collection select="com.yanam.dao.EmployeeMapperPlus.getEmployeesByDepartment"
                    column="id_department"
                    property="employees" ofType="com.yanam.bean.Employee"/>
    </resultMap>

    <select id="getDepartmentStep" resultMap="DepartmentStep">
        SELECT * FROM tbl_department
        WHERE id_department = #{idDepartment}
    </select>
```

### 传递多个值

```xml
    <resultMap id="DepartmentStep" type="com.yanam.bean.Department">
        <collection select="com.yanam.dao.EmployeeMapperPlus.getEmployeesByDepartment"
                    column="{key1=id_department,key2=value2"
                    property="employees" ofType="com.yanam.bean.Employee"/>
    </resultMap>

    <select id="getDepartmentStep" resultMap="DepartmentStep">
        SELECT * FROM tbl_department
        WHERE id_department = #{idDepartment}
    </select>
```

### 鉴别器

```xml
    <resultMap id="DiscriminatorMap" type="com.yanam.bean.Employee">
        <discriminator javaType="string" column="gender">
            <case value="1" resultType="com.yanam.bean.Employee">
                <result column="last_name" property="email"></result>
                <result column="last_name" property="lastName"></result>
            </case>
            <case value="0" resultType="com.yanam.bean.Employee">
                <result column="email" property="lastName"></result>
                <result column="email" property="email"></result>
            </case>
        </discriminator>
    </resultMap>
    <select id="getEmployeeByDiscriminator" resultMap="DiscriminatorMap">
        SELECT * FROM tbl_employee WHERE id_department = #{idDepartment}
    </select>
```

### Bind

```xml
<bind name="_lastName" value="'%'+lastName+'%'"></bind>
<select id="">
    select * from tbl where last_name=#{_lastName}
</select>
```

### 一级缓存

失效情况

1. 不同的SqlSession
2. SqlSession相同，查询条件不同
3. SqlSession相同，但两次查询中间有增删改操作
4. 手动清除缓存，SqlSession.clearCache();

### 二级缓存

SqlSession关闭以后，才进入二级缓存

> 1. 全局设置 cacheEnabled=true
> 2. 在mapper标签里加入cache标签
>    1. cache标签的属性
>    2. eviction:缓存策略
>       1. LRU 最近最少使用；移除最长时间不被使用的对象。（默认策略）
>       2. FIFO 先进先出
>       3. SOFT 软引用；移除基于垃圾回收器状态和软引用规则的对象。
>       4. WEAK 弱引用；更积极的移除基于垃圾收集器状态和弱引用规则的对象。
>    3. flushInterval 缓存刷新时间，单位ms
>    4. readOnly 是否只读
>       1. true 返回对象的引用，不安全，速度快。
>       2. false 返回克隆对象，安全，速度慢。
>    5. size 缓存对象的个数。
>    6. type 自定义的缓存实现的全类名，可以自己实现cache接口。
> 3. 在readOnly=false时，POJO需要时间serializable接口