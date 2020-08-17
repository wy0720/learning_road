## 关于jsonp:通过script标签获取数据的方式

> HTML 中 script 标签可以加载其他域下的js，比如我们经常引入一个其他域下线上cdn的jQuery。那如何利用这个特性实现从其他域下获取数据呢？
> 可以先这样试试：
> <script src="http://api.jirengu.com/weather.php"></script>
> 这时候会向天气接口发送请求获取数据，获取数据后做为 js 来执行。但这里有个问题， 数据是 JSON 格式的数据，直接作为 JS 运行的话我如何去得到这个数据来操作呢？
> 这样试试：
> <script src="http://api.jirengu.com/weather.php?callback=showData"></script>
> 这个请求到达后端后，后端会去解析callback这个参数获取到字符串showData，在发送数据做如下处理：
> 之前后端返回数据： {"city": "hangzhou", "weather": "晴天"}
> 现在后端返回数据： showData({"city": "hangzhou", "weather": "晴天"})
> 前端script标签在加载数据后会把 「showData({"city": "hangzhou", "weather": "晴天"})」做为 js 来执行，这实际上就是调用showData这个函数，同时参数是 {"city": "hangzhou", "weather": "晴天"}。
> 用户只需要在加载提前在页面定义好showData这个全局函数，在函数内部处理参数即可。
> <script>
> function showData(ret){
> console.log(ret);
> }
> </script>
> <script src="http://api.jirengu.com/weather.php?callback=showData"></script>

「原来这就是 JSONP(JSON with padding)，总结一下：」

\1. JSONP是通过 script 标签加载数据的方式去获取数据当做 JS 代码来执行

\2. 提前在页面上声明一个函数，函数名通过接口传参的方式传给后台，后台解析到函数名后在原始数据上「包裹」这个函数名，发送给前端。换句话说，JSONP 需要对应接口的后端的配合才能实现。

「原理很简单，但用起来代码好丑陋，我做个封装让小饥看看」



```js
function jsonp(setting){
  setting.data = setting.data || {}
  setting.key = setting.key||'callback'
  setting.callback = setting.callback||function(){} 
  setting.data[setting.key] = '__onGetData__'

  window.__onGetData__ = function(data){
    setting.callback (data);
  }

  var script = document.createElement('script')
  var query = []
  for(var key in setting.data){
    query.push( key + '='+ encodeURIComponent(setting.data[key]) )
  }
  script.src = setting.url + '?' + query.join('&')
  document.head.appendChild(script)
  document.head.removeChild(script)

}

jsonp({
  url: 'http://api.jirengu.com/weather.php',
  callback: function(ret){
    console.log(ret)
  }
})
jsonp({
  url: 'http://photo.sina.cn/aj/index',
  key: 'jsoncallback',
  data: {
    page: 1,
    cate: 'recommend'
  },
  callback: function(ret){
    console.log(ret)
  }
})
```