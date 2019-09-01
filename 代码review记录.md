### 代码review记录

#### 代码规范

* redis中key的命名，多个单词使用：链接。
* 没有实际变更的不要提交，空换行，空格等。提交前使用代码对比工具对比完在提交。
* debug代码不要保留，注释掉的debug代码也不行。注释的var_dump的代码或者自己写死参数调试的代码都要去掉。
* 变量命名要有意义。且命名要和所处理的内容相关。
* 将代码的复杂度降低，增加代码的可读性。

#### 脚本相关

* 编写脚本要保证可以重复执行。输出的信息要足够详细。
* 脚本中的异常要打印出来。并包含较多的有用信息。

#### 代码优化

* 过多相同结构的if else需要考虑将代码优化掉，比如

  ```php
  if (strpos($msg['Content'], '客服') !== false) {
      $msgs = [];
  } else if (strpos($msg['Content'], '中山') !== false) {
      $msgs = [];
  } else if (strpos($msg['Content'], '茂名') !== false) {
      $msgs = [];
  } else if (strpos($msg['Content'], '佛山') !== false) {
      $msgs = [];
  } else if (strpos($msg['Content'], '阳江') !== false) {
      $msgs = [];
  } else {
      $msgs = []
  }
  ```

  ```php
   //回复内容
   $replyData = [
       '客服' => [],
       '中山' => [],
       '茂名' => [],
       '佛山' => [],
       '阳江' => []
   ];
   foreach ($replyData as $replyKey => $replyContent) {
       if (strpos($msg['Content'], $replyKey) !== false) {
           $keyWorldReply = true;
           $msgs[]        = $replyContent;
       }
   }
  ```

* 对图片内容是否进行更改判断图片源地址，不能使用判断内容的方式来进行是否变更的判断。特别是用户流量的入口处，慎用file_get_contents。

* 如果很相似的两段代码出现了两次，需要考虑将其抽象封装成一个方法到service层或者其他工具层中进行复用。

* 在一个方法中调用中有调用的数量的需求时，应该考虑将底层方法封装成支持获取 指定数量的方法，而不是在上层方法中将其取出然后在截取。

  **好处：**

  * 提升代码效率
  * 减少很多重复代码（如果不支持数量调用，上层代码必定是会有很多重复代码进行数量的处理）。

* 对于底层封装的方法要做好空/不存在的判断！！！

* 在有循环查询的时候先考虑能不能使用wherein将数据筛选出来，再循环。

#### 数据库相关

* 注意不要<font color='red'>硬编码</font>！！！

  * 数据表中的有些字段有多个字段值的时候，将每一个值封装成常量到model层中，后面的查询条件中的这些字段使用这些常量来查询！！！

  * 有些很多个地方调用到的变量需要放到常量中。

* 在操作数据库时如果表比较小就直接循环操作插入就好，不需要批量插入，这样能够增加代码的可读性。

* 在attitude中存json的数据时候，取出来时使用getJsonField(key)这个方法，会将数据decode出来。key是attribute中的key值。

* 我们数据库中的金额都是存储的分。

* 在循环查库时要考虑一下是否使用缓存，方式有几种：

  * 使用getByIdStatic。
  * 若不是根据id来查询，使用static将数据缓存。

* 在循环查询时要考虑使用Utils::batchModels来查询。

* 查询数据库时要考虑是否会影响到索引的使用。

  **例如：**

  ```php
  andWhere('TO_DAYS(checked_time) = TO_DAYS(NOW())')
  ```

  **难点：**

  * 对于sql查询什么情况下会索引失效。

  **解决方式：**

  * 只有自己抽时间把这块东西补上。

* 数据库查询时要慎用asArray(),因为会跳过我们的权限。

* 数据更新同步修改updated字段。

* 在写sql或者进行表字段的新增是时候要考虑数据如果迁出去改用es查询，这样的查询方式es能不能走得通，或者实现难度如何。

  **例子：**在评论类型设置时，将评论的类型type用不同的string来区分，然后不同类型有不同的排序策略。那么久有了如下的sql:

  ```sql
  (case type when 'default' then 0 when 'tag' then 1 when 'text' then 2 when 'img' then 3 when 'text_img' then 4 else 0 end) desc
  ```

  **缺点：**对于这样的mysql特有的语句一旦将数据迁到es查询，实现起来会比较痛苦！！！

  **困难点：**在做这些策略的时候，前面的阻碍往往是我对es的了解程度。

  **解决方式：**只有多学习es的相关东西。

* 关于数据库设计

  * 状态不要用数字，例如审核状态有：待审核，审核通过，审核未通过，这几种状态，就不能够使用数组，使用字符串来区别不同的状态。

#### pr相关

* 关于pr的描述
  * 如果有脚本，需要将脚本指出来。包括：脚本功能，脚本执行的命令，脚本执行的频次（每天执行一次还是怎么着）
  * 如果数据表表结构有变更，需要将变更的sql发出来，并进行说明。
  * 若果新增配置，需要在pr中进行详细说明。
  * pr中要将任务描述清楚。进行了哪些改动或新增。

#### 总结代码书写的原则

* 代码可读性，尽可能的提升<font color=red>代码可读性</font>。
  * 代码层级不应该太深。
  * 代码复杂度不应该太大。
  * 执行效率和代码可读性冲突时，权衡考量一下，往可读性方面优化。
* 变量命名规范、有意义。
* 相似的代码<font color=red>重复两次，抽象封装</font>出来。
* 底层方法要做<font color=red>好空判断和isset判断</font>。
* 注重代码的<font color=red>执行效率</font>。
  * 加入static缓存。
  * 不做无意义的循环判断。
* 对于一些<font color=red>大表，用户流量大</font>的模块功能，要考虑后期<font color=red>数据迁移</font>到es等中间件的情况，适当考量<font color=red>表设计，查询</font>的方案。
* <font color='red'>查表</font>要关注索引，避免写太复杂的sql。
* 切记<font color='red'>硬编码</font>！！！否则全文搜索改到死！
* 对于一些<font color='red'>之前的肯定人碰到过的逻辑</font>，先考虑有没有<font color='red'>公共方法</font>，而不是去造轮子。比如attribute的使用，getJsonFiled的使用！！！
* 数据库字段<font color=red>状态不要用数字</font>
* 封装的方法返回的结构体要一致。