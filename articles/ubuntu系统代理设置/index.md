# Ubuntu系统代理设置/JMS/Clash


# Ubuntu系统代理设置/JMS/Clash

<!--more-->

## 引

​    入学拿到一台主机，自己装了Ubuntu系统，常年在网络畅游的我发现竟然不能用Google搜索，连Chrome同步收藏夹都不行，于是折腾开始在Ubuntu上装代理。

## 准备工作

1.   不花钱的：Python3/pip3

2.   要花钱的：机场<font size=2 color='grey'>（懂得都懂）</font>

{{< admonition type=note open=true >}}
机场我个人用的JustMySocks提供的服务，购买之后（300+一年）会提供6个服务器地址。
{{< /admonition >}}

{{< admonition type=info title="website address" open=true >}}
网址：[JMS网站](https://justmysocks3.net/)
{{< /admonition >}}



## 踩过的坑

​    因为其他系统的电脑上用的一直是Shadowsocks，配置服务器比较简单明了，所以刚开始尝试的一直是“如何在Ubuntu系统布置Shadowsocks”，网上有很多教程，但是有一个地方我一直过不去:

​    在安装配置了Shadowsocks之后，总是提示**不支持加密方式aes-256-gcm**，按照网上的解决方法需要升级ss2.8到3.0，但是ss3.0的github仓库由于不知名原因没有内容了，找遍解决方法也迈不过这道坎，于是转战Clash（说得轻松其实浪费了好几个小时）。

{{< image src="/images/Ubuntu系统代理设置/ss仓库界面.png" caption="ss仓库界面">}}

## 正式开始

### 1 建立文件夹

​    在Ubuntu文件主目录中新建一个文件夹，名称clash（也可以自己选位置）

```shell
cd ~
mkdir clash
```

{{< image src="/images/Ubuntu系统代理设置/创建文件夹.png" caption="创建文件夹">}}

### 2 下载Clash

浏览器访问[Clash releases](https://github.com/Dreamacro/clash/releases)，选择**合适的版本**下载下来，*应该大部分电脑是下载图中框的那个版本*。

因为此时Ubuntu应该是无法访问github的，所以用可以访问github的其他电脑下载下来再用U盘或ssh传给Ubuntu系统。

<font color='red'>这里假设把该文件传到了 `~/下载`文件夹中</font>

{{< image src="/images/Ubuntu系统代理设置/下载页面.png" caption="下载页面">}}

传到Ubuntu系统中后，**解压缩并移动到创建的clash文件夹中**，如果跟我创建位置一样的可以执行：

```shell
# 解压缩
cd ~/下载
gunzip clash-linux-amd64-v1.11.8.gz
# 这里在下载文件夹中将出现一个可执行文件clash-linux-amd64-v1.11.8

# 移动文件
mv clash-linux-amd64-v1.11.8 ～/clash

# 可执行文件改名
cd ~/clash
mv clash-linux-amd64-v1.11.8 clash
```

{{< admonition type=tip open=true >}}

1.   可执行文件改名这一步很重要 

2.   所有的路径和文件名称都按照自己情况修改一下 

3.   <font color='HotPink'>如果命令提示没有权限就在前面加`sudo`</font>

     {{< /admonition >}}

完成后如下所示：

{{< image src="/images/Ubuntu系统代理设置/可执行文件改名.png" caption="可执行文件改名">}}

### 3 配置Clash

到这里Clash软件已经下载好了，但是还需要根据自己购买的服务进行配置。windows和mac端都有图形界面很简单就可以操作，Ubuntu要复杂一点。

#### <font color='DeepSkyBlue'>win/mac已有配置</font>

如果在windows和mac系统已经配置好了clash，可以按照以下步骤来做，这里:以mac系统为例，**否则跳过**：

:cat:点击你mac系统的小猫，选择`配置-打开配置文件夹`

{{< image src="/images/Ubuntu系统代理设置/clash配置.png" caption="clash配置">}}

:dog:将文件夹中你**想用的配置**传输到Ubuntu系统的<font color='red'>clash文件夹</font>中（u盘或者ssh，这里不赘述）。

{{< image src="/images/Ubuntu系统代理设置/配置传输.png" caption="配置传输">}}

:bird:接下来需要注意，传输完成后，如果文件名不是`config.yaml`则需要改成`config.yaml`

```shell
cd ~/clash
mv 传过去的文件名 config.yaml
```

#### <font color='DarkSeaGreen'>Ubuntu从头配置</font>

如果在其他系统中没用过clash，那么需要手动配置。

{{< admonition type=info open=true >}}

可能有些同学在其他博客中看到JMS开通了订阅服务可以直接订阅，但是亲测有问题，无法正确下载config.yaml文件，所以为了保险，这里还是介绍手动修改的方法，可以符合大部分读者的情况。

{{< /admonition >}}

:cow:首先下载并打开[**clash配置模版文件**](https://v2xtls.org/clash_jms2.yaml)。

用Ubuntu自带的文本编辑器或者terminal vim打开，这里介绍文本编辑器的方式，会用vim的自己应该知道怎么搞:stuck_out_tongue_winking_eye:。

:snake:打开文件后翻倒第85行左右，可以看到下面有六个配置块，一般是2个Shadowsocks+4个VMess。

其中`type`、`server`、`prot`、`cipher`、`password`、`uuid`、`alterId`是需要修改的，将其修改为机场提供的对应信息。

name：服务器名称，不用修改

>   type：服务类型ss对应Shadowsocks，vmess对应V2ray，如果是JSM就不用改，如果是其他提供商要根据自己的服务修改
>
>   server：服务器地址，注意不用加引号
>
>   prot：服务端口
>
>   cipher：加密方式
>
>   password：Shadowsocks密码
>
>   uuid：V2ray UUID
>
>   alterId：V2ray Alt Id

{{< image src="/images/Ubuntu系统代理设置/文件内容.png" caption="文件内容">}}

:chicken:打开服务商提供的server信息（这里以JMS为例），修改对应信息，有多少个服务器就改多少个。

{{< image src="/images/Ubuntu系统代理设置/server信息.png" caption="server信息">}}

配置完成后将文件名改为`config.yaml`

```shell
cd ~/clash
mv clash_jms2.yaml clash.yaml
```

### 4 启动clash

在terminal中其中clash可执行文件:

```shell
cd ~/clash
./clash -d .

# 如果提示权限不够 输入 chmod +x clash
# 在输入 ./clash -d .
```

显示以下界面表示成功配置并启动：

{{< image src="/images/Ubuntu系统代理设置/成功信息.png" caption="成功信息">}}

启动后，我们可以登陆clash的外部控制面板来改变代理设置

浏览器中打开[clash external controller](https://clash.razord.top/#/proxies)，如果打不开的话关闭所有网页再试一次。

如果提示输入地址、端口、密码的话，可以在我们的`config.yaml`文件里找到或者修改，如图红色标识:
{{< image src="/images/Ubuntu系统代理设置/登陆设置.png" caption="登陆设置">}}

打开的界面如图所示，可以进行一些代理策略的修改。

{{< image src="/images/Ubuntu系统代理设置/clash控制台.png" caption="clash控制台">}}

### 5 设置Ubuntu网络代理

还没完，还需要把clash应用为Ubuntu系统的代理

打开Ubuntu`设置-网络-选择小齿轮`，修改图中框选的部分

{{< image src="/images/Ubuntu系统代理设置/系统设置.png" caption="系统设置">}}

怎么改呢，看到前面运行clash之后terminal打印出的信息：

{{< image src="/images/Ubuntu系统代理设置/成功信息.png" caption="成功信息">}}

倒数第二对应设置里的HTTP（HTTPS跟HTTP保持一致就行）

倒数第一行对应设置里的Socks主机

<font size=5 color='IndianRed'>到这里整个配置就完成了，可以任意访问你想访问的网站了。</font>

{{< admonition type=note title=注意 open=true >}}

记得每次需要启动代理时都要运行一次clash

```shell
cd ~/clash
./clash -d .
```

{{< /admonition >}}

## 结束

转载请注明出处：[www.liwenwu.space](www.liwenwu.space)

### 参考文章：

[ClashX配置Just My Socks教程](https://v2xtls.org/clashx%e9%85%8d%e7%bd%aejust-my-socks%e6%95%99%e7%a8%8b/)

[Clash 在Ubuntu下新手使用教程](https://icode9.com/content-3-1040233.html)



如果有任何错误请留言。




