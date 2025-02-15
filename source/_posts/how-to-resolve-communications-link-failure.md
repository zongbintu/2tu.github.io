title: 如何排查解决Communications link failure
tags: [druid,MySQL,CJCommunicationsException]
date: 2025-02-14 16:43:11
categories: Data
---

- 项目启动运行几小时后数据库连接异常，重启正常使用
- 偶发数据库连接异常，重启正常使用
- 偶发事物数据丢失

怎么办，网上搜索，druid issue搜索，一会儿让改一下这个参数，一会儿改一下那个参数。各种改了重启，观察。然后报错依然复现存在。没想到我们也有资格成为调参工程师。

### 环境
- MySQL：8.0.32
- Druid: 1.1.24
- Driver：mysql-connector 8.0.27

### 错误异常
通常错误有以下几种
```
Caused by: java.net.SocketException: Connection reset

Caused by: com.mysql.cj.exceptions.CJCommunicationsException: Communications link failure
The last packet successfully received from the server was 1,254,739 milliseconds ago. The last packet sent successfully to the server was 1,254,740 millisecond ago.s ago.

Caused by: java.sql.SQLException: connection disabled
at com.alibaba.druid.pool.DruidPooledConnection.checkStateInternal(DruidPooledConnection.java:1169)

org.springframework.jdbc.UncategorizedSQLException:
### Error querying database. Cause: java.sql.SQLException: connection disabled

Cause: java.sql.SQLException: connection disabled
; uncategorized SQLException for SQL []; SQL state [null]; error code [0]; connection disabled; nested exception is java.sql.SQLException: connection disabled

2025-02-11 04:46:31.300 ERROR 1 --- [http-nio-8080-exec-7] [c.a.druid.pool.DruidPooledStatement :368] : CommunicationsException, druid version 1.1.24, jdbcUrl : jdbc:mysql://host:3306/db?useUnicode=true&characterEncoding=UTF8&zroDateTimeBehavior=convertToNull&autoReconnect=true&failOverReadOnly=false&maxReconnects=10&useSSL=false&connectTimeout=10000&socketTimeout=120000, testWhileIle true, idle millis 6403958, minIdle 5, poolingCount 2, timeBetweenEvictionRunsMillis 60000, lastValidIdleMillis 6403958, driver com.mysql.cj.jdbc.Driver, exeptionSorter com.alibaba.druid.pool.vendor.MySqlExceptionSorter
2025-02-11 04:46:31.302 ERROR 1 --- [http-nio-8080-exec-7] [com.alibaba.druid.util.JdbcUtils :96] : close connection error

java.sql.SQLNonTransientConnectionException: Communications link failure during rollback(). Transaction resolution unknown.
```

### 排查步骤
以上所有的错误都是指向连接异常。需要我们抽丝剥茧。杀人是个罪恶的行径，查出真相是我的责任。
- 网络问题
作为程序员首先想到的不是程序有问题出，而是环境有问题。是否有防火墙杀死应用到数据库的连接

- 数据库超时时间设置问题
查看数据库设置超时时间是否和druid配置不匹配
查看数据库设置
```
show variables like '%timeout%';
```

![MySQL timeout](mysql_timeout.jpeg)
其中wait_timeout和interactive_timeout是设置MySQL在连接不操作多少秒后断开连接。  
如：客户端连接到MySQL超过8小时没有任何操作，则MySQL将主动断开连接。其中wait_timeout为非交互的连接，我们应用程序连接使用的就是这个。  
如果wait_timeout过小和druid配置连接检查间隔时间不合理，则有可能出现d以为该连接没关闭，但是MySQL已经主动关闭连接，导致连接异常的问题。但是这种情况较少，因为我们校验连接通常都是多少秒校验一次。  
另外MySQL参数中也有事务时间，应用程序有长事务，导致超过MySQL设置事务时间，会导致事务不成功  

另外我们可以通过查询MySQL的连接来等待判断时间
```
select * from information_schema.processlist where db = 'mydb'
```
![MySQL processlist](mysql_processlist.jpeg)


- druid配置不合理
外部原因排查完后，就只剩下排查自己问题了。不对，也可以是druid提示不明显。  
我们通常将连接池托管给三方库，我们只需要按其提供建议配置就好。druid就是其中一种。各版本配置有些许变化，
```
shardingsphere:
props:
sql-show: true
query-with-cipher-column: false
datasource:
names: salve,master
master:
type: com.alibaba.druid.pool.DruidDataSource
driverClassName: com.mysql.jdbc.Driver
url: jdbc:mysql://${db.host}:${db.port}/${db.database}?useUnicode=true&characterEncoding=UTF8&zeroDateTimeBehavior=convertToNull&autoReconnect=true&failOverReadOnly=false&maxReconnects=10&useSSL=false&connectTimeout=10000&socketTimeout=60000
username: ${db.username}
password: ${db.password}
maxActive: ${db.maxActive:100}
minIdle: ${db.minIdle:5}
initialSize: ${db.minIdle:5}
minEvictableIdleTimeMillis: ${db.minEvictableIdleTimeMillis:180000}
maxEvictableIdleTimeMillis: ${db.maxEvictableIdleTimeMillis:300000}
maxWait: ${db.maxWait:10000}
maxWaitThreadCount: ${db.maxWaitThreadCount:1000}
testOnBorrow: false
testOnReturn: false
testWhileIdle: true
keepAlive: true
timeBetweenEvictionRunsMillis: 20000
validationQueryTimeout: 10
validationQuery: SELECT 1
## TRANSACTION_READ_COMMITTED
default-transaction-isolation: 2
```

加入配置druid.mysql.usePingMethod=false
在检查有效连接的时候才不会使用ping而使用validationQuery sql来检查
另外可以查看[DruidDataSource配置属性列表](https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8)

- 事务未正确关闭
在排除了非程序问题，非配置问题后，还没有找到原因。就要想是不是druid的源码问题，还是MySQL源码问题。或者可能是自己程序哪里有问题。在看了一阵druid的源码和issue后发现有跟程序未正确结束事务（commit or rollback），会导致这个事务持有数据库连接无法正常归还到连接池，从而导致连接池中可用连接变少。
我的问题就是这个，修改后，解决了这个连接异常问题
[issue](https://github.com/alibaba/druid/issues/5889)