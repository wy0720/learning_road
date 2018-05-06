# JQuery学习

### 1.onclick相关

* 场景问题描述：在项目当中HTML标签中使用了onclick=“cancel()”来进行js的调用，其中遇到了两个问题，解决了很久，我的需求是从a标签中获得订单号，传入js中进行ajax请求。

  * 方式一(最初想法)

  ```php
  <a class="btn-cancel wd-60 ma"  onclick="cancel({$value['order_num']})" title="取消订单">取消订单</a>
  ```

  ```javascript
   function cancel(order_num){
     		console(order_num);
          $.post(
              "{:url('Users/cancel')}",
              {act:'cancel', order_num: order_num},
              function (data) {
                  if (data) {
                      alert(data.message);
                      if (data.status) {
                          setTimeout(function () {
                              window.location.reload();
                          }, 2000);
                      }
                  }
              }, 'json');
      }
  ```

  结果：

  ```
  order_num始终都是订单号加一	我的订单号是纯数字
  ```

  既然你加一我就在js中给你减一。。。。改良：

  ```javascript
   function cancel(order_num){
     		var tmp = parseInt(order_num) - 1;
     		console(tmp);
          $.post(
              "{:url('Users/cancel')}",
              {act:'cancel', order_num: order_num},
              function (data) {
                  if (data) {
                      alert(data.message);
                      if (data.status) {
                          setTimeout(function () {
                              window.location.reload();
                          }, 2000);
                      }
                  }
              }, 'json');
      }
  ```

  结果：

  ```
  控制台输出为NaN.........我居然改不动这个数。。待摸索
  ```

  结论猜想：onclick属性传值时如果方法中有数字会将数字加一然后传过去，然后附上某些特殊魔性导致不可改动。onclick属性点击应该是有一个值在记录是否点击的，点击即加1.。。。猜想待验证

  * 方式二：摒弃这种传参方式，我将订单号存到自定义的标签data-type当中在取出来总可以了吧。

  ```php
  <a class="btn-cancel wd-60 ma" data-type="{$value['order_num']}" onclick="cancel()" title="取消订单">取消订单</a>
  ```

  ```javascript
      function cancel(){
          console.log($(this));
          console.log('num：'+$(this).attr('data-type'));
          $.post(
              "{:url('Users/cancel')}",
              {act:'cancel', order_num: order_num},
              function (data) {
                  if (data) {
                      alert(data.message);
                      if (data.status) {
                          setTimeout(function () {
                              window.location.reload();
                          }, 2000);
                      }
                  }
              }, 'json');
      }
  ```

  以上代码我的意图是获取当前元素的data-type的属性值，然而。。

  $(this)选中的是当前window元素

  undefined

  查阅后。。。。。有些明了了

  ```php
  <a class="btn-cancel wd-60 ma" data-type="{$value['order_num']}" onclick="cancel(this)" title="取消订单">取消订单</a>
  ```

  ```javascript
      function cancel(obj){
          console.log(obj);
          console.log('num：'+$(obj).attr('data-type'));
  .....
      }
  ```

  获取成功

  总结：1.onclick(value),如果value是数字，传到方法中时会加一；2.onclick属性要得到该元素需要在onclick（）传入即：onclick="cancel(this)"；3.onclick=“cancel(this.id)”这种形式获取属性值只能够获取到系统定义的属性值，如果使用onclick=“cancel(this.data-type)”是获取不到的。

  备注：

  ```
  data-* 后面的是自定义的

  data-* 属性用于存储页面或应用程序的私有自定义数据。
  ```

  jquery 中的overlay属性十分有用，可以做弹出框

  根据官方的建议：具有 true 和 false 两个属性的属性，如 checked, selected 或者 disabled 使用prop()，其他的使用 attr()

  ​