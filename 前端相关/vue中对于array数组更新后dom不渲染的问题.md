### vue中对于array数组更新后dom不渲染的问题

* 参考：https://blog.csdn.net/zifeiyu130/article/details/78950244

* 我的理解

  * 通过传统的方式如：

    ```javascript
    Array.push(this.message,{msg:'asdf'})
    ```

    vue并不能感知数据的变化，只能通过vue中的set方法才能通知vue数据已经改变，为了保持使用的一致性，vue进行了push，pop等方法的进一步封装，称作变异方法，使用方式如下

    ```javascript
    this.message.push({msg:'asdf'})
    ```

    ​