### 阿里中央库

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings> 
<localRepository>D:\Tools\java\maven\repository</localRepository><!--需要改成自己的maven的本地仓库地址-->
    <mirrors>
        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
  <profiles>
    <profile>
       <id>nexus</id> 
        <repositories>
            <repository>
                <id>nexus</id>
                <name>local private nexus</name>
                <url>http://maven.oschina.net/content/groups/public/</url>
                <releases>
                    <enabled>true</enabled>
                </releases>
                <snapshots>
                    <enabled>false</enabled>
                </snapshots>
            </repository>
        </repositories>
        
        <pluginRepositories>
            <pluginRepository>
            <id>nexus</id>
            <name>local private nexus</name>
            <url>http://maven.oschina.net/content/groups/public/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
            </pluginRepository>
        </pluginRepositories>
    </profile></profiles>
</settings>
```

### 属性(properties)

```xml
<properties>
    <spring.version>4.3.12.RELEASE</spring.version>
</properties>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
</dependency>
```



### 解析XML

#### dom4j

```xml
<!-- https://mvnrepository.com/artifact/dom4j/dom4j -->
<dependency>
    <groupId>dom4j</groupId>
    <artifactId>dom4j</artifactId>
    <version>1.6.1</version>
</dependency>
```

```java
try {
    SAXReader reader = new SAXReader();
    Document document = reader.read("./src/main/resources/" + path);
    Element root = document.getRootElement();
    Iterator<Element> iterator = root.elementIterator();
    while (iterator.hasNext()) {
        Element element = iterator.next();
        String id = element.attributeValue("id");
        String className = element.attributeValue("class");
        Class clazz = Class.forName(className);
        Constructor constructor = clazz.getConstructor();
        Object object = constructor.newInstance();

        Iterator<Element> beanIter = element.elementIterator();
        while (beanIter.hasNext()) {
            Element property = beanIter.next();
            String name = property.attributeValue("name");
            String valueStr = property.attributeValue("value");
            String methodName = "set" + name.substring(0, 1).toUpperCase() + name.substring(1);
            Field field = clazz.getDeclaredField(name);
            Method method = clazz.getMethod(methodName, field.getType());

            Object value = null;
            if (field.getType().getName() == "long") {
                value = Long.parseLong(valueStr);
            }
            if (field.getType().getName() == "java.lang.String") {
                value = valueStr;
            }
            if (field.getType().getName() == "int") {
                value = Integer.parseInt(valueStr);
            }

            method.invoke(object, value);
            ioc.put(id, object);
        }
    }
} catch (DocumentException e1) {
    e1.printStackTrace();
} catch (ClassNotFoundException e2) {
    e2.printStackTrace();
} catch (NoSuchMethodException e3) {
    e3.printStackTrace();
} catch (InstantiationException e4) {
    e4.printStackTrace();
} catch (IllegalAccessException e5) {
    e5.printStackTrace();
} catch (InvocationTargetException e6) {
    e6.printStackTrace();
} catch (NoSuchFieldException e7) {
    e7.printStackTrace();
}
```

### Spring Framework IoC 核心


#### spring-core

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>${spring.version}</version>
</dependency>
```
#### spring-context

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.1.RELEASE</version>
</dependency>
```

#### spring-beans

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>${spring.version}</version>
</dependency>
```

#### spring-expression

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>${spring.version}</version>
</dependency>
```

### Spring Framework AOP 核心

#### spring-aop

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.2.1.RELEASE</version>
</dependency>
```

#### spring-aspects

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.2.1.RELEASE</version>
</dependency>
```

##### aspectjweare

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.2</version>
</dependency>
```

### 自动生成getter和setter方法(lombok)

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.10</version>
    <scope>provided</scope>
</dependency>
```

### 单元测试(junit)

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

### MySQL驱动(connector)

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.44</version>
</dependency>
```

### IDEA的Maven是不会编译src的java目录的xml文件

```xml
 <build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
</build>
```



### 数据库连接池

#### DBCP

#### c3p0

```xml
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.2</version>
</dependency>
```

```xml
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.5.4</version>
</dependency>
```

```java
ComboPooledDataSource dataSource = new ComboPooledDataSource();
dataSource.setUser("root");
dataSource.setPassword("123456");
dataSource.setDriverClass("com.mysql.jdbc.Driver");
dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
```



#### Druid



### Log

#### 日志门面

- JCL（jakarta Commons logging）
- SLF4J（Simple Logging Facade for java）
- jboss-logging

#### 日志实现

- Log4j
- JUL（java.util.logging）
- Log4j2
- Logback

#### Spring 使用的日志框架

- Spring 使用的框架是 JCL 

- Spring Boot 使用的是 SLF4J + Logback

#### Legacy APIs

- JCL（jakarta Commons Logging) -> jcl-over-slf4j.jar
- log4j -> log4j-over-slfj.jar
- JUL（java.util.logging）-> jul-to-slf4j

### SLF4J

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>2.12.1</version>
</dependency>
```

### Log4j2

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.12.1</version>
</dependency>
```

### HttpServlet

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.0.1</version>
    <scope>provided</scope>
</dependency>
```

#### 设置java版本，以及字符编码设置

```properties
<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <encoding>UTF-8</encoding>
        <java.version>1.8</java.version>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```



