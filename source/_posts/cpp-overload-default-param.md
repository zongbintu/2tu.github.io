title: C++重载默认参数函数问题？    
tags: [Override]  
date: 2019-01-18 15:15:27   
categories: C/C++
---
函数重载以参数类型和参数个数不同区分。如:  
```
int add(int m1, int m2);
int add(int m1, int m2, int m3);
int add(int m1, int m2, int m3, int m4);
int add(char m1, char m2);
```
以上程序大家都没有什么疑惑。那么下面的程序呢？
<!-- more -->
```
int add(int m1, int m2);
int add(int m1, int m2, int m3);
int add(int m1, int m2, int m3, int m4 = 0);
int add(char m1, char m2);
```
看着也无大问题，但是编译却会报错  
![](overload_argument.png)  
原因在于C++中有了默认参数后4个int参数的方法相当于  
```
int add(int m1, int m2, int m3, int m4 = 0);
int add(int m1, int m2, int m3){
    int m4(0);
}
```
所以再定义一个int (int m1, int m2, int m3)已经造成了歧义。
由此设置默认参数需要注意2点  
1. 只能由后往前  
2. 不能对少于参数个数的函数进行重载