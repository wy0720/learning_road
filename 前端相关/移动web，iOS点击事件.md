###移动web，iOS点击事件

在移动web当中安卓很好的支持onclick事件，而在iOS当中使用onclick事件没有效果，查阅资料说iOS当中onclick事件支持的不是很好。有以下几个解决方案：

* 在点击的标签元素中添加cursor: pointer;属性

* 绑定click的同时绑定touchend

  ```javascript
  <a class="back">点击返回</a>

  $(document).on('click touchstart','.back',function(){
      window.history.back();
  })
  ```

  ​

