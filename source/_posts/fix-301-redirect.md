title: 301重定向导致不能正常访问
date: 2017-12-07 23:04:55
tags: 301 302 Http Status Code
---
事件通报中称由于挂维护跳转301后，恢复服务，app WebView访问缓存仍然停留在公司官方网页，无法正确访问。  
参考[Http Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)  
究其原因可以知道默认情况下大家都必须遵守规范，才能不乱，并在这套规则中永久玩下去。  
如果遇到类似情况，直接分析请求头