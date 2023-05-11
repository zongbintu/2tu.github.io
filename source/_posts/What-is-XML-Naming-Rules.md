title: XML元素名称规范
tags: [dom4j,XML]
date: 2023-05-11 19:07:45
categories: Java
---
#### 背景
使用dom4j生成XML，期望结果
生成XML数据
``` xml
<gNB间NG切换成功率(%)>100</gNB间NG切换成功率(%)>
```
但是抛错
``` java
java.lang.IllegalArgumentException: Illegal character in name: 'gNB间NG切换成功率(%)'.

	at org.dom4j.QName.validateName(QName.java:340)
	at org.dom4j.QName.<init>(QName.java:151)
	at org.dom4j.tree.QNameCache.createQName(QNameCache.java:245)
	at org.dom4j.tree.QNameCache.get(QNameCache.java:115)
	at org.dom4j.DocumentFactory.createQName(DocumentFactory.java:191)
	at org.dom4j.tree.AbstractElement.addElement(AbstractElement.java:760)
	at Dom4jTest.test_AddTagBySpecial(Dom4jTest.java:25)

```

<!-- more -->
#### 抛错的源码
查看方法at org.dom4j.QName.validateName(QName.java:340)
``` java
    private static final Pattern RE_NAME = Pattern.compile("[:_A-Za-zÀ-ÖØ-öø-˿Ͱ-ͽͿ-\u1fff\u200c-\u200d⁰-\u218fⰀ-\u2fef、-\ud7ff豈-\ufdcfﷰ-�][:_A-Za-zÀ-ÖØ-öø-˿Ͱ-ͽͿ-\u1fff\u200c-\u200d⁰-\u218fⰀ-\u2fef、-\ud7ff豈-\ufdcfﷰ-�-.0-9·̀-ͯ‿-⁀]*");

    private static void validateName(String name) {
        if (!RE_NAME.matcher(name).matches()) {
            throw new IllegalArgumentException(String.format("Illegal character in name: '%s'.", name));
        }
    }
```
dom4j为什么抛出这个错，是XML的规则就是这样还是dom4j版本不正确还是什么原因？
#### XML Naming Rules
参见[XML Elements](https://www.w3schools.com/xml/xml_elements.asp)
![XML Naming Rules](XMLNamingRules.png)