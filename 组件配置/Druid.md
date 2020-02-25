### 配置YML

```yaml
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://127.0.0.1:3306/jdbc
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
#    schema:
#      - classpath:department.sql
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validtionQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters,去掉监控界面sql无法统计，"wall" 用于防火墙
#    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: durid.stat.mergeSql=true;durid.stat.slowSqlMills=500
```




### 配置监控

```java
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid() {
        return new DruidDataSource();
    }

    @Bean
    public ServletRegistrationBean statViewServlet() {
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String, String> initParams = new HashMap<>();
        initParams.put(ResourceServlet.PARAM_NAME_USERNAME, "admin");
        initParams.put(ResourceServlet.PARAM_NAME_PASSWORD, "123456");
        bean.setInitParameters(initParams);
        return bean;
    }

    @Bean
    public FilterRegistrationBean webStateFilter() {
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        Map<String, String> initParams = new HashMap<>();
        initParams.put(WebStatFilter.PARAM_NAME_EXCLUSIONS, "*.js,*.css,/druid/*");
        bean.setInitParameters(initParams);

        bean.setUrlPatterns(Arrays.asList("/*"));
        return bean;
    }
}
```

### springboot 

```yml
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/dbshiro?useUnicode=true&CharacterEncoding=UTF-8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      initial-size: 1
      max-active: 5
      min-idle: 1
      max-wait: 10000

      #配置间隔多久启动一次 DestroyThread，对连接池内的连接才进行一次检测，单位是毫秒。
      #检测时:
      # 1.如果连接空闲并且超过minIdle以外的连接，如果空闲时间超过minEvictableIdleTimeMillis设置的值则直接物理关闭。
      # 2.在minIdle以内的不处理。
      time-between-eviction-runs-millis: 600000

      #配置一个连接在池中最大空闲时间，单位是毫秒
      min-evictable-idle-time-millis: 300000

      #设置从连接池获取连接时是否检查连接有效性，
      # true时，每次都检查;
      # false时，不检查
      test-on-borrow: false

      #设置往连接池归还连接时是否检查连接有效性，
      # true时，每次都检查;
      # false时，不检查
      test-on-return: false

      #设置从连接池获取连接时是否检查连接有效性，
      # true时，如果连接空闲时间超过minEvictableIdleTimeMillis进行检查，否则不检查;
      # false时，不检查 -->
      test-while-idle: true

      #检验连接是否有效的查询语句。
      #如果数据库Driver支持ping()方法，则优先使用ping()方法进行检查，
      #否则使用validationQuery查询进行检查。(Oracle jdbc Driver目前不支持ping方法)
      validation-query: select 1 from dual

      #单位：秒，检测连接是否有效的超时时间。底层调用jdbc Statement对象的void setQueryTimeout(int seconds)方法
      #validation-query-timeout: 1

      #打开后，增强timeBetweenEvictionRunsMillis的周期性连接检查，minIdle内的空闲连接，每次检查强制验证连接有效性.
      #参考：https://github.com/alibaba/druid/wiki/KeepAlive_cn -->
      keep-alive: true

      #接泄露检查，打开removeAbandoned功能 , 连接从连接池借出后，长时间不归还，将触发强制回连接。回收周期随timeBetweenEvictionRunsMillis进行，
      #如果连接为从连接池借出状态，并且未执行任何sql，并且从借出时间起已超过removeAbandonedTimeout时间，则强制归还连接到连接池中。
      remove-abandoned: true

      # 超时时间，秒
      remove-abandoned-timeout: 80

      #关闭abanded连接时输出错误日志，这样出现连接泄露时可以通过错误日志定位忘记关闭连接的位置
      log-abandoned: true

      # 根据自身业务及事务大小来设置
      connection-properties: oracle.net.CONNECT_TIMEOUT=2000;oracle.jdbc.ReadTimeout=10000

      #打开PSCache，并且指定每个连接上PSCache的大小，Oracle等支持游标的数据库，打开此开关，会以数量级提升性能，具体查阅PSCache相关资料
      pool-prepared-statements: true
      max-pool-prepared-statement-per-connection-size: 20

      #配置监控统计拦截的filters
      filters: stat,wall
      filter:
        stat:
          log-slow-sql: true
          slow-sql-millis: 2000
      stat-view-servlet:
        login-username: admin
        login-password: 123456
        reset-enable: false
        url-pattern: /druid/*
        enabled: true
      web-stat-filter:
        url-pattern: /*
        exclusions: "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*"
```

