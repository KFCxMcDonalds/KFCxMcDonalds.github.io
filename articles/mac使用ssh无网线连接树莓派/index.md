# Mac使用ssh无网线连接树莓派


<!--more-->

# Mac使用ssh无网线连接树莓派

不使用鼠标键盘和网线通过ssh连接树莓派

## 前提

树莓派开机后自动连接好WiFi并开启ssh服务，具体文章见[初见树莓派：系统安装](https://www.liwenwu.space/articles/%E5%88%9D%E8%A7%81%E6%A0%91%E8%8E%93%E6%B4%BE%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/)或[树莓派WiFi连接和开启ssh](https://www.liwenwu.space/articles/%E6%A0%91%E8%8E%93%E6%B4%BEWiFi%E8%BF%9E%E6%8E%A5%E5%92%8C%E5%BC%80%E5%90%AFssh)。

## 1 连接步骤

-   搜索树莓派IP地址
-   ssh远程连接树莓派terminal



### 1.1 搜索树莓派IP地址

本文面向不使用键盘和屏幕连接树莓派的读者，所以需要使用电脑或路由器来查找同一网络下树莓派的IP。这里介绍常用的几种方式，更多可以查看[官网doc](https://www.raspberrypi.com/documentation/computers/remote-access.html#how-to-find-your-ip-address)

#### :lion:路由器

浏览器打开路由器地址:`192.168.1.1`，登陆路由器账户并进入控制面板。

打开`已连接设备列表`，可以看到连接到本路由器的所有设备，通过排除能够认识的设备，比如电脑、打印机、手机等，找到树莓派的IP地址。

#### :cat:使用支持mDNS的设备查找

如果你的设备支持mDNS，那么可以通过ping树莓派的主机名来查找IP

{{< admonition type=info >}}
主机名可以在安装系统时在[Imager](https://www.raspberrypi.com/software/)的Advance Option中设置，详见文章[初见树莓派：系统安装](https://www.liwenwu.space/articles/%E5%88%9D%E8%A7%81%E6%A0%91%E8%8E%93%E6%B4%BE%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/)，默认为raspberry.local）：
{{< /admonition>}}

用网线连接电脑和树莓派，输入命令

```shell
ping raspberry.local
```

如果可以找到该主机名，那么ping命令将显示ping到的地址：

```shell
PING raspberry.local (192.168.1.131): 56 data bytes
64 bytes from 192.168.1.131: icmp_seq=0 ttl=255 time=2.618 ms
```

#### :tiger:nmap命令
##### <font color='red'>安装</font>

nmap(Network Mapper)是一个开源的网络查找工具。

-   Linux在命令行输入`apt install nmap`下载
-   macOS和Windows的下载请查看[nmap官网](https://nmap.org/download.html)

##### <font color='red'>使用</font>
要使用nmap查找树莓派的IP，首先需要知道你电脑的本机IP地址。

-   Linux在命令行输入`hostname -I`
-   macOS`系统设置-网络-详细信息- IP地址`

<img src="/images/Mac使用ssh无网线连接树莓派/mac本机ip.png" />

-   Windows`控制面板-网络-显示网络连接-选择网络-显示连接信息`

然后使用nmap查找本网段中的所有设备，比如你的本机IP为192.168.1.4，那么输入命令：`nmap -sn 192.168.1.0/24`

得到信息：

```shell
Starting Nmap 6.40 ( http://nmap.org ) at 2014-03-10 12:46 GMT
Nmap scan report for hpprinter (192.168.1.2)
Host is up (0.00044s latency).
Nmap scan report for Gordons-MBP (192.168.1.4)
Host is up (0.0010s latency).
Nmap scan report for ubuntu (192.168.1.5)
Host is up (0.0010s latency).
Nmap scan report for raspberrypi (192.168.1.8)
Host is up (0.0030s latency).
Nmap done: 256 IP addresses (4 hosts up) scanned in 2.41 seconds
```

即可看到树莓派的主机名及其IP地址。
{{< admonition type=note >}}
该方法来自官网教程，但是本人在使用时亲测无法显示出主机的hostname，只有IP，并不能很好地识别出树莓派，不建议使用。
{{< /admonition>}}

#### :dog:软件搜索

手机可以使用软件<font color='blue'>Fing</font>，查找本地网络中的树莓派。

macOS我使用的免费的<font color='blue'>LanScan</font>，可以找到网络中的树莓派。

![image-20230210210515555](/images/Mac使用ssh无网线连接树莓派/lanscan.png)
{{< admonition type=note >}}
如果按照教程完成了操作系统的安装和网络的配置还是找不到树莓派IP的话，可能是提供给树莓派的<font color='red'>电源电压不足</font>，必须是5V3A，如果电源功率不足黄灯可能规律的2次闪烁或者亮度很低。
正常启动的黄灯是不规律闪烁的，代表SD卡在写入读取数据。
{{< /admonition>}}



### 1.2 ssh连接树莓派

打开terminal，输入`ssh 用户名@ip地址`，会要求输入密码，输入后即可连接成功。

-   如果用Imager写入的系统，那么登陆时就使用当时设置的用户名和密码
-   如果是老版本自己写入的镜像，那么默认用户名为`pi`，密码是`raspberry`

![image-20230210213638921](/images/Mac使用ssh无网线连接树莓派/ssh连接.png)

显示以上界面表示连接成功，已经可以使用树莓派的命令行功能了。

至此本文结束。如果需要远程连接桌面，还需要用ssh执行一些列命令，具体见下一篇文章。

## 结束

转载请注明出处：[www.liwenwu.space](www.liwenwu.space)


如果对文章有疑问请留言。

