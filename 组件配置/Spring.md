### Spring XML 模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="stu" class="com.southwind.entity.Student"></bean>
</beans>
```

### Spring 中的 bean 是根据 scope 生成的

| Scope     |                                               |
| --------- | --------------------------------------------- |
| singleton | 单例                                          |
| prototype | 原型，表示通过Spring 容器获取的对象都是不同的 |
| request   | 请求，表示在一次http请求内有效                |
| session   | 会话，表示在一个会话内有效                    |

