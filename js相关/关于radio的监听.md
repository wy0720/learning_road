### 关于radio的监听

* 需求：需要点击不同的radio展示不同的内容

* 错误方案：change事件，click事件

  ```html
  <div class="type">
          <input type="radio" name="one_type" value="1" class="minimal type-jump">跳转url
  </div>
  <div class="type">
          <input type="radio" name="one_type" value="2" class="minimal type-alert">弹框
  </div>
  <div class="type">
          <input type="radio" name="one_type" value="3" class="minimal" checked>点击无效果
  </div>
  ```

  ​

  ```javascript
  //这两种方式都选不上，害我折腾半天
  $("input[name='one_type']").change(function(){
       
   })
  $("input[name='one_type']").click(function(){
        
    })
  ```

  ​

* 正确方案：ifChecked事件

  ```javascript
  $("input[name='one_type']").on('ifChecked',function () {})
  ```

  ​