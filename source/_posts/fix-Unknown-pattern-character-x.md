title: 修复X转换Time Zone出错问题
tags: [SimpleDateFormat,Time Zone]
date: 2019-05-16 18:44:54
categories: Android
---
IllegalArgumentException Unknown pattern character 'x', when using SimpleDateFormat
   
代码如下：
```
        try {
          new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssX").parse("2019-05-14T19:00:11+08:00");
        } catch (ParseException e) {
          e.printStackTrace();
        }
```
在Android 8.0运行没问题，但在5.1及以下会抛“IllegalArgumentException Unknown pattern character 'x', when using SimpleDateFormat”，如此看来基本可以认为是版本兼容问题

<!-- more -->

首先我们查找文档对于格式化定义，之前文章[日期格式化YYYY日期错位](/2018/01/03/date-formate-YYYY-in-java/)中有提到Java中Date and Time Patterns，下面我们再看看Android中java.txt.SimpleDateFormat中的
Date and Time Patterns
  
Date and Time Patterns  
 
| Symbol| Meaning | Kind | Example |
| -------- | ----- | ---- | ---- |
| D| 	Day in year	| Number| 	189| 
| E| 	Day of week	| Text	| E/EE/EEE:Tue,EEEE:Tuesday,EEEEE:T| 
| F	| Day of week in month| 	Number	| 2(2nd Wed inJuly)|
| G        | Era designator      | Text | AD |
| H| 	Hour in day (0-23)| 	Number| 	0| 
| K| 	Hour in am/pm (0-11)	| Number| 	0| 
| L| 	stand-alone month	| Text | 	L:1 LL:01 LLL:Jan LLLL:January LLLLL:J| 
| M	| Month in year	| Text| 	M:1 MM:01 MMM:Jan MMMM:January MMMMM:J| 
| S	| fractional seconds| 	Number| 	978| 
| W| 	Week in month| 	Number| 	2| 
| Z	| time zone (RFC 822)	| time zone| 	Z/ZZ/ZZZ:-0800 ZZZZ:GMT-08:00 ZZZZZ:-08:00|
| a| 	Am/pm marker	| Text| 	PM| 
| c| 	stand-alone day of week	| Text| 	c/cc/ccc:Tue, cccc:Tuesday, ccccc:T| 
| d	| Day in month| 	Number| 	10|  
| h| 	Hour in am/pm (1-12)| 	Number	| 12| 
| k	| Hour in day (1-24)| 	Number| 	24| 
| m| 	Minute in hour| 	Number	| 30| 
| s	| Second in minute	| Number| 	55| 
| w| 	Week in year| 	Number| 	27| 
| y |	Year |	Number|	yy:10 y/yyy/yyyy:2010 |
| z| 	time zone	| Time Zone	| z/zz/zzz:PST zzzz:Pacific Standard Time| 
| '| 	escape for text	| Delimiter	| 'Date=':Date=| 
| ' '| 	single quote	| Literal	| 'o''clock':o'clock| 

从中我们看到X和Z都是time zone，改成Z即可。
Android中的SimpleDateFormat也是使用Java的api
Android 5.0开始采用了JDK 7，对应应该已经有X了，见[SimpleDateFormat](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html)
但是我们从Android 22的源码中可以看到
```
public class SimpleDateFormat extends DateFormat {

    private static final long serialVersionUID = 4774881970558875024L;

    // 'L' and 'c' are ICU-compatible extensions for stand-alone month and stand-alone weekday.
    static final String PATTERN_CHARS = "GyMdkHmsSEDFwWahKzZLc";

    // The index of 'Z' in the PATTERN_CHARS string. This pattern character is supported by the RI,
    // but has no corresponding public constant.
    private static final int RFC_822_TIMEZONE_FIELD = 18;

    // The index of 'L' (cf. 'M') in the PATTERN_CHARS string. This is an ICU-compatible extension
    // necessary for correct localization in various languages (http://b/2633414).
    private static final int STAND_ALONE_MONTH_FIELD = 19;
    // The index of 'c' (cf. 'E') in the PATTERN_CHARS string. This is an ICU-compatible extension
    // necessary for correct localization in various languages (http://b/2633414).
    private static final int STAND_ALONE_DAY_OF_WEEK_FIELD = 20;

    private String pattern;

    private DateFormatSymbols formatData;
```

Android 28
```
public class DateFormatSymbols implements Serializable, Cloneable {
...
    /**
     * Unlocalized date-time pattern characters. For example: 'y', 'd', etc.
     * All locales use the same these unlocalized pattern characters.
     */
    // Android-changed: Add 'c' (standalone day of week), 'b' (day period),
    //   'B' (flexible day period)
    static final String  patternChars = "GyMdkHmsSEDFwWahKzZYuXLcbB";
```

重点：
static final String PATTERN_CHARS = "GyMdkHmsSEDFwWahKzZLc";  
static final String  patternChars = "GyMdkHmsSEDFwWahKzZYuXLcbB";

源码里可以看到导致问题原因，但是为何Android 22中的java代码与Java 7中不一致呢？



---  END  ---
分享程序员所看、所想、所悟、所望