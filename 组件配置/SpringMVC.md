### 文件头

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
</beans>    
```

### 配置包扫描

```xml
<context:component-scan base-package="com.yanam"/>
```

### 编码问题

```xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes" value="text/html;charset=UTF-8"></property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

### 视图解析器

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/"></property>
    <property name="suffix" value=".jsp"></property>
</bean>
```

### 处理全局异常

```java
public class MyExceptionResolver implements HandlerExceptionResolver {

    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
//        String url = request.getRequestURI();
        String url = request.getHeader("Referer");
        String title = "";
        String msg = "";
        if (ex instanceof UnauthorizedException) {
            title = "权限不足";
            msg = "没有权限，请联系管理员!";
        } else {
            title = "运行错误";
            msg = ex.getMessage();
        }

        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("/auth/unauthorized");
        modelAndView.addObject("url", url);
        modelAndView.addObject("title", title);
        modelAndView.addObject("msg", msg);

        return modelAndView;
    }
    
    //加入容器
    @Bean
    public MyExceptionResolver myExceptionResolver() {
        return new MyExceptionResolver();
    }

```

