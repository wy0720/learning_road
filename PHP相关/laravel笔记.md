# laravel笔记

## 中间件

参考文章：http://blog.csdn.net/zjiang1994/article/details/52638037

1.注册：

### handle()方法

handle()方法有两个参数 

handle()方法有两个参数 
`$request` —>请求信息，里面包含了输入，URL，上传文件等等信息。 

handle()方法有两个参数 
`$request` —>请求信息，里面包含了输入，URL，上传文件等等信息。 
`$next` —>闭包函数。我的理解是将接下来需要执行的逻辑装载到了其中。

### terminate()方法

参数 

参数 
`$request` —>请求信息，里面包含了输入，URL，上传文件等等信息。 

参数 
`$request` —>请求信息，里面包含了输入，URL，上传文件等等信息。 
`$response` –>响应消息，包含了逻辑处理完成后传出到的响应消息。

因为terminate()方法只是在响应后进行一些处理所以没有返回值。



### 数据库查询wherein

```
laravel中当wherein 条件为空时会自动转化为 where 0 = 1
```