title: SPI机制
tags: [SPI]
date: 2024-04-08 20:38:27
categories: Java
---
SPI（Service Provider Interface），是JDK内置的一种 服务提供发现机制，可以用来启用框架扩展和替换组件，主要是被框架的开发人员使用，比如java.sql.Driver接口，其他不同厂商可以针对同一接口做出不同的实现，MySQL和PostgreSQL都有不同的实现提供给用户，而Java的SPI机制可以为某个接口寻找服务实现。Java中SPI机制主要思想是将装配的控制权移到程序之外，在模块化设计中这个机制尤其重要，其核心思想就是 **解耦**。
SPI整体机制图如下：
![SPI](spi.jpg)

服务提供方提供了接口实现后，需要在classpath下的META-INF/services/目录下创建以服务接口命名的文件，文件内容为接口的实现类名。
其他程序使用服务时，会通过查找这个jar的META-INF/services/中文件，获取实现类名，进行加载实例化，该服务就可以使用了。JDK中查找服务实现的类为java.util.ServiceLoader。
#### 应用-JDBC
##### JDBC接口定义
在java中定义了接口java.sql.Driver，并没有实现，具体实现由不通厂商实现。
##### MySQL实现
MySQL的jar包（mysql-connector-java-8.0.30.jar）中，META-INF/services目录下有文件名java.sql.Driver的内容为com.mysql.cj.jdbc.Driver
![SPI META-INF/services](spi-services.png)
##### 使用方法
```java
Connection conn = DriverManager.getConnection(url,username,password);
```
##### SPI如何实现
在使用的时候并没有指定使用哪个Driver来连接，那如何使用上MySQL的驱动的呢？这就是我们SPI在起作用。
```java
package java.sql;
public class DriverManager {

    static {
        loadInitialDrivers();
        println("JDBC DriverManager initialized");
    }
    
    private static void loadInitialDrivers() {
        String drivers;
        try {
            drivers = AccessController.doPrivileged(new PrivilegedAction<String>() {
                public String run() {
                    return System.getProperty("jdbc.drivers");
                }
            });
        } catch (Exception ex) {
            drivers = null;
        }
       
        AccessController.doPrivileged(new PrivilegedAction<Void>() {
            public Void run() {

                ServiceLoader<Driver> loadedDrivers = ServiceLoader.load(Driver.class);
                Iterator<Driver> driversIterator = loadedDrivers.iterator();

                try{
                    while(driversIterator.hasNext()) {
                        driversIterator.next();
                    }
                } catch(Throwable t) {
                }
                return null;
            }
        });

        println("DriverManager.initialize: jdbc.drivers = " + drivers);

        if (drivers == null || drivers.equals("")) {
            return;
        }
        String[] driversList = drivers.split(":");
        println("number of Drivers:" + driversList.length);
        for (String aDriver : driversList) {
            try {
                println("DriverManager.Initialize: loading " + aDriver);
                Class.forName(aDriver, true,
                        ClassLoader.getSystemClassLoader());
            } catch (Exception ex) {
                println("DriverManager.Initialize: load failed: " + ex);
            }
        }
    }
}
```
其中
ServiceLoader<Driver> loadedDrivers = ServiceLoader.load(Driver.class);
Iterator<Driver> driversIterator = loadedDrivers.iterator();
负责查找classpath下及jar包中META-INF/services目录下java.sql.Driver文件中的内容获取具体实现。

### SPI机制的缺陷
通过上面的解析，可以发现，我们使用SPI机制的缺陷：
- 不能按需加载，需要遍历所有的实现，并实例化，然后在循环中才能找到我们需要的实现。如果不想用某些实现类，或者某些类实例化很耗时，它也被载入并实例化了，这就造成了浪费。
- 获取某个实现类的方式不够灵活，只能通过 Iterator 形式获取，不能根据某个参数来获取对应的实现类。
- 多个并发多线程使用 ServiceLoader 类的实例是不安全的.


