### git更改远程仓库地址方法

* 参考：https://blog.csdn.net/u014203449/article/details/79974857


* 场景描述：原来从远程仓库克隆的项目，由于某些原因远程仓库换到了另外一个地方，那么我这个项目应该怎么处理？

* 解决方式：

  * 1.暴力解决法：删除项目，重新克隆~~~~

  * 2.修改.git 的config配置文件将远程分支的URL地址改变，然后在fetch一下，将本地分支删除。这种方式是最本质的修改，缺点是需要手动操作

  * 3.通过命令来解决（推荐！！！）

    ```shell
    #删除远程仓库分支
    git remote rm origin	

    #添加远程仓库分支地址
    git remote add origion 地址

    #fetch一下，分支信息等会有改变~~
    git fetch

    #查看一下远程分支
    git branch -r

    #一顿操作
    ....
    ```

    ​