title: Node.js 版本管理-本地多个版本切换使用
tags: [nvm,Node.js版本,hexo,0kb]
date: 2023-02-17 08:02:10
categories: Web
---
在不同项目里依赖于不同Node.js版本，使用NVM（Node.js version manager）即可做到。
###  NVM安装
下载地址
[NVM for Windows](https://github.com/coreybutler/nvm-windows/releases)
[nvm](https://github.com/nvm-sh/nvm)  sh，支持Mac、Linux
下面介绍Windows安装方法
下载并安装
![nvm-setup.exe](/css/images/20230217_nvm-setup.png)

### NVM使用
<!-- more -->
运行命令行窗口
###### 查看版本信息
```bash
D:\>nvm -v
1.1.10
```
###### 查看Node.js版本
```bash
D:\>nvm list // 显示已安装Node.js版本，与nvm list installed相同
D:\>nvm list available // 显示可下载安装的Node.js版本
```
###### Node.js安装
- shell 安装

```bash
D:\>nvm install 12.14.0 // 安装12.14.0版本Node.js
```
有时候会安装不成功，此时可以手动下载

- 手动安装

下载需要的Node.js版本
[Node.js Releases](https://nodejs.org/en/download/releases/)
解压到nvm目录并改名为(v版本号)
![rename](/css/images/20230217_npm-rename.png)
此时查看已安装Node.js版本就已经有以上安装的版本了
###### 当前使用Node.js版本查看及切换
```bash
D:\>nvm current // 查看当前使用Node.js版本
D:\>nvm use 12.14.0 // 切换Node.js版本
```