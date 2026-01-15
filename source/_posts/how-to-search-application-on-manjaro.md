title: Manjaro怎么把安装的程序放到Applications
tags: [Manjaro]
date: 2026-01-15 20:26:37
categories: Other
---

自己下载安装的程序在Manjaro的Applications搜索不到，每次从opt目录点进去打开太麻烦了。解决需要手动创建一个 .desktop 快捷方式文件
1. 创建桌面入口文件
     在终端中使用编辑器（如 nano）在用户应用目录中创建一个新文件：
```shell
vi ~/.local/share/applications/软件名.desktop
```
2. 编写文件内容
   将以下模板复制并粘贴到文件中，根据实际情况修改路径：
```shell
[Desktop Entry]
Version=1.0
Name=你的应用名称 (e.g., IntelliJ IDEA)
Comment=你的应用描述 (e.g., Powerful IDE)
Exec=/opt/your-app-name/bin/your-executable-file %U  # 替换成你的实际路径
Icon=/opt/your-app-name/icon.png # 替换成你的图标路径 (可选)
Terminal=false
Type=Application
Categories=Development;IDE; # 替换成适合你应用的分类
```

关键字段说明：
Name: 搜索栏中显示的名称。
Exec: 程序启动文件的绝对路径。 %U 支持一次性打开多个文件或链接。
Icon: 图标文件的路径（通常在 /opt 目录下可以找到 .png 或 .svg 文件）。
Terminal: 如果程序需要打开终端运行，设为 true；普通图形界面程序设为 false。

3. 设置可执行
```shell
chmod +x ~/.local/share/applications/软件名.desktop
```

