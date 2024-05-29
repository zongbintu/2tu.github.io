title: 如何预估MySQL表空间占用大小
tags: []
date: 2024-05-29 20:37:39
categories: Data
---
我们经常需要知道表空间大小，在修改表结构的时候需要知道表空间大小以预估影响时间；在设计表的时候需要预估表的数据量，预估磁盘空间等。
#### 查看已存在的表空间大小
MySQL中information_schema有记录
``` SQL
SELECT concat(round(sum((data_length+index_length)/1024/1024),2),'MB' as data
FROM information_schema.tables
WHERE table_schema='mydb' and table_name='mytable';
```

#### 新建表空间大小测算
从上面可以看到表空间大小由数据大小+索引大小两部分组成
下面通过一个例子来实际测算一下
DDL
``` SQL
mysql> desc City;
+-------------+----------+------+-----+---------+----------------+
| Field       | Type     | Null | Key | Default | Extra          |
+-------------+----------+------+-----+---------+----------------+
| ID          | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name        | char(35) | NO   |     |         |                |
| CountryCode | char(3)  | NO   | MUL |         |                |
| District    | char(20) | NO   |     |         |                |
| Population  | int(11)  | NO   |     | 0       |                |
+-------------+----------+------+-----+---------+----------------+
```
- 数据大小测算
根据表结构中字段大小来测算
66 bytes per row of data（4+35+3+20+4）

- 索引大小测算
4 bytes per row for the primary key
7 bytes per row for country code index
 - 3 bytes for the country
 - 4 bytes for Clustered Key attached to the country code

这不考虑BTREE或表空间碎片的内务管理
对于一百万行，这将是77000000字节（73.43 MB）

参考
[How to estimate/predict data size and index size of a table in MySQL](https://dba.stackexchange.com/questions/46069/how-to-estimate-predict-data-size-and-index-size-of-a-table-in-mysql)

更详细的还需要看[InnoDB : Tablespace Space Management](https://dev.mysql.com/blog-archive/innodb-tablespace-space-management/)


