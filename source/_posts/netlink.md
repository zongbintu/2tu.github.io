title: netlink
tags: [Linux]
date: 2023-10-07 18:07:29
categories: Other
---

什么是Netlink通信机制

Netlink是linux提供的用于内核和用户态进程之间的通信方式。

但是注意虽然Netlink主要用于用户空间和内核空间的通信，但是也能用于用户空间的两个进程通信。只是进程间通信有其他很多方式，一般不用Netlink。除非需要用到Netlink的广播特性时。

那么Netlink有什么优势呢？

一般来说用户空间和内核空间的通信方式有三种：/proc、ioctl、Netlink。而前两种都是单向的，但是Netlink可以实现双工通信。

Netlink协议基于BSD socket和AF_NETLINK地址簇(address family)，使用32位的端口号寻址(以前称作PID)，每个Netlink协议(或称作总线，man手册中则称之为netlink family)，通常与一个或一组内核服务/组件相关联，如NETLINK_ROUTE用于获取和设置路由与链路信息、NETLINK_KOBJECT_UEVENT用于内核向用户空间的udev进程发送通知等。

netlink具有以下特点：

- 支持全双工、异步通信(当然同步也支持)
- 用户空间可使用标准的BSD socket接口(但netlink并没有屏蔽掉协议包的构造与解析过程，推荐使用libnl等第三方库)
- 在内核空间使用专用的内核API接口
- 支持多播(因此支持“总线”式通信，可实现消息订阅)
- 在内核端可用于进程上下文与中断上下文

没有Linux和C的基础是真看不懂这是啥玩意儿...