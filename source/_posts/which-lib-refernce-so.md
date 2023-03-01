title: 如何找出so依赖来源
tags: [so,有缺陷的OpenSSL版本,DebugNativeLibs]
date: 2023-03-01 22:13:56
categories: Android
---
### 问题
上架Google Play时提示引用了有缺陷的OpenSSL版本
![缺陷的库](/css/images/20230301_so_error_openssl.png)

### 解决思路
- 找出该so属于哪个库
- 升级或如何排除该so

### 找出so属于哪个库

在app下的build.gradle中添加如下代码
```shell
//列出所有包含有so文件的库信息
tasks.whenTaskAdded { task ->
    if (task.name.contains("DebugNativeLibs")) {
        task.doFirst {
            println("------------------- find so files start -------------------")

            it.inputs.files.each { file ->
                printDir(new File(file.absolutePath))
            }

            println("------------------- find so files end -------------------")
        }
    }
}

def printDir(File file) {
    if (file != null) {
        if (file.isDirectory()) {
            file.listFiles().each {
                printDir(it)
            }
        } else if (file.absolutePath.endsWith(".so")) {
            println "find so file: $file.absolutePath"
        }
    }
}
```
<!-- more -->
在build执行mergeFlavorDebugNativeLibs时即会打印出引用的所so库
![缺陷的库](/css/images/20230301_so_error_libavformat-58.png)
Github中搜索该库是否有修正新的OpenSSL版本
![wlmedia](/css/images/20230301_so_error_wlmedia.png)
发现该库最新版仍然引用错误的OpenSSL版本，且没有再维护。

### 排除三方库中的so
我们知道在packagingOptions中可以排除库以及同名库时选择采用哪个库
exclude、pickFirst、doNotStrip、merge
查看发现exclude并不能排除aar、lib中的so，所以只能看引用的库是否能升级，否则考虑是否引用该库。另外在保证功能正常使用的情况下，加入同名so，采用pickFirst的方式引用我们想引用的so
![packagingOptions](/css/images/20230301_so_error_packagingOptions.png)

### 参考
[gradle配置项packagingOptions使用说明](https://blog.csdn.net/liuzehn/article/details/103587571)
[Gradle 提示与诀窍](https://developer.android.google.cn/studio/build/gradle-tips?hl=zh-cn)
[【Gradle实战】利用Gradle查找项目里的so库来源](https://juejin.cn/post/6983931827480166414)
