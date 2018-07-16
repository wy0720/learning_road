# select2使用

```html
<select class="form-control select2" id="search-schedule-name">
       <option value="0">请选择</option>
                                                                                            <option value="松江白班" selected="selected">松江白班</option>
                                                                                            <option value="徐汇白班">徐汇白班</option>
                                                                                            <option value="松江晚班">松江晚班</option>
                                                                                            <option value="徐汇晚班">徐汇晚班</option>
                                                                                            <option value="广州白班">广州白班</option>
                                                                                            <option value="松江一班">松江一班</option>
                                                                                            <option value="松江二班">松江二班</option>
</select>
```

* 取值和赋值

  ```javascript
  //赋值
  $('#search-schedule-name').select2().find("option:selected").text();
  //赋值
  $('#search-schedule-name').select2().find("option:contains('"+schedule_name+"')").attr('selected',true);
  ```

* 赋值时候的问题

  ```javascript
  $('#search-schedule-name').select2().find("option:contains('"+schedule_name+"')").attr('selected',true);
  $('#search-schedule-name').select2().find("option:contains('"+schedule_name+"')").attr('selected',true);
  //连续选两次才能赋值上去。。。。。
  ```



  经过几次实验，有如下发现：
  $('#search-schedule-name').select2().find("option:contains('"+schedule_name+"')").attr('selected',true);//第一次
  $('#search-schedule-name').select2();//第二次
  ```

  总结：赋值时候第一次只是利用插件的取值方法将选中的option元素找到，第二次则是利用插件来渲染被选中的元素。之所以为什么叫select2，就是应为赋值要选两次，哈哈哈哈。

* 加载

  在HTML加载时候需要在js当中加载触发select2，即：

  ```javascript
  $("document").ready(function (){
      .select2.select2();
  }
  ```

* select2设置默认值：

  ```javascript
  $('#select').val("要设置的值").select2();
  ```

  ​