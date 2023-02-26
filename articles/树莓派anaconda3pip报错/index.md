# 树莓派miniconda3 pip 报错 illegal instruction，运行程序显示No module named `xxx`


<!--more-->

## 2023.2.26 14:18更新
我又来了，太一波三折了，我好像解决了，就是比较麻烦。
原因是miniconda的libcrypto.so.1.1文件出错，需要手动替换。

{{< admonition type=tip >}}
假设miniconda安装位置是在`~/miniconda3`，如果不是的话后面的路径需要做相应修改。
{{< /admonition >}}

用`/usr/lib/aarch64-linux-gnu/libcrypto.so.1.1`替换`~/miniconda3/lib/libcrypto.so.1.1`解决。

<img src="/images/树莓派anaconda3pip报错/libcrypto.png" />

#### 解释一下其他情况怎么使用这个方法：
1. 对于树莓派4B64位系统，第一个文件的位置应该跟我一样。但是如果是其他系统，可能会稍有不同，比如github issue上有人是在`/usr/lib/libcrypto.so.1.1`，有人是`/usr/lib64/libcrypto.so.1.1`，所以根据自己的系统可能需要找一下。
2. 麻烦的点在第二个位置。都知道conda是有一个默认环境base的，所以上述被替换的文件，对应的是<font color=purple>base</font>环境的文件，即替换之后，base环境可以正常使用pip了，但是之前创建的其他虚拟环境还是illegal instruction。<font color=purple>需要将其他虚拟环境里的文件也手动替换</font>，文件位置在`~/miniconda3/envs/test/lib/libcrypto.so.1.1`。其中`test`替换为你虚拟环境的名字。
3. 也就是说，以后每次新建一个conda虚拟环境，都要手动替换一次。其他更方便的方法暂时没有找到，找到了再更新吧。


## 2023.2.26 01:00更新

用了下面的解决方法后，又发现一个问题，只能在conda的base环境正常使用，create新的环境后依然，不可以，pip，提示illegal instruction。我去github上找了下，发现很多人有这个问题，然后我trace了一下pip的命令，发现fail在打开一个文件`openat(AT_FDCWD, "/home/liwenwu/miniconda3/envs/RPI_exps/lib/python3.7/lib-dynload/../../libcrypto.so.1.1", O_RDONLY|O_CLOEXEC) = 3`

github issue里[有一个老哥](https://github.com/conda/conda/issues/10723#issuecomment-971603856)用`/usr/lib/libcrypto.so.1.1`文件替换`~/pi/miniconda3/lib/libcrypto.so.1.1`之后解决了，是conda自己的文件corrupt了。

但是我的树莓派，第二个文件有，第一个文件没有，我不知所措了。总之，miniconda在aarch64上有问题，别用了，吐了。

{{< style "color:skyblue;text-align:center" >}}
# ------以下为原文------
{{< /style >}}

## 错误分析
全部是本人踩的坑，以下文章内容全部关于miniconda3。不想看的直接跳到<font color='red'>解决方法</font>部分。

本人是在树莓派上安装了vscode来写代码，然后发现，使用miniconda3创建的虚拟环境运行程序时会遇到如下问题（包括但不限于）：
1. 不能在虚拟环境里用pip安装package。`pip install xxx`，提示illegal instruction。
我就想有没有可能miniconda3虚拟环境没有自动给我安装pip，于是我想手动安装，输入了`conda install pip`，但是提示我已经安装过了。输入`conda list`果然已经有pip了，就很扯，有为啥不能用。
2. 然后我发现可以使用`sudo pip`。但是！pip安装的包，还是不能被import：如果vscode直接run或者terminal输入`python xxx.py`运行程序会提示No module named 'xxx'。`sudo python xxx.py`就可以正常使用。

于是显然，问题肯定是出在`sudo`上，sudo是什么呢？超级用户权限，于是我打开了我之前安装miniconda3过程记录的文章[【树莓派】树莓派安装miniconda 2023版](https://www.liwenwu.space/articles/%E6%A0%91%E8%8E%93%E6%B4%BE%E5%AE%89%E8%A3%85miniconda/)（现在错误已更改）发现，我安装miniconda时是用sudo权限安装的；但是我明明用`chown -R`更改了miniconda3文件夹下所有文件的权限到当前用户而不是超级用户了，为什么当前用户不能使用conda的某些命令呢。这一部分我还是没有搞清楚，总感觉是权限分配没成功，但是管他的，我已经找到完美解决上述的方法了，暂时先不去深究原因了。

## 解决方法
简单粗暴，重新安装miniconda3，根除问题。

1. 删除miniconda3文件夹
    懒得找conda卸载命令了，直接删文件夹。找到你当年安装的miniconda3文件，默认是在`/root/miniconda3`。

  ```python
  sudu su
  cd /root
  rm -r miniconda3
  exit
  ```
  重新启动一下terminal。
2. 重新安装miniconda3
    这一部分比较多，请见文章[【树莓派】树莓派安装miniconda 2023版](https://www.liwenwu.space/articles/%E6%A0%91%E8%8E%93%E6%B4%BE%E5%AE%89%E8%A3%85miniconda/)的<font color='red'>主要步骤</font>部分，注意一定不要再用sudo安装了。
3. 问题解决
    是的，解决了，至少我的树莓派上完全解决了，vscode也能用了，terminal也能用了。

## 结束

转载请注明出处：[www.liwenwu.space](www.liwenwu.space)
