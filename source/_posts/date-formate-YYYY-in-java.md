title: 日期格式化 YYYY日期错位  
date: 2018-01-03 13:58:38  
categories: Java  
tags: [DateFormat,YYYY]
---
#### 起因  
支付系统通知账务还款结果时，对请求时间使用“YYYYMMddHHmmss”格式化，20171231变为了20181231致使数据不匹配。  

我们大多都知道mm与MM的区别，因为分和月会同时出现，但很少会关注其它的pattern大小写有无区分。  
<!--more-->

重现demo如下  

``` 
    Date date = new Date(2017 - 1900, 11, 31);

    DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
    System.out.println(dateFormat.format(date));

    dateFormat = new SimpleDateFormat("YYYY-MM-dd");
    System.out.println(dateFormat.format(date));
```  
输出结果：  
2017-12-31  
2018-12-31  
  
Date and Time Patterns  
 
| Letter | Date or Time Component | Presentation | Examples |
| -------- | ----- | ---- | ---- |
| G        | Era designator      | Text | AD |
| y |	Year |	Year |	1996; 96 |
| Y| 	Week year	| Year	| 2009; 09| 
| M	| Month in year (context sensitive)	| Month| 	July; Jul; 07| 
| L| 	Month in year (standalone form)	| Month| 	July; Jul; 07| 
| w| 	Week in year| 	Number| 	27| 
| W| 	Week in month| 	Number| 	2| 
| D| 	Day in year	| Number| 	189| 
| d	| Day in month| 	Number| 	10| 
| F	| Day of week in month| 	Number	| 2| 
| E| 	Day name in week	| Text	| Tuesday; Tue| 
| u| 	Day number of week (1 = Monday, ..., 7 = Sunday)	| Number	| 1| 
| a| 	Am/pm marker	| Text| 	PM| 
| H| 	Hour in day (0-23)| 	Number| 	0| 
| k	| Hour in day (1-24)| 	Number| 	24| 
| K| 	Hour in am/pm (0-11)	| Number| 	0| 
| h| 	Hour in am/pm (1-12)| 	Number	| 12| 
| m| 	Minute in hour| 	Number	| 30| 
| s	| Second in minute	| Number| 	55| 
| S	| Millisecond| 	Number| 	978| 
| z| 	Time zone	| General time zone| 	Pacific Standard Time; PST; GMT-08:00| 
| Z	| Time zone	| RFC 822 time zone| 	-0800
| X	| Time zone	| ISO 8601 time zone	| -08; -0800; -08:00| 

以上y为正常理解中的year，Y为week year，当天所在的周所属年份，一周从周日开始，周六结束。只要本周跨年，那么这周即算入下一年，2017-12-31为周日，刚好是2017最后一周并跨年。用YYYY获取自然得出2018，造成了这个错误。  
  
#### 参考  
[SimpleDateFormat by Java](https://docs.oracle.com/javase/9/docs/api/java/text/SimpleDateFormat.html)   
[Date Formatters by iOS](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1)  