title: Makefile如何使用
tags: [Makefile]
date: 2024-02-06 19:54:21
categories: C/C++
---

工程化编译项目时，Java用Maven\Gradle，前端用npm，C/C++用Make
使用Make命令编译C/C++时，是通过Make工具实现。推荐使用[w64devkit](https://www.mingw-w64.org/downloads/#w64devkit)
w64devkit支持Linux命令

- 根据更改的源文件，自动确定需要更新哪些文件。它还自动确定更新文件的正确顺序，以防一个非源文件依赖于另一个非来源文件。
因此，如果您更改一些源文件，然后运行Make，则不需要重新编译所有程序。它只更新那些直接或间接依赖于您更改的源文件的非源文件。
如何确定依赖的呢，来源与我们在Makefile中指定的dependencies
- Make不限于任何特定的语言。所有能在命令行运行的编程语言都能处理（Java\Golang\Python...）。另外基于文件的改变然后更新另外的文件也可以。

Makefile中的一条规则告诉Make如何执行一系列命令，以便从源文件构建目标文件。它还指定了目标文件的依赖项列表。此列表应包括用作规则中命令输入的所有文件（无论是源文件还是其他目标文件）。

```
target:   dependencies ...
          commands
          ...
```

```
hello: hello.o
	g++ -o hello hello.o
hello.o: hello.cpp
	g++ -c hello.cpp
```

#### 更新机制
运行Make时，可以指定要更新的特定目标；否则，Make会更新makefile中列出的第一个目标。当然，必须首先更新生成这些目标所需的任何其他目标文件作为输入。
Make使用makefile来确定哪些目标文件应该更新，然后确定哪些文件实际上需要更新。如果目标文件比其所有依赖项都新，那么它已经是最新的，不需要重新生成。其他目标文件确实需要更新，但顺序正确：每个目标文件都必须重新生成，然后才能用于重新生成其他目标。
#### Makefile文件命名
Make自动查找makefile文件，顺序为GNUmakefile>makefile>Makefile
GUNmakefile:不建议使用，只能支持GUN make
makefile:所有版本都能识别
Makefile:推荐，最常用

运行make时没有找到上述文件会报错，但可以手动指定文件名
```
make -f <filename>
make --file=<filename>

```


