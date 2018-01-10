title: module自动适应app中的flavors 
date: 2015-12-11 23:43:19
tags: [Android,module,flavor]
---
* **需求**  

app进行分层设计，将业务层剥离至module中  

* **问题**  
######  module自动适应app中的flavors  
######  打包时自动匹配  

* **解决办法**  
######  app与module使用相同的flavors配置  
######  module中build.gradle添加“publishNonDefault true”  
######  app中添加module flavor compile  
<!--more-->  
`localCompile project(path: ':mylibrary', configuration: 'localRelease')  `
`productionCompile project(path: ':mylibrary', configuration: 'productionRelease')  `             
`compile project(':mylibrary')   `

* 示例  
app [build.gradle](https://github.com/2tu/BuildVariationModuleSelection/blob/master/app/build.gradle)  
module [build.gradle](https://github.com/2tu/BuildVariationModuleSelection/blob/master/build.gradle)   
[工程](https://github.com/2tu/BuildVariationModuleSelection)

* **参考**  

http://stackoverflow.com/questions/24860659/multi-flavor-app-based-on-multi-flavor-library-in-android-gradle  
http://stackoverflow.com/questions/24307596/how-can-i-add-flavors-in-a-module-with-android-studio  
http://stackoverflow.com/questions/27519128/android-studio-build-variation-module-selection