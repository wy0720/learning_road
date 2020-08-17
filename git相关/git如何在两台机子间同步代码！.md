## git如何在两台机子间同步代码！

# 前言

本文记录了如何使用git来在两台机器间同步代码，一台机器是linux，另一台是windows。
需求描述

有一份代码放在linux机器A上，我需要在另一台机器B上查看并编辑它，为了方便两台机器之间相互同步修改后的代码，git相比于scp等手动操作的好处：

1. 自动检查到修改的文件，不要自己去找改了那些文件
2. 附带版本管理功能
3. 便于扩展：有了第三台或更多机器的话，自己clone一份即可
4. 

# 步骤

### 1.在存放原始代码的机器上，比如A，假设代码目录为：/codes/project,

```shell


cd /codes/project

# 创建git代码仓库

git init
git add .
git commit -m "create project"


# 切换到project父目录，创建一个project-bare目录
cd ..
mkdir project-bare
cd project-bare


# 从原始代码仓库创建bare仓库，作为“中央”仓库，其他机器(包括本机的原始仓库)往这里push，从这里pull

git clone --bare ../project ./project-bare.git

# 回到project仓库目录

cd ../project

# 把project-bare添加为remote，

git remote add origin ../project-bare.git
git branch --set-upstream-to=origin/master master
```

### 2.在其它机器上，比如B:

假设通过ssh来连接机器A

git clone ssh://<username>@<ip>:<port>:/codes/project-bare/project-bare.git ./project

    1

clone下来之后，在机器B上做修改，然后commit，push之后，在机器A上就可以pull到了。反之在机器A的project目录做修改，commit，push之后，在机器B上也能pull下来了。

若要再添加一台开发机，重复第2步，clone操作即可。
遇到的坑

注意第1步中创建 project-bare.gti 的必要性

如果B直接clone A上的project仓库，并且将其添加为remote，那么在执行git push操作的时候是会报错的，因为此时B不知道A是否也在对工作副本进行了修改，直接push过去可能造成working copy的冲突，而创建的project-bare仓库只记录仓库信息不保存working copy，作为一个中央仓库使用



————————————————
原文链接：https://blog.csdn.net/elloop/java/article/details/54898512