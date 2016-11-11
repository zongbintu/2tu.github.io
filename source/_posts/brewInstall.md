title: brew install mac安装失败的问题  
date: 2015-10-16 17:18:42
tags:brew install Could not resolve host: github.com
---
* 问题：brew 安装失败
* 思路：将github仓库放到本地，不用ruby下载
* 解决办法：

1、下载[https://raw.githubusercontent.com/Homebrew/install/master/install](https://raw.githubusercontent.com/Homebrew/install/master/install)文件(使用浏览器打开，另存为也可以)，命名为install  

2、在github上把Homebrew(https://github.com/Homebrew/homebrew) 
的库下载下来(下载zip包即可)
把下载的zip包放在文件夹"brew"(自己建立)内，
同时在 "brew" 文件夹内新建文件夹"brewRemote.git"
把步骤1下载下来的install文件也放进"brew"文件夹中  
<!--more-->  
3.下面开始使用 shell，前提，shell先进入到"brew"文件夹内


    cd brewRemote.git
    git init --bare  
    cd ../homebrew-master
    git init
    git add .
    git commit -m "create project"
    git remote add origin  brewRemote.git(的绝对路径)
    git push -u origin master

现在，我们本地就有了Homebrew的git库:brewRomote.git
接下来，修改install文件： 
应该在第7行左右，把

    HOMEBREW_REPO = 'https://github.com/Homebrew/homebrew'
改为：  
    `HOMEBREW_REPO = '/Users/tu/Documents/brew/brewRomote.git'`  
    
这里就是把Homebrew的github库，替换为本地库。 
注意：上面的brewRomote.git(/Users/tu/Documents/brew/brewRomote.git)的绝对路径为我的，你要替换为你自己本地的绝对路径
，最后继续执行ruby命令(注意：shell当前路径依然为brew)

    rm -rf /usr/local/Cellar /usr/local/.git && brew   
    cleanup
    ruby install  
    
第一行的rm命令，是为了防止之前你安装Homebrew失败而残留的文件，导致这次安装失败  
ruby install执行之后，安装命令行提示安装，应该会安装成功。 
安装成功后，执行以下命令：  
  
    `brew doctor`    
    
这个命令是Homebrew的自我检测命令，看看有没有配置不对的地方。 
但是我们使用的本地的git库brewRomote.git,所以会检测出这个有问题，我们接下来修改这个问题。  
找到这个文件 /usr/local/.git/config, 
由于.git文件是隐藏文件夹，你可以使用命令行来查找  

    open /usr/local/.git   
    
这样你就能找到config文件了
使用编辑器来修改config文件  
  
    url = /Users/baidu/TestTmp/yanshi/brewRomote.git改为       
    url = https://github.com/Homebrew/homebrew
保存config文件，大功告成！，再使用brew doctor检查一下，应该就没问题了。

原文：http://doc.okbase.net/shede333/archive/122619.html