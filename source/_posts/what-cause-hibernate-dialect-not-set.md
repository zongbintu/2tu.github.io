title: 服务器时间错误导致MySQL证书错误问题
tags: [Hibernate,MySQL]
date: 2023-06-24 16:16:41
categories: Data
---
### HibernateException 'hibernate.dialect' not set
![](hibernate-dialect-not-set.png)
同一个jar包，在某个环境下启动应用报错，另一个环境可以正常启动。报上面的错误
```java
caused by: org.hibernate.HibernateException: Access to DialectResolutionInfo cannot be null when 'hibernate.dialect' not set
```

### MySQL证书错误
查看MySQL日志mysql/mysqld.log其中有
```
2000-02-05T02:19:10.762462Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
2000-02-05T02:19:10.762482Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
```
![MySQL cert error](mysqld-log-cert.png)
<!-- more -->
**服务器安装时服务器时间为2000年，在这个时间下安装启动了MySQL，现在将服务器时间正常同步为2023年，服务器时间差异大，MySQL证书过期导致无法访问**

### 解决问题
1. 删除旧证书
2. 重启MySQL
![rm pem](rm-pem.png)
### 参考
[Creating SSL and RSA Certificates and Keys using MySQL](https://dev.mysql.com/doc/mysql-security-excerpt/5.7/en/creating-ssl-rsa-files-using-mysql.html)


