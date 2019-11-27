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



