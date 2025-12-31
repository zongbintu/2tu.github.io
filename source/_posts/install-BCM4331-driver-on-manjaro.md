title: install-BCM4331-driver-on-manjaro
tags: [manjaro,BCM4331,Mac]
date: 2025-12-31 08:59:56
categories: Other
---
### 现象
2012款MacBook Pro安装Manjaro xfce后使用无线网卡无法上网

### 查看网卡型号
```shell
lspci -nnk | grep -iA3 'network'
02:00.0 Network controller [0280]: Broadcom Inc. and subsidiaries BCM4331 802.11a/b/g/n [14e4:4331] (rev 02)
	Subsystem: Apple Inc. AirPort Extreme [106b:00f5]
	Kernel driver in use: bcma-pci-bridge
	Kernel modules: bcma
```

### 安装网卡驱动
```shell
yay -S noconfirm b43-firmware
```

### 重启
```shell
sudo reboot
```