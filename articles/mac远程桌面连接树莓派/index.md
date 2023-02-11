# Mac远程桌面连接树莓派


<!--more-->
# Mac远程桌面连接树莓派

通过ssh连接启用树莓派的远程桌面功能。

通过前面几步已经完成了成功打开树莓派并ssh连接了，接下来将用Remote Desktop连接树莓派使用。

## 1 安装远程桌面插件并启用

更新apt-get：

```shell
sudo apt-get updata 
```

安装远程桌面插件xrdp，等待完成。
{{< admonition type=note >}}
如果无法安装，可能是网络原因，最好是将树莓派换源，可以大大加快下载速度。
{{< /admonition>}}

```shell
sudo apt-get install xrdp
```

启动服务：

```shell
sudo service xrdp restart
```

完成之后，就可以使用Mac上的软件远程连接桌面了。

## 2 远程桌面软件

我使用的是`microsoft remote desktop`

<img src="/images/Mac远程桌面连接树莓派/remoteDesktop.png" />

1. 点击加号新建连接

     <img src="/images/Mac远程桌面连接树莓派/点击加号新建连接.png" />

2. 输入树莓派的ip地址

     <img src="/images/Mac远程桌面连接树莓派/输入树莓派的ip地址.png" />

3. 点击save然后双击窗口后输入用户名和密码后即可成功连接

<img src="/images/Mac远程桌面连接树莓派/用户名密码.png" />

4. 以后每次连接都需要输入一次用户名密码比较麻烦，所以在Remote Desktop中保存用户信息：

-   点击修改

<img src="/images/Mac远程桌面连接树莓派/点击修改.png" />

-   添加用户信息

<img src="/images/Mac远程桌面连接树莓派/添加用户信息.png" />

-   输入用户名和密码<img src="/images/Mac远程桌面连接树莓派/输入.png" />



用远程桌面登陆时如果出现蓝屏问题，请参考文章[树莓派安装远程桌面，并解决远程登录蓝屏的问题](https://blog.csdn.net/u011983700/article/details/128330740?spm=1001.2014.3001.5506)。

## 结束

转载请注明出处：[www.liwenwu.space](www.liwenwu.space)

