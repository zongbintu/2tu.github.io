title: Android正确获取IP及MAC
date: 2017-04-06 20:06:52  
categories: Android  
tags: [ipv4,dummy0,ipv6,mac,wlan0,NetworkInterface,WifiInfo,02:00:00:00:00:00]
---
每个产品都会统计用户终端信息。稍不注意就会经我们的手造成公司得到的数据错误，造成分析甚至战略错误。（假装是程序猿缔造了世界）  
   
设备：Oppo R9s、vivo X9  
系统：Android 6.0.2  
网络：4G  
错误IP和MAC分别为  
<font color=red>
fe80::188c:24ff:fe49:8e54%dummy0  
02:00:00:00:00:00
</font>

错误代码：  
```
  public static String getLocalIpAddress() {
    try {
      for (
          Enumeration<NetworkInterface> networkInterfaces = NetworkInterface.getNetworkInterfaces();
          networkInterfaces.hasMoreElements(); ) {
        NetworkInterface networkInterface = networkInterfaces.nextElement();
        for (Enumeration<InetAddress> inetAddresses = networkInterface.getInetAddresses();
            inetAddresses.hasMoreElements(); ) {
          InetAddress inetAddress = inetAddresses.nextElement();
          if (!inetAddress.isLoopbackAddress()) {
            String ipAddress = inetAddress.getHostAddress().toUpperCase();
            return ipAddress;
          }
        }
      }
    } catch (SocketException e) {
    }
    return null;
  }
```

然后根据搜索结果认为，直接将“%dummy0%2"去掉结果即为ipv6地址。不管你信不信反正当时我是信了，后来脑海里有一个声音时不时告诉我这不对，世界不是这样的。（程序猿后遗症）  
<!--more-->

#### 链路地址、回送地址(Link-local address、Loopback address) 
上面的代码中我们屏蔽了Loopback address。并且获得的fe80::188c:24ff:fe49:8e54确为IPv6，但是前缀为fe80::/64都是链路本地地址，不是我们真正想要的ip。IPv6几种地址类型、概念，详细见[IPv6](http://baike.baidu.com/link?url=iCSUzfkaTpmskK6k2ybPoCy6-dr28dzlAXY1ED8nszM6n-vs3lRgSEhUactfzgMuyIQrmcCNGUUx9bwdReOMFK)  

#### 解锁获取IP正确姿势  
经过上面度娘普及姿势得知Link-local address、Loopback address不是我们想要的IP，于是干掉它们，改造后代码如下（仅需要注意注释后面那一句）
```
  public static String getLocalIpAddress() {
    try {
      for (
          Enumeration<NetworkInterface> networkInterfaces = NetworkInterface.getNetworkInterfaces();
          networkInterfaces.hasMoreElements(); ) {
        NetworkInterface networkInterface = networkInterfaces.nextElement();
        for (Enumeration<InetAddress> inetAddresses = networkInterface.getInetAddresses();
            inetAddresses.hasMoreElements(); ) {
          InetAddress inetAddress = inetAddresses.nextElement();
          //过滤Loopback address, Link-local address
          if (!inetAddress.isLoopbackAddress() && !inetAddress.isLinkLocalAddress()) {
            return inetAddress.getHostAddress();
          }
        }
      }
    } catch (SocketException e) {
    }
    return null;
  }
```
OPPO R9s Android 6.0.1运行结果：  

类型 | IP
-------- | ---
wifi | 172.27.35.3
流量 | 100.58.248.64

#### 扩展-解锁6.0后获取正确MAC姿势  
dummy0、lo、p2p0、rev_rmnet0、rev_rmnet1、rev_rmnet2、rev_rmnet3、rmnet0、rmnet1、rmnet2、rmnet3、rmnet_smux0、sit0、wlan0等等，我们发现有很多这样的名称。我的理解是除了wlan0是与电脑一样的真实无线网卡外，其它全是虚拟网卡，用于内部通信。  
在6.0之后，Android 移除了对设备本地硬件标识符的编程访问权。使用WifiInfo.getMacAddress() 、BluetoothAdapter.getAddress() 返回值为常量02:00:00:00:00:00。有了上面通过Java api获取IP地址的经验，获取mac地址是否也可以在这里使用。上代码
```
  public static String getMac() {
    try {
      for (
          Enumeration<NetworkInterface> networkInterfaces = NetworkInterface.getNetworkInterfaces();
          networkInterfaces.hasMoreElements(); ) {
        NetworkInterface networkInterface = networkInterfaces.nextElement();
        if ("wlan0".equals(networkInterface.getName())) {
          byte[] hardwareAddress = networkInterface.getHardwareAddress();
          if (hardwareAddress == null || hardwareAddress.length == 0) {
            continue;
          }
          StringBuilder buf = new StringBuilder();
          for (byte b : hardwareAddress) {
            buf.append(String.format("%02X:", b));
          }
          if (buf.length() > 0) {
            buf.deleteCharAt(buf.length() - 1);
          }
          String mac = buf.toString();
          return mac;
        }
      }
    } catch (SocketException e) {
      e.printStackTrace();
    }
    return null;
  }
```
OPPO R9s Android 6.0.1运行结果：  

代码类型 | MAC | 权限
------ | ----- | ---
WifiInfo | 02:00:00:00:00:00 | android.permission.ACCESS_WIFI_STATE
NetworkInterface | B8:37:65:2D:23:ED | android.permission.INTERNET

这里面涉及到挺多网络知识，现在还是比较懵逼  
dummy0、lo、p2p0、rev_rmnet0、rev_rmnet1、rev_rmnet2、rev_rmnet3、rmnet0、rmnet1、rmnet2、rmnet3、rmnet_smux0、sit0到底咋回事儿？

#### 特别鸣谢
大牛老张，前公司上司，全栈。最近搞了一个有些性感的产品[图交](http://wkok.me/)  
硬件玩家，网络牛人莽哥

#### 参考：  
[IPv6](http://baike.baidu.com/link?url=iCSUzfkaTpmskK6k2ybPoCy6-dr28dzlAXY1ED8nszM6n-vs3lRgSEhUactfzgMuyIQrmcCNGUUx9bwdReOMFK)    
[InetAddress API](https://developer.android.google.cn/reference/java/net/InetAddress.html)  
[硬件标识符访问权](https://developer.android.google.cn/about/versions/marshmallow/android-6.0-changes.html?hl=zh-cn#behavior-hardware-id)