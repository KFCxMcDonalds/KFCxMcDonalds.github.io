# 网站维护


# 网站维护

## push本地内容到github

Step1: 查看工程代码做了什么修改:taiwan:

Step2: 更新修改的代码 add:joy:

$\sum_1^2x +y = z$



$$ a+b - c\\c+2=3 $$



vhuyin:

[Hugo]^(一个开源框架)





Step3: 书写更新日志 commit

Step4: 提交更新到远程仓库 push 	 

```shell
git status  # 查看修改

git add .  # 更新所有的修改代码
# git add test.cpp. # 更新部分代码

git commit -m "更新日志"

git push origin master. # 提交更新到master分支
# git push origin panda. # 提交更新到panda分支
```

## pull仓库内容到本地

```shell
git pull --rebase origin master
# git push origin master. # 拉取后重新push
```

如果本地内容有更改，可能无法pull本地内容，可以强制覆盖本地代码

```shell
git fetch --all  # 拉取所有更新，不同步
git reset --hard origin/master  # 本地代码同步线上最新版本
```

也可以暂存变更内容

```shell
git stash. # 暂存
git pull --rebase origin master
```

## 从本地更新网站内容

```shell
cd ~/Liwen_Blog/mysite
# 更新内容指令见后面
hugo server  # 本地检查内容 http://localhost:1313/
hugo  # build site --> public
cd public
git add .
git commit -m "更新日志"
git push origin master
```


