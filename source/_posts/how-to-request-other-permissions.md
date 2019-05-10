title: how-to-request-other-permissions
tags: []
date: 2019-05-09 16:56:33
categories:
---
我们知道Android中权限分为 [normal](https://developer.android.google.cn/guide/topics/permissions/overview#normal_permissions), [dangerous](https://developer.android.google.cn/guide/topics/permissions/overview#dangerous_permissions)，其实官方介绍中还有[signature](https://developer.android.google.cn/guide/topics/permissions/overview#signature_permissions) and [special](https://developer.android.google.cn/guide/topics/permissions/overview#special_permissions)   

正常权限，对用户风险小，声明后系统会在安装时自动授予该应用该权限。不提示用户授予正常权限，用户无法撤消这些权限。

危险权限，涉及用户隐私，需要用户授权。且用户授权后也可以再修改拒绝。所以每次使用危险权限时都得先检查用户是否赋予该权限。

签名权限则是相同签名的应用可以使用。

特殊权限，与正常、危险权限不同，特别敏感。[SYSTEM_ALERT_WINDOW](https://developer.android.google.cn/reference/android/Manifest.permission.html#SYSTEM_ALERT_WINDOW)、[WRITE_SETTINGS](https://developer.android.google.cn/reference/android/Manifest.permission.html#WRITE_SETTINGS)

以上需要的权限都在权限列表内，声明，检查即可。

前几天gayhub群友贴了一张图问这个通知权限在哪里设置的

![image](/css/images/notification_enable.jpg)

WHAT，什么意思？如果拒绝用户就收不到应用的推送了吗？

必须收不到，或者说不会显示。但是官方权限列表中却没有对应允许通知的权限。从中发现部份不在权限列表中，仍然严重影响我们APP体验的**设置**。如：**允许通知、省电优化**等

这样的设置，官方为部份提供了API检测是否已设置。且只能引导用户设置，没有提供API让用户一键设置。

**允许通知**

检测是否开始允许通知权限

NotificationManagerCompat 中的 areNotificationsEnabled()

跳转允许通知设置界面

Settings.ACTION_APP_NOTIFICATION_SETTINGS

另外还有版本、机型兼容问题，需要自行研究和搜索

下面的后台运行则只能通过引导用户设置，而没有检测API

![image](/css/images/back_op.jpg)



---  END  ---
分享程序员所看、所想、所悟、所望  
![程序猜想](/css/images/qrcode4assert1024.jpg)