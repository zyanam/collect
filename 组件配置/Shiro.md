### 权限注解

```java
@RequiresAuthentication
表示当前Subject已经通过login进行了身份验证；即Subject.isAuthenticated()返回true；

@RequiresUser
表示当前Subject已经身份验证或通过记住我登录的

@RequiresGust
表示当前Subject没有身份验证或通过记住登陆过，即游客身份

@RequiresRoles(value={"admin","user"},logical=Logical.AND)
表示当前Subject需要角色 admin 和 user

@RequiresPermissions(value={"user:a","user:b"},logical=Logical.OR)
表示当前Subject需要权限user:a 或 user:b
   	
```

### 设置 setUnauthorizedUrl 无效

```java
@Bean
public SimpleMappingExceptionResolver resolver() {
    SimpleMappingExceptionResolver resolver = new SimpleMappingExceptionResolver();
    Properties properties = new Properties();
    properties.setProperty("org.apache.shiro.authz.UnauthorizedException", "/auth/unauthorized");
    resolver.setExceptionMappings(properties);
    return resolver;
}
```

