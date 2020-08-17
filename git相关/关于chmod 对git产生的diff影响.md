## 关于chmod 对git产生的diff影响

背景：项目开发在Linux服务器上，通过idea同步代码来进行开发。所以我们的代码一般都是从服务器拉的。而我偏爱idea的代码变更的变灰提示，于是我装了个git，在本地拉代码！！

起因：在项目目录下pull，死活pull不下来，使用git status查看文件状态，拉出一堆爆红的文件。经过文件对比，没有一个字符的更改

解决方法：git diff看到

```shell
old mode 100755
new mode 100644
```

查阅资料发现之前chmod对文件产生了大小变更，于是

```shell
git config core.filemode false
```

