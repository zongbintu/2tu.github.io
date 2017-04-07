title: Android正确获取IP及MAC
date: 2017-04-06 20:06:52
tags: ipv4 dummy0 ipv6 mac wlan0 NetworkInterface WifiInfo 02:00:00:00:00:00
---
最近出了个bug，公司CMC报用户终端信息格式不正确。  
其中有MAC地址，  
设备：Oppo R9s、vivo X9  
系统：Android  
网络：4G  
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
      Log.e("WifiPreference IpAddres", e.toString());
    }
    return null;
  }
```
获取结果，格式为
<table><tr><td bgcolor=yellow>
fe80::188c:24ff:fe49:8e54%dummy0
</td></tr></table>
然后根据百度搜索得出，直接将“%dummy0%2"去掉，前面即为ipv6地址。当时未及细想信以为真，改了直接上线。最近回想惊觉造出了一堆错误数据。  
<!--more-->
##### 链路地址、回送地址(Link-local address、Loopback address) 
之前的代码中我们已经屏蔽了Loopback address。fe80::188c:24ff:fe49:8e54的确为IPv6，但是前缀为fe80::/64都是链路本地地址，不是我们真正想要的ip。IPv6几种地址类型、概念，详细见[IPv6](http://baike.baidu.com/link?url=iCSUzfkaTpmskK6k2ybPoCy6-dr28dzlAXY1ED8nszM6n-vs3lRgSEhUactfzgMuyIQrmcCNGUUx9bwdReOMFK)  

##### 获取IP正确方式  
从上面分析得知Link-local address和Loopback address，改造代码
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
          if (!inetAddress.isLoopbackAddress() && !inetAddress.isLinkLocalAddress()) {
            return inetAddress.getHostAddress().toString();
          }
        }
      }
    } catch (SocketException e) {
      Log.e("WifiPreference IpAddres", e.toString());
    }
    return null;
  }
```
OPPO R9s Android 6.0.1运行结果：  
<table><tr>
<th>类型</th>
<th>ip</th>
</tr>
<tr>
<td>wifi</td>
<td>172.27.35.3</td>
</tr>
<tr>
<td>流量</td>
<td>100.58.248.64</td>
</tr>
</table>
##### 扩展-获取6.0后的MAC地址  
dummy0、lo、p2p0、rev_rmnet0、rev_rmnet1、rev_rmnet2、rev_rmnet3、rmnet0、rmnet1、rmnet2、rmnet3、rmnet_smux0、sit0、wlan0我们发现有很多这样的名称，我的理解是除了wlan0是像我们电脑一样的真实无线的网卡以外，别的全是虚拟网卡用作内部通信用。说到网卡就想到获取mac地址。  
在6.0之后，Android 移除了对设备本地硬件标识符的编程访问权。WifiInfo.getMacAddress() 方法和 BluetoothAdapter.getAddress() 方法现在会返回常量值 02:00:00:00:00:00。有了上面使用Java api获取IP地址的经验，获取mac地址是否也可以在这里使用。上代码
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
<table><tr>
<th>代码类型</th>
<th>MAC</th>
<th>权限</th>
</tr>
<tr>
<td>WifiInfo</td>
<td>02:00:00:00:00:00</td>
<td>android.permission.ACCESS_WIFI_STATE</td>
</tr>
<tr>
<td>NetworkInterface</td>
<td>B8:37:65:2D:23:ED</td>
<td>android.permission.INTERNET</td>
</tr>
</table>

这里面涉及到挺多网络知识，现在还是比较懵逼。  
dummy0、lo、p2p0、rev_rmnet0、rev_rmnet1、rev_rmnet2、rev_rmnet3、rmnet0、rmnet1、rmnet2、rmnet3、rmnet_smux0、sit0到底咋回事儿？

##### 特别鸣谢
大牛老张，前公司上司，全栈。最近搞了一个有意思的产品[图交](http://wkok.me/)  
硬件玩家莽哥

##### 参考：  
[IPv6](http://baike.baidu.com/link?url=iCSUzfkaTpmskK6k2ybPoCy6-dr28dzlAXY1ED8nszM6n-vs3lRgSEhUactfzgMuyIQrmcCNGUUx9bwdReOMFK)    
[InetAddress API](https://developer.android.google.cn/reference/java/net/InetAddress.html)  
[硬件标识符访问权](https://developer.android.google.cn/about/versions/marshmallow/android-6.0-changes.html?hl=zh-cn#behavior-hardware-id)