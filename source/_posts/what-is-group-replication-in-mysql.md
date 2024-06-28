title: MySQL MGR是什么？
tags: [MGR]
date: 2024-05-29 20:57:05
categories: Data
---
#### 术语
- MGR：MySQL Group Replication，即MySQL组复制
- RPO：Recovery Point Objective恢复点目标
- RTO：Recovery Time Objective恢复时间目标

#### 背景
公司核心应用A为公司核心应用，对数据库RPO、RTO都有较高要求。一次数据库主库磁盘expender背板坏了，影响磁盘IO通道，数据库层面出现大面积超时和错误。由于机器未完全坏掉，数据库可以连接，SQL可以执行，导致主从未能自动切换，手动切换时准备脚本，校验数据同步等问题耗时过长，导致公司业务出现重大损失。(原有方案为MHA高可用，半同步复制)
由此改为MGR集群技术方案，以期降低主从切换时效，降低损失。

#### MGR
MGR高可用方案中的RPO、RTO
RPO：架构模型保障了数据一致性，无需人为干预和检测
RTO：因其本身RPO的自动保障，无数据差异，准备耗时短，

MGR具备以下几个特点：

基于shared-nothing模式，所有节点都有一份完整数据，发生故障时可以直接切换。
MGR提供了数据一致性保障，默认是最终一致性，可根据业务特征需要自行调整一致性级别。
支持在线添加、删除节点，节点管理更方便。
支持故障自动检测及自动切换，发生故障时能自动切换到新的主节点，再配合MySQL Router中间件，应用层无需干预或调整。
支持单节点、多节点写入两种模式，可根据架构或业务需要选择哪种方案，不过强烈建议选用单主模式。


#### 参考
https://dev.mysql.com/doc/refman/8.4/en/group-replication.html

https://greatsql.cn/blog-10-9.html


