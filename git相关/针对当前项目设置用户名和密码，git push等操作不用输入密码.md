## 针对当前项目设置用户名和密码，git push等操作不用输入密码

```
git config  credential.helper store  

```

1. 在项目所在位置打开git-bash， 在git bash交互环境输入命令：

``` git config  credential.helper store  ```
不加参数： --global  只对这个仓库生效，并非全局设置 。

2. 运行后 查看 .git文件夹的 config 文件，发现最后多了两行内容：



3. 后续正常 push，第一次要输入账号密码，以后就不用了。
4. 参考：https://blog.csdn.net/jiangyu1013/article/details/90264211