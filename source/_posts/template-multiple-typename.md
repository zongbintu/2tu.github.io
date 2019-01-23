title: C++模板多类型形参  
tags: [模板,多形参]
date: 2019-01-23 16:44:14
categories: C++  
---
C++模板与Java泛型有共通之处，为了参数化代码。所以也疑惑C++中的类模板是否与Java泛型相同只能指定一个类型？  
C++模板格式  
1、 函数模板格式  
```
template<typename T1, typename T2, ...> 返回类型 函数名(参数列表){
    ...
}
```
2、类模板格式  
```
template<typename T1, typename T2, ...> class 类名{
    ...
};
```  
由此可见，无论函数模板、类模板都支持多类型。如：
```
template<typename T1,typename T2> class MultipleTypename{
    private:
        T1 a;
        T2 b;
    public:
        void setA(T1 a);
        void setB(T2 b);
        T1 getA();
        T2 getB();
};
```

###### 转载请标明出处： 
###### http://2tu.github.io/
###### 本文出自Tu's blog