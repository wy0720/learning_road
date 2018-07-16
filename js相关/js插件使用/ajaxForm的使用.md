ajaxForm的使用

有一个属性：beforeSubmit:  showRequest,  // 提交前修改数据

```javascript
function showRequest(formData, jqForm, options){
    //formData: 数组对象，提交表单时，Form插件会以Ajax方式自动提交这些数据，格式如：[{name:user,value:val },{name:pwd,value:pwd}]
   //jqForm:   jQuery对象，封装了表单的元素   
   //options:  options对象
}
```



参见：https://www.cnblogs.com/liuhongfeng/p/5150389.html