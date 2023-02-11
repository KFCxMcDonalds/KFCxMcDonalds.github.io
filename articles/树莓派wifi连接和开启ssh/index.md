# 树莓派WiFi连接和开启ssh


<!--more-->
# 树莓派WiFi连接和开启ssh

树莓派连接电源时自动连接WiFi并开启ssh，如果是最新使用官方[Imager](https://www.raspberrypi.com/software/)安装的系统将不需要此操作，具体见文章[初见树莓派：系统安装](https://www.liwenwu.space/articles/%E5%88%9D%E8%A7%81%E6%A0%91%E8%8E%93%E6%B4%BE%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/)。

## 1 树莓派开机自动连接WiFi

用电脑读取打开树莓派的sd卡根文件，新建一个文件`wpa_supplicant.conf`，添加内容：

```shell
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
   ssid="ssid"
   psk="password"
   key_mgmt=WPA-PSK
   priority=1
}
```

{{< admonition type=info >}}
### 各个项的含义：
*ssid*：网络的名字

*psk*：网络密码

*priority*：连接优先级，数字越大优先级越高。(可以使用多个network块来指定可连接的多个WIFI)

*key_mgmt*：WIFI加密方式

*scan_ssid*：连接隐藏WIFI时需要置1
{{< /admonition >}}

### <font color="hot pink">特殊情况</font>

1.   WIFI没有密码

``` shell
network = {
	ssid="ssid"
	key_mgmt=NONE
}
```

2.   WIFI使用WEP加密

```shell
network = {
	ssid="ssid"
	key_mgmt=NONE
	wep_key0="password"
}
```



## 2 树莓派自动开启ssh

2016.11.25之后的老版本系统镜像，ssh服务是默认关闭的。

打开SD卡根目录，新建`ssh`文件，<font color="purple">文件无后缀</font>。即可在下次树莓派开机时自动打开ssh服务。

> 注意：打开树莓派后，一定要设置以后开机自启ssh服务，不然重启后还是会自动关闭，又需要插拔SD卡新建ssh文件才能使用。



## 结束

转载请注明出处：[www.liwenwu.space](www.liwenwu.space)



如果对文章有疑问请留言。


