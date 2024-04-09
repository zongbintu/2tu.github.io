title: 如何生成requirements.txt
tags: [python]
date: 2024-03-27 19:32:46
categories: Other
---

写了一个简单的python项目，希望生成一个requirements.txt文件方便在Github或其他地方运行安装依赖。
作为新手我首先是一个一个从import里面去找到写到文件中的，结果发现居然会报错。
后面找到一个工具pipreqs可以自动识别出项目中用到的所有依赖生成requirements.txt文件
### How
#### Installation
```
pip install pipreqs
```
#### Usage
```
$ pipreqs /home/project/location
Successfully saved requirements file in /home/project/location/requirements.txt
```
生成requirements.txt内容如
```
wheel==0.23.0
Yarg==0.1.9
docopt==0.6.2
```

更详细的参数用法参照[pipreqs Usage](https://github.com/bndr/pipreqs?tab=readme-ov-file#usage)


