title: 如何主动PUSH SDK更新给开发者
date: 2017-12-01 10:19:47  
categories: 其它  
tags: [如何主动PUSH,SDK更新给开发者,SDK,主动push,github api,fit]
---

之前苹果禁用jspatch时，涉及到的SDK全部需要更新。此时开发者得全面排查所使用SDK是否需要更新。怎么查？一个一个去SDK官网查看是否有更新解决方案。啰嗦，实现上就是想维护Github上面可怜的Star，所以想利用Github的开放api更新了SDK给他们发个邮件  

1、查询Star的api  
文档：https://developer.github.com/v3/activity/starring/  
调用：https://api.github.com/repos/2tu/fit/stargazers  
发现只有user，没有其邮箱  

2、查询用户信息  
文档：https://developer.github.com/v3/users/  
调用：https://api.github.com/users/2tu  
发现并没有想要的email，提示如下：  
Note: The returned email is the user's publicly visible email address (or null if the user has not specified a public email address in their profile). The publicly visible email address only displays for authenticated API users.  
只能显示用户自己的email，虽然可以通过爬虫直接爬取，但是不正当，该想法搁置