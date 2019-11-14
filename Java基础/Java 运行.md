#### Java 语言有哪些特点

- 面向对象

- 跨平台

- 有大量的 API 扩展和开源项目

- 多线程支持
####  Java 的跨平台实现的原理是什么？

- 首先通过编译器把 .java 源码编译成 .class 的字节码，通过 JVM 运行 java 程序。
- JVM 根据不同的平台把 .class 的字节码编译成相对应平台的机器码。

#### JDK、JRE、JVM 有哪些区别？

- JDK, Java Development Kit 的简称，开发环境和运行环境。
- JRE, Java Runtime Environment Java运行环境。
- JVM, Java Virtual Machine,Java 程序运行的载体。

#### Java中如何获取明天此刻的时间？

```java
Calendar calendar = Calendar.getInstance();
calendar.add(Calendar.DATE,1);

System.out.println(calendar.getTime());
```

```java
LocalDateTime today = LocalDateTime。now();
LocalDateTime tomorrow = today.plusDays(1);

System.out.pringln(tomorrow);
```

#### Java中如果跳出多重嵌套循环？

```java
//定义一个标号，使用break加标号的方式
myfor:
for (int i = 0; i < 100; i++) {
    for (int j = 0; j < 100; j++) {
        System.out.print(",j:" + j);
        if (j == 10) {
            break myfor;
        }
    }
}
```

```java
//使用全局变量
boolean flag = true;
for (int i = 0; i < 100 && flag; i++) {
    for (int j = 0; j < 100 && flag; j++) {
        System.out.print(",j=" + j);
        if (j == 10) {
            flag = false;
        }
    }
}
```

#### Java中，char变量能不能存储一个中文汉字？为什么？

- 可以
- Java默认编码是Unicode,一个char类型占2个字节。

#### Java中会存在内存泄漏码？做简单描述。

- 一个不再使用的对象或变量一直没有被回收。就造成了内存泄漏。
  - 长生命周期对象持有段生命周期的对象。
  - 各种连接未调用关闭方法。只要有关闭方法，就一定要调用
  - 改变已经存入HashSet集合中的对象里面参与Hash计算的字段的值。

  