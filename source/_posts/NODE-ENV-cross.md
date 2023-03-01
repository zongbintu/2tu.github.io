title: NODE_ENV跨平台设置  
date: 2018-01-18 22:13:30  
categories: Web  
tags: [NODE_ENV,cross-env,spawn]
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
<font color="red">感谢[justjavac](https://github.com/justjavac)提供支持</font>  
###### &&为shell逻辑与运算符  
格式  
command1 && command2 [&& command3 ...]  
command1执行成功才会执行command2，任意命令执行失败则不会再执行后面的命令  
shell执行命令可以(;、&&、||）间隔
###### 正确使用 [cross-env vs cross-evn-shell](https://github.com/kentcdodds/cross-env#cross-env-vs-cross-env-shell)
cross-env
```
"build": "node ./tools/gulpfile.js&&cross-env NODE_ENV=__PROD__ node ./tools/webpack.config.js"
```

cross-env-shell
```
"build": "node ./tools/gulpfile.js&&cross-env-shell NODE_ENV=__PROD__ \"node ./tools/webpack.config.js && echo $NODE_ENV\""
```
\"\"包裹中的内容都可以有效取到cross-env-shell设置的环境变量，这里面的原理在于Node本身的跨平台机制[spawn](https://nodejs.org/api/child_process.html#child_process_child_process_spawn_command_args_options)

#### 参考   
[cross-env 不起作用怎么办？](https://segmentfault.com/q/1010000009324489)