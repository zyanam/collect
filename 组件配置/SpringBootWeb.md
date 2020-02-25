### 生成LOGO

http://patorjk.com/software/taag/#p=display&f=Graffiti&t=hello

### 静态资源的映射规则

**WebMvcAutoConfiguration.java**

```java
		@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
			CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
			if (!registry.hasMappingForPattern("/webjars/**")) {
				customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
						.addResourceLocations("classpath:/META-INF/resources/webjars/")
						.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
			}
			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
			if (!registry.hasMappingForPattern(staticPathPattern)) {
				customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
						.addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
						.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
			}
		}
```



- 访问路径 /webjars/**映射到classpath:/META-INF/resources/webjars/；

  - webjars:以jar包的方式引入静态资源； https://www.webjars.org/ 

- 访问路径 /**默认映射到以下路径，

  - ```java
    private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
        "classpath:/META-INF/resources/",			
        "classpath:/resources/", 
        "classpath:/static/", 
        "classpath:/public/" 
    };
    ```

  - 当前项目的根路径

  - 也可以通过"spring.resources.static-locations="自定义，自定义后，默认的失效。

- 默认页面；静态资源文件夹下的所有index.html页面；

### 模板引擎

- JSP、Velocity、Freemarker、Thymeleaf

#### Thymeleaf

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

