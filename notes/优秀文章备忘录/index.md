# 优秀文章备忘录


<!--more-->


## 1 [如何在树莓派上使用 Clash](https://mraddict.top/posts/clash-on-rpi/)

<img src="/images/优秀文章备忘录/如何在树莓派上使用 Clash.png" />

clash中树莓派4B64位对应的架构是armv8

## 2 [解决CondaHTTPError:HTTP 000 CONNECTION FAILED for url](https://blog.csdn.net/weixin_51484460/article/details/122179000)

conda安装到树莓派上后，用`conda create`会报错，显示连接有问题。

<img src="/images/优秀文章备忘录/condacreate问题.png" />

核心解决方法是禁用ssl，把channel里所有的`https://`都改成`http://`，然后输入命令`conda config --set ssl_verify false`
