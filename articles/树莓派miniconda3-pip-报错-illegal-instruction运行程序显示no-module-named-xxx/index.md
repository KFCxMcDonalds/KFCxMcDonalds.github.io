# 树莓派miniconda3 pip 报错 illegal instruction，运行程序显示No module named `xxx`


<!--more-->

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
