title: 自定义ShardingSphere的JSON加解密器
tags: [ShardingSphere]
date: 2024-05-28 21:32:54
categories: Data
---
基于数据安全的目的，需要对敏感数据进行加密存储。其中有整个字段是敏感内容的数据，也有部分灵活内容存储为JSON，JSON中的部分path为敏感内容。
针对这部分内容，需要对JSON指定path加解密，以避免对整个JSON加解密造成存储空间、应用缓存资源浪费。
#### ShardingSphere整体架构
![](architecture.png)
#### 加密规则
加密配置主要分为四部分：数据源配置，加密算法配置，加密表配置以及查询属性配置，其详情如下图所示：
![](rule.png)
JSON加解密器实现在加密配置->用户自定义处

#### 配置
##### 加密器类型配置
``` xml
spring:
  shardingsphere:
    rules:
      encrypt:
      	encryptors:
          json_encryptor:
            type: json
```

##### 加密字段配置
``` xml
spring:
  shardingsphere:
    rules:
      encrypt:
      	tables:
          t_user:
            columns:
              info:
                plainColumn: info
                cipherColumn: info_cipher
                encryptorName: json_encryptor
```

##### JSON path加密配置
``` xml
spring:
  shardingsphere:
    rules:
      encrypt:
      	encryptors:
          json_encryptor:
            props:
              column0_path: bankCard <!-- {唯一名字}_path=需要加密的json路径   -->
              column0_path_encryptor: bankCard <!-- {唯一名字}_path_encryptor=加密类型 -->
```

#### 自定义加密
接下来就是自定义ShardingSphere加密部分
可以查看ShardingSphere官网，[EncryptAlgorithm](https://shardingsphere.apache.org/document/1.3.0/cn/reference/encrypt/#encryptalgorithm)
继承EncryptAlgorithm
- 重写getType为json（加密器类型配置处使用）
- 重写setProps（JSON path加密配置会从此处拿到）
- 重写encrypt、decrypt，在遇到加密字段配置中的SQL时，触发改写SQL，可以根据props中配置的json path自定义加密。

实现SPI
在代码的resources路径下创建META-INF\services\org.apache.shardingsphere.spi.encrypt.ShardingEncryptor
文件内容为自定义加密类的全路径，如：com.company.shardingsphere.encrypt.JSONEncryptor



