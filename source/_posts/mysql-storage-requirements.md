title: MySQL数据类型字段占用空间
tags: [MySQL]
date: 2023-09-03 11:41:25

categories: Data
---

经常看到这种建表sql

``` sql
CREATE TABLE `table1` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) DEFAULT NULL,
  `status` int(2) DEFAULT '0',
  `fileProtocol` tinyint(1) NOT NULL DEFAULT '0' ,
  PRIMARY KEY (`id`)
) ;
```

sql中为每个字段都加上了长度限制，能限制住吗？存储数据时占用的空间又是多少呢？

具体见[Data Type Storage Requirements](https://dev.mysql.com/doc/refman/5.7/en/storage-requirements.html)

