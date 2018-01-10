title: use RestTemplate in Spring
date: 2017-03-13 15:48:08
tags: [Java,RestTemplate,Spring,HttpClient]
---
Spring中提供RestTemplate方便访问Web服务，不再需要使用HttpClient、HttpComponents等
### 差异
使用方式详细见[api](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html)，唯一注意区别参数uriVariables，遵循RESTful风格为uri变量，如：https://api.github.com/users/2tu
可以写成
```java  
restTemplate.getForObject("https://api.github.com/users/{userName}", String.class, "2tu");  
Map<String, String> uriVariables = Collections.singletonMap("userName", "2tu"); 
restTemplate.getForObject("https://api.github.com/users/{userName}", String.class, uriVariables);
```  

<font color=red>注：SpringBoot默认采用jackson</font>

参考：  
[Consuming a RESTful Web Service](https://spring.io/guides/gs/consuming-rest/)  
[RestTemplate API](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html)