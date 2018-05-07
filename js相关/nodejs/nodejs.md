# nodejs

## 1.安装：

教程：https://www.phpsong.com/2117.html

## 2.线上部署

作者：知乎用户

链接：https://www.zhihu.com/question/46630687/answer/157166318

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

感觉好多人把最基本的都忘了

还npm run dev

还守护进程,用守护进程也应该你自己用nodejs实现一个web静态服务器而不是去守护npm run dev

楼主问的既然是布署，哪默认就应该是生产环境下的布署，vue开发的应用本质上就是静态文件,无论你用何种web服务器，放上去就应该能通过http访问

为什么现在好多人连这个都搞不定了呢？

原因在于现在太多的前端工具帮我们做了太多，而我们只学会了打命令，而没有搞清楚这些命令到底做了些什么

接下来我来帮大家来捋一捋

首先VUE 是一个javascript的前端框架，注定了它是运行在浏览器里的，对服务器本地没有任何要求，只要一个静态文件服务器能通过http访问到其资源文件就足矣！无论你是用apache ,ngnix 就算你要用node 自己实现一个静态文件服务器，也用不了多少行代码。

npm run dev 是用来在本地开发的时候做调试用的，**vue开发的是前端的东西，不是nodejs 服务端程序，按道理讲，生产环境里就不该存在npm，甚至nodejs也不需要（用nodejs来做web静态服务的除外）**，楼主通过ssh进入到服务器里，再运行npm run dev 来启动哪是开发机上做的事情。正确的做法很简单，通过**npm run build 把生成的dist文件夹（不要上传文件夹）里的内容上传到http服务器上**就可以通过 http来访问了，开发机上正常，上传以后 程序出现错误不能运行的原因99.99%的可能性是你**引用资源的路径**有问题。

一般如果vue框架的程序上传到网站服务器的根目录下是不 会有问题的，也不存在资源文件引用错误的情况，但如果你不是根目录，就会有问题，通过vue-cli 生成的开发目录，build以后默认引用资源文件的路径是

> 比如 app.js的引用方式<script type="text/javascript" src="/static/js/app.js"></script>

所以，如果你的目录结构是 如下

```
www/
 +hot/
   +static/
   +index.html

```

当你通过[http://www.xxx.com/hot/index.html](https://link.zhihu.com/?target=http%3A//www.xxx.com/hot/index.html)来访问的时候 会出现找不到app.js

因为index.html里引用的app.js路径是

```
http://www.xxx.com/static/js/app.js

```

但是app.js的实际路径是

```
http://www.xxx.com/hot/static/js/app.js 所以出现了404

```

要解决的方法很简单把引用方式 改成如下两种方式 都可以

```
<script type="text/javascript" src="./static/js/app.js"></script>
或者
<script type="text/javascript" src="/hot/static/js/app.js"></script>
推荐这样的方式，可以保正在vue-router生成的url下也不出现问题

```

都可以，当然这也不需要手动去改，

在webpack的配置文件中修改

```
assetsPublicPath: './'
或者
assetsPublicPath: '/hot/',

```

这样，我们就基本解决了有服务器子目下运行的问题，

当然，如果你还用到了vue-router

要在router的配置中加上

```
export default new Router({
  mode: 'history',
  base: '/hot/', //加上这一行

```

用浏览器打开[http://www.xxx.com/hot/](https://link.zhihu.com/?target=http%3A//www.xxx.com/hot/)

一切运行正常，点击各种链接也能出来，但是由于我启用了history模式，我直接在浏览里访问 [http://www.xxx.com/hot/item/1](https://link.zhihu.com/?target=http%3A//www.xxx.com/hot/item/1) 这样的url是又404了，为什么 我可以在首页通过点击进入 [http://www.xxx.com/hot/item/1](https://link.zhihu.com/?target=http%3A//www.xxx.com/hot/item/1) 这个页面，但是直接访问又不行呢，

因为在history 模式下，只是动态的通过js 操作window.history 来改变有浏览器地址栏里的路径，并没有发起http请求，但当你直接 在浏览器里输入这个地址的时候 就一定要先对服务器放起http请求，但是这个目标在服务器上又不存在所以就返回了404了，怎么解决呢，就是把所有的请求全部转发到[http://www.xxx.com/hot/index.hmtl](https://link.zhihu.com/?target=http%3A//www.xxx.com/hot/index.hmtl)上就可以了

我用的是apache 做web服务器的虚拟空间，而且开启支.htaccess文件支持，

我成我的文件里加上如下，就一切正常了，

```
<IfModule mod_rewrite.c>
  Options +FollowSymlinks
  RewriteEngine On
  
  RewriteCond %{REQUEST_URI} ^/(hot|hot/.*)$
  RewriteRule ^/hot/index\.html$ - [L,NC]
  
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(hot|hot/.*)$ hot/index.html [L]
</IfModule>

```

这个配置的作用就是把所有服务器上不存在请求全部转发到index.html上去，这样就可以直接通过各种url来访问了

