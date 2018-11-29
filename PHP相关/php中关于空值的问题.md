### php中关于空，null,'',0,false的理解，也是empty（），isset（）函数的重新认识

* 先上代码

  ```php
  $a = null;
  $b = '';
  $c = 0;
  $d = '0';
  $e = false;

  var_dump($a);	//NULL 
  var_dump($b);	//string(0) ""
  var_dump($c);	//int(0)
  var_dump($d);	//string(1) "0"
  var_dump($e);	//bool(false)
  ```

  从这里我们看出了什么问题吗？赋值为null的没有数据类型，那么其他数据都被设置了吗？即isset（），我们继续验证

  ```php
  $a = null;
  $b = '';
  $c = 0;
  $d = '0';
  $e = false;

  var_dump(isset($a));	//bool(false) 
  var_dump(isset($b));	//bool(true)
  var_dump(isset($c));	//bool(true)
  var_dump(isset($d));	//bool(true)
  var_dump(isset($e));	//bool(true)
  ```

  很明显，我们的假设是正确的，那么，这些值的empty判断又如何呢？

  ```php
  $a = null;
  $b = '';
  $c = 0;
  $d = '0';
  $e = false;

  var_dump(empty($a));	//bool(true) 
  var_dump(empty($b));	//bool(true)
  var_dump(empty($c));	//bool(true)
  var_dump(empty($d));	//bool(true)
  var_dump(empty($e));	//bool(true)
  ```

  结果很明显，这些值都是空的~~~~，那么对于$d这样的值，已经赋值了一个string的数据，那为嘛还是空？？莫非会转化为int 0 来判断？？

  查看了手册，里面对这个函数的解释是：

  ```
  判断一个变量是否被认为是空的。当一个变量并不存在，或者它的值等同于FALSE，那么它会被认为不存在。如果变量不存在的话，empty()并不会产生警告。 

  以下的东西被认为是空的： 
      •"" (空字符串)
      •0 (作为整数的0)
      •0.0 (作为浮点数的0)
      •"0" (作为字符串的0)
      •NULL
      •FALSE
      •array() (一个空数组)
      •$var; (一个声明了，但是没有值的变量)
  ```

* 总结

  经过上边的实验，其实已经明了了

  1、对于变量是否被设置（isset（））判断，值为null的都未被设置，只要有变量类型了，这个变量就已经被设置了！！！

  2、对于变量是否是空（empty（））的判断，为被设置的，为0的字符串都为空！！！