title: 使用antd-init创建第一个可以运行的ant design
tags: [antd-init,create-umi]
date: 2019-09-12 14:28:30
categories: 前端
---
1、安装npm  

2、安装antd  
```
npm install antd-init -g
```

3、创建ant-demo  
```
mkdir antd-demo
```

4、进入antd-demo目录  
```
cd antd-demo
```

<!-- more -->

5、安装npm依赖  
```
antd-init --type plain-react
```
产生以下文件  
	node_modules
	index.css
	index.js
	package.json
	webpack.config.js

6、运行 (npm run build构建部署)  
```
npm start
```

7、根据listen端口进行访问  
```
           dora: listened on 8000

webpack: bundle build is now finished.

```

![Demo](/css/images/antd_demo_20190912150102.jpg)


8、最后  
---

antd-init@2 仅适用于学习和体验 antd，如果你要开发项目，推荐使用 create-umi 进行项目初始化。umi 是一个可插拔的企业级 React 前端应用框架，已在生产环境广泛应用。
antd-init@2 is only for experience antd. If you want to create projects, it's better to init with create-umi. umi is a pluggable enterprise-level react application framework.

Usage:

mkdir myapp && cd myapp
yarn create umi

Visit https://github.com/umijs/create-umi/ to learn more.


---  END  ---
分享程序员所看、所想、所悟、所望  