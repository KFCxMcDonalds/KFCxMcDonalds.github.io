# 树莓派安装miniconda


<!--more-->
# 树莓派安装miniconda

树莓派4B已经将处理器从arm架构换成了aarch64架构，所以能够使用最新的aarch64版本的miniconda了。但是参考部分文章并实测发现，当前4B不能兼容miniconda3 4.10以上的版本，所以本文使用4.9版本。

4B以下版本请参考文章内容自行更改安装内容。

## <font color='sky blue'>前置步骤</font>

本部分主要引导读者查看本机的主要信息从而匹配最佳版本。

### 1   **查看本机处理器架构**

``` shell
uname -a
```

{{< figure src="/images/树莓派安装miniconda/uname.png" width=0.5 >}}

红框的那一串就是你手上树莓派的架构。如果是4B并且用的[Imager](https://www.raspberrypi.com/software/)安装的OS的话（详见文章[初见树莓派：系统安装](https://www.liwenwu.space/articles/%E5%88%9D%E8%A7%81%E6%A0%91%E8%8E%93%E6%B4%BE%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/)），显示的内容跟本文的应该是差不多的。如果是老版本的树莓派，那么显示的内容可能如下：

{{< figure src="/images/树莓派安装miniconda/别人的.png" width=0.5 >}}

说明你的架构是armv7l，他们两个使用的软件是完全不同的，不能混用。

### 2 **查看本机python版本**

``` shell
python
```

{{< figure src="/images/树莓派安装miniconda/py版本.png" width=0.5 >}}

第一行就是你本机的python版本。

## <font color='sky blue'>主要步骤</font>

得到上面的信息后，就可以选择合适的miniconda版本下载了，假设现在terminal所在的位置是`/home/pi/`

### 1 **获取miniconda安装包**

``` shell
wget https://repo.anaconda.com/miniconda/Miniconda3-py37_4.9.2-Linux-aarch64.sh
```

​	输入该命令将会把上面链接的文件下载到terminal当前文件夹`/home/pi/`下。

​	这个文件名表示该版本适合<font color='orange'>*python3.7*</font>，架构<font color='orange'>*aarch64*</font>的树莓派（Linux）安装，miniconda3的版本是4.9.2	

​	请读者根据自己机器的配置在[这个网页](https://repo.anaconda.com/miniconda/)上寻找下载合适的版本，注意如果是aarch64架构那miniconda3的版本不要超过4.10。

{{< admonition type=info >}}
如果你的架构是armv7l，那么应该下载的文件是：
{{< figure src="/images/树莓派安装miniconda/armv7l.png" height=1 >}}
{{< /admonition >}}


### 2 **安装**

``` shell
sudo bash Miniconda3-py37_4.9.2-Linux-aarch64.sh
```

​	文件名是刚才找到的一样的文件名。

### 3  **等待安装**

​	安装过程中可能需要输入一些字符。请根据提示进行。

{{< figure src="/images/树莓派安装miniconda/安装路径.png" width=0.5 >}}


​	值得一提的是到这一步时会提示你安装Miniconda3的位置，默认安装到根目录下，按ENTER将执行默认设置，	也可以在后面输入你想安装的位置，比如我这里为了方便以后寻找安装到了用户文件夹下。

### 4 超级用户尝试使用

```shell
sudo su
conda list
exit
```

​	由于刚刚Miniconda自动启动了conda init，并且我们是在超级用户的权限下安装的Miniconda，所以他自动加入了root用户的环境变量，直接输入命令就可以看到提示。	

![image-20230213005659495](/images/树莓派安装miniconda/超级用户.png)

### 5 更改权限

但是如果每次使用conda都要使用超级用户权限太过麻烦，所以我把他的权限更改给了用户（我默认使用的那一个）。

-   首先进入你安装miniconda3的目录，比如默认的是在/root，如果在第3步改到了pi用户目录<font color='pink'>（pi是你的用户名，不知道你的用户名是啥的话看terminal绿色的提示符，xxx@raspberry，xxx就是你的用户名）</font>那么就是在/home/pi，注意是miniconda3的上级目录

```shell
cd /home/pi
```

-   更改miniconda3文件的所有者

``` shell
sudo su
chown -R pi miniconda3
exit
```

这里的`pi`就是你用户的名字，不输出任何东西就是更改成功了。

### 6 添加环境变量

要在当前用户下使用conda，那么还必须把他的执行路径添加进环境变量。

``` shell
vim ~/.bashrc
```

进入`.bashrc`文件，如果提示没有vim的话用`nano ~/.bashrc`也可以。

在文件的最后添加一行：

```vim
export PATH="/home/pi/miniconda3/bin:$PATH"
```

{{< figure src="/images/树莓派安装miniconda/环境变量.png" height=1 >}}

这里的pi也是用户名字。写完后输入`:wq`退出vim。

{{< admonition type=info >}}
nano是先ctrl+o保存，再ctrl+x退出
{{< /admonition >}}

``` shell
source ~/.bashrc
```

应用更改。

### 7 尝试使用

``` shell
conda
```

显示如下信息表示成功：

{{< figure src="/images/树莓派安装miniconda/试一试.png" width=4 >}}

8.   换源

如果嫌安装packages慢的话可以换到清华源

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --set show_channel_urls yes
conda update conda
```

## <font color="red">错误分析</font>

成功安装miniconda3之后，如果想要使用创建好的虚拟环境，即：

```shell
conda activate test
```
可能会报如下错误：

```shell
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run
 
    $ conda init <SHELL_NAME>
 
Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell
 
See 'conda init --help' for more information and options.
 
IMPORTANT: You may need to close and restart your shell after running 'conda init
```

此时只需要根据提示初始化conda就可以，树莓派应该输入:
```shell
conda init bash
```
然后<font color='red'>重启terminal</font>，注意一定要重启一下，即可成功activate虚拟环境。

## 结束

转载请注明出处：[www.liwenwu.space](www.liwenwu.space)


## 参考文章

1.   [树莓派4B64位系统安装miniconda（折腾了几天终于解决）](https://blog.csdn.net/IT_lesliewu/article/details/124893143?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-124893143-blog-128620393.pc_relevant_3mothn_strategy_and_data_recovery&spm=1001.2101.3001.4242.1&utm_relevant_index=3)

2.   [树莓派上如何安装anaconda/miniconda环境配置](https://blog.csdn.net/weixin_39589455/article/details/128620393)
