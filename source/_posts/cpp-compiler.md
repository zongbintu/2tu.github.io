title: C++编译器有哪些
tags: [Compiler]
date: 2023-10-21 09:57:53
categories: C/C++
---
没想到C++环境搭建都这么麻烦，编译器有很多种。
- [MSVC](https://code.visualstudio.com/docs/cpp/config-msvc)
- [w64devkit](https://github.com/skeeto/w64devkit)
- Clang

没有研究是觉得有些复杂。

在编译时会将我们include，#define等合并成一个文件。那如何看到这个编译的中间文件呢？
在Visual Studio中设置
![Preprocess to a File](preprocess-to-a-file.png)


代码
``` C++
#include <iostream>

int main() {
	std::cout << "Hello World!" << std::endl;
	std::cin.get();
}
```

Ctrl + F7编译后，可以从Debug目录下看到FileName.i文件中已经包含了include的 iostream代码



**注意：Visual Studio和Visual Studio Code是不同的工具**

下一个看看C++如何建立link，为什么C++中不像java一样需要导入包，就可以link，然后引用呢？如果有两个同名的函数，那link的时候会报错吗？
