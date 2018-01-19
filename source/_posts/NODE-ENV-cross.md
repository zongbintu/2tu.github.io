title: NODE_ENV跨平台设置  
date: 2018-01-18 22:13:30
tags: [Node,NODE_ENV,cross-env]
---
##### set NODE_ENV问题
最近接手系统中采用React作为管理端，其中有这样一句

```
"build": "node ./tools/gulpfile.js&&set NODE_ENV=__PROD__&&node ./tools/webpack.config.js"
```

其中“set NODE_ENV=__PROD__”却未在我的Mac下生效，其他成员使用Windows。  
原因是Windows下使用set NODE_ENV=xx，Unix下应该为export NODE_ENV=xx，于是不得不区分操作系统进行设置，于是就有了[cross-env](https://github.com/kentcdodds/cross-env)这个方案。  
<!--more-->
##### 使用cross-env解决跨平台
修改命令为

```
"build": "node ./tools/gulpfile.js&&cross-env NODE_ENV=__PROD__&&node ./tools/webpack.config.js"
```

执行后，结果还是TMD没生效。查看cross-env的Usage后，区别在于我使用了&&分隔，在后面的webpack.config.js中获取前面设置的NODE_ENV，问题是否出在这里呢？

##### &&与空格问题  
cross-env模块下，划分出前后两个环境，后一句的环境没有设置到NODE_ENV变量，值为undefined。  
所以最后的正确的代码是

```
"build": "node ./tools/gulpfile.js&&cross-env NODE_ENV=__PROD__ node ./tools/webpack.config.js"
```


#### 参考   
[cross-env 不起作用怎么办？](https://segmentfault.com/q/1010000009324489)  

###### 转载请标明出处： 
###### http://2tu.github.io/2018/01/18/NODE-ENV-cross/ 
###### 本文出自Tu's blog