title: 符合国际标准的城市编码
categories: Development
tags: []
date: 2023-08-04 07:53:00
---





### 问题

fork的项目[running_page](https://github.com/yihong0618/running_page)有一个[Issue](https://github.com/yihong0618/running_page/issues/292)提到国家和地区识别不正确，看了数据样例较为复杂，有的是有国家省市区，有的只有部分，还有国外的。running_page中代码以逗号分隔取国家，然后根据[统计用区划代码和城乡划分代码](http://www.stats.gov.cn/sj/tjbz/qhdm/)对比取出城市。

问题是，数据是全球的用户是全球的，那全球各个国家和区域的标准应该是什么？中国有省市区，那其他国家有吗？有统一标准吗？

### 国际标准 ISO 3166

全球化那就必然要找国际标准了，看这里[ISO 3166](https://en.wikipedia.org/wiki/ISO_3166), ***Codes for the representation of names of countries and their subdivisions***

里面又包含了

- **[ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1)** 国家代码
- **[ISO 3166-2](https://en.wikipedia.org/wiki/ISO_3166-2)** 国家分支机构代码
- **[ISO 3166-3](https://en.wikipedia.org/wiki/ISO_3166-3)** 国家曾用名代码

### 如何使用

我们拿到gps后做需要逆地址解析拿到ISO 3166标准的代码，有哪些方式可以呢，google map？

我调研了2个方式

<!-- more -->

#### GeoNames

The GeoNames geographical database covers all countries and contains over eleven million placenames that are available for download free of charge.

免费全球地理数据库

注册账号，每个账号有免费调用接口额度，对于个人来说完全够了，接口不同占用额度不同，具体如下

[Terms and Conditions](https://www.geonames.org/export/)

![Terms and Conditions](terms-and-conditions.png)

[GeoNames Webservice Credits](https://www.geonames.org/export/credits.html)

![GeoNames Webservice Credits](webservice-credits.png)

#### Nominatim 

[Nominatim](https://nominatim.org/release-docs/develop/)

Nominatim 是一个开源的地理数据库，可通过 REST API 访问。它由 OpenStreetMap 社区维护，包含来自各种来源的数据，包括 OpenStreetMap 数据库、维基百科和其他开放数据集。

Nominatim 解析的地址是使用国际标准化组织 (ISO) 3166 标准。ISO 3166 标准定义了世界各国的名称和代码。Nominatim 还使用其他标准，如 ISO 6709 标准，该标准定义了地理坐标的格式。



有python库Nominatim

### 参考

[对ISO3166《国家和所属地区名称代码》国际标准的几点认识](https://mp.weixin.qq.com/s?__biz=MzIzMjU3MjUyMg==&mid=2247484595&idx=4&sn=0cbe29b0051ac460b3bb7924dba62395&chksm=e89397b6dfe41ea03322c234d5745812f144968e74034fc229b550bab65a1755a1fba446629b&scene=27)
