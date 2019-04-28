## mysql可以远程登陆本地登陆不了

1.环境描述：mysql 5.6.40

 2.遇到的问题：

   mysql -u xxxx -p xxx

   用户名密码远程可以登陆，但是本地cmd中执行登陆不了。

   原因其实很简单，mysql中一个用户名为空的访问本地权限的字段，所有登陆时优先匹配了这一条，就无法登陆了。

select user,host from mysql.user;

3.解决方法

###删除用户即可

drop user ''@localhost;

###刷新

####flush privileges;

作者：镜水灵动 
来源：CSDN 
原文：https://blog.csdn.net/u014172271/article/details/80579871 
版权声明：本文为博主原创文章，转载请附上博文链接！