title: VUE中使用Mock模拟数据请求  
tags: [Mock,mockjs,vue]
date: 2020-04-08 22:11:48
categories: Web
---

是
[Mock.js](https://github.com/nuysoft/Mock) 是一款模拟数据生成器，旨在帮助前端攻城师独立于后端进行开发，帮助编写单元测试。提供了以下模拟功能：

- 根据数据模板生成模拟数据
- 模拟 Ajax 请求，生成并返回模拟数据
- 基于 HTML 模板生成模拟数据

记录一下在项目中的使用方法
####  安装  

```
npm install mockjs
```

当然通常在项目中会在package.json中添加mockjs依赖，再npm install所有项目中需要依赖的库

```
"dependencies": {
    "mockjs": "^1.1.0"
  }
```

####  引用

src/mock/index.js
```
//引入mock
import Mock from 'mockjs'

let configArray = []
// 使用webpack的require.context()遍历所有mock文件
const files = require.context('.', true, /\.js$/)
files.keys().forEach((key) => {
  if (key === './index.js') return
  configArray = configArray.concat(files(key).default)
})

// 设置拦截ajax请求的相应时间
Mock.setup({
  timeout: '200-600'
});

// 注册所有的mock服务
configArray.forEach((item) => {
  for (const [path, target] of Object.entries(item)) {
    const protocol = path.split('|')
    Mock.mock(protocol[1], protocol[0], target)
  }
})
```

main.js中引入

```
//开发环境引入
if (process.env.NODE_ENV === 'development') {
  require('@/mock')
}
```

mock文件下建立personList.js  
```
let personList = [{
        id: 1,
        name: '小丽',
        age: '18',
        number: '8'
    },{
        id: 1,
        name: '小芳',
        age: '20',
        number: '6'
    }]

export default {
    'get|/parameter/query':  option => {
    return {
      status: 200,
      message: 'success',
      data: personList
    };
  }
}
```

接下来在网络请求时，发现如果是get请求路径是/parameter/query，就会返回我们mock定义好的数据。另外还有其它语法来生成随机数据。详细参考[数据模板定义 DTD](http://mockjs.com/0.1/#%E6%95%B0%E6%8D%AE%E6%A8%A1%E6%9D%BF%E5%AE%9A%E4%B9%89%20DTD)

#### 参考  
 [Mock.js](http://mockjs.com)  
 [Mock](https://github.com/nuysoft/Mock)

###### 转载请标明出处： 
###### http://blog.520wa.com/
###### 本文出自Tu's blog
