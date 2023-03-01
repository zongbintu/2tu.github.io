title: 头文件和命名空间是否必须同时引用？    
tags: [头文件,命名空间]  
date: 2019-01-11 17:27:48   
categories: Other
---
c++中开始的两行一般为
```
#include <iostream>
using namespace std;
```
但也可以使用std::cin方式显示指定namespace。那么不使用"using namespace std;"是否可以直接采用std::cin调用呢？试验后却发现会报错。    

头文件iostream中定义了std namespace，如果不用using namespace std，则cin cout不可见？目前未搞懂，先记录  
<!-- more -->
iostream源码如下：
```
// iostream standard header for Microsoft
#pragma once
#ifndef _IOSTREAM_
#define _IOSTREAM_
#ifndef RC_INVOKED
#include <istream>

 #pragma pack(push,_CRT_PACKING)
 #pragma warning(push,3)
 #pragma push_macro("new")
 #undef new
_STD_BEGIN
  #ifdef _M_CEE_PURE
__PURE_APPDOMAIN_GLOBAL extern istream cin, *_Ptr_cin;
__PURE_APPDOMAIN_GLOBAL extern ostream cout, *_Ptr_cout;
__PURE_APPDOMAIN_GLOBAL extern ostream cerr, *_Ptr_cerr;
__PURE_APPDOMAIN_GLOBAL extern ostream clog, *_Ptr_clog;

__PURE_APPDOMAIN_GLOBAL extern wistream wcin, *_Ptr_wcin;
__PURE_APPDOMAIN_GLOBAL extern wostream wcout, *_Ptr_wcout;
__PURE_APPDOMAIN_GLOBAL extern wostream wcerr, *_Ptr_wcerr;
__PURE_APPDOMAIN_GLOBAL extern wostream wclog, *_Ptr_wclog;

  #else /* _M_CEE_PURE */
		// OBJECTS
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 istream cin, *_Ptr_cin;
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 ostream cout, *_Ptr_cout;
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 ostream cerr, *_Ptr_cerr;
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 ostream clog, *_Ptr_clog;

__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 wistream wcin, *_Ptr_wcin;
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 wostream wcout, *_Ptr_wcout;
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 wostream wcerr, *_Ptr_wcerr;
__PURE_APPDOMAIN_GLOBAL extern _CRTDATA2 wostream wclog, *_Ptr_wclog;

		// CLASS _Winit
class _CRTIMP2_PURE _Winit {
public:
	__thiscall _Winit();
	__thiscall ~_Winit() _NOEXCEPT;
private:
	__PURE_APPDOMAIN_GLOBAL static int _Init_cnt;
	};
  #endif /* _M_CEE_PURE */
_STD_END
 #pragma pop_macro("new")
 #pragma warning(pop)
 #pragma pack(pop)
#endif /* RC_INVOKED */
#endif /* _IOSTREAM_ */

/*
 * Copyright (c) by P.J. Plauger. All rights reserved.
 * Consult your license regarding permissions and restrictions.
V6.50:0009 */

```

### 参考
[[C++]头文件(Header Files)和命名空间(Namespace)](https://blog.csdn.net/libing_zeng/article/details/81032978)