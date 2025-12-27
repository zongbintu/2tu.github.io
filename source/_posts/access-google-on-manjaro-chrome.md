title: 在manjaro下使用Chrome科学上网
tags: [Manjaro]
date: 2025-12-27 21:23:04
categories: Other
---
安装完Manjaro之后迫不及待安装上clash-verge、Chrome，然后发现自带的Firefox可以科学上网，Chrome访问不上
原因是这个版本的Chrome中不支持代理设置，需要手动设置
将Clash Verge的代理端口设置给Chrome就可以了
```shell
cd ~/.config
vi chrome-configs.flag # 插入内容
-proxy-server=127.0.0.1:port # 替换成你clash代理的端口，保存 重启chrome就可以了
```
以上方法亲测可用，还有同学说可以如下设置的，没有尝试
```shell
/opt/google/chrome/chrome --proxy-server="[http://127.0.0.1:7890](http://127.0.0.1:7890/)"
```
