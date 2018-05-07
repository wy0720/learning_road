### #dataTable使用

参考：http://www.cnblogs.com/sheldon-lou/p/4169902.html

js中单双引号使用（双引号包单引号，变量能解析）。。。好坑

```javascript
var type = GetQueryString('type');	//结果：type是一个字符串
console.log($('#search-type').find('option:contains("'+type+'")'));//可以获取
console.log($('#search-type').find("option:contains('"+type+"')"));//也可以获取
//用value选中nn
$('#search-channel-id').find("option[value='"+channel_id+"']").attr('selected',true);
```

总结：有格式为：方法（引号 子方法（引号 变量 引号） 引号）格式的拼接变量的方法：

```javascript
"  '" + 变量 + "' "；
'  "' + 变量 + '" ';
```

