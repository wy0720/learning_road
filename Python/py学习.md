### py学习

* list和数组

  list可以包含不同类型的数据

  array只能是同一类型数据的集合

* 赋值

  * ```python
    a, b, c = 1, 2, 3
    ```

    ​

* py的运算奇葩处

  ```python
  除法：
  8 / 4     =>2.0
  8 / 3     => 2.66666
  8 // 4	  => 2
  8 // 2	  => 2

  /	值为浮点数
  //	值为整数部分，不做四舍五入
  ```

* 切片

  切片是对list，tuple，字符串等进行操作的表达式，其中tuple,字符串是特殊的list，tuple是不可改变的list。

  ```python
  L = ['A' , 'B' , 'C' , 'D']
  print(L[2:4])
  ```

* 生成器（generator）

  生成器是py中边计算边生成list空间的一个工具

  使用方式

  * 直接使用（）代替list 的 []即可生成

  ```python
  g = (k for k in range(1, 10))
  ```

  yield具有记忆功能，打印生成器时可以记住当前打印的位置，下次接着这个位置继续

* 迭代器（Iterator）

  * 迭代对象：凡是可以用for循环的即为迭代对象（Iterable）,可用isinstance判断是否是迭代对象。
  * 迭代器：能被next（）调用并且不断返回下一个值的对象叫做迭代器，可以使用isinstance()判断一个对象是否是迭代器。迭代器提前不能知道数据的长度的数据类型。

* 函数

  * 传入函数

    即一个函数中可接受变量为函数

    ```python
    def foo(a, b, f):
    	return f(a) + f(b)

    print(foo(-5, 6, abs))	#11
    ```

    ​

* 匿名函数

  使用lambda关键字定义，函数只能有一个表达式

  ```python
   L = list(map(lambda x: x * x, [1, 2]))

  ```

* 装饰器

  装饰器就是在一个函数中可以执行另一个函数，废话，本来就可以。。。。

  举例：

  ```python
  def foo():
  	return sum(range(1, 10000))
  ```

  如果我需要加入他执行了多少时间如何处理？

  传统方式：在代码中加入

  ```python
  import time
  def foo():
  	print('开始：' + time.time())
  	s = sum(range(1, 10000))
  	print('结束：' + time.time())
  	return s
  ```

  那么其他函数也要这样操作呢？一个一个加？nonono,这很违反开放封闭原则，给你你也不敢

  so.....

  ```python
  import time
  def show_time(func):
      start_time=time.time()
      func()
      end_time=time.time()
      print('spend %s'%(end_time-start_time))
   
   
  def foo():
      print('hello foo')
      time.sleep(3)
   
  show_time(foo)
  ```

* 偏函数

  相当于将原来的函数进行多一次的封装，封装成常用功能的不同传太多麻烦的参数的函数调用

  优点：简化函数调用

  ```python
  int()	#默认可以转化为10进制，传参可以转8,16,2
  int(123, base = 2)

  #每次调用都得传相同的参数，所以以偏函数的名义进行改造
  def int2(num):
  	return int(num, base=2)
  	
  ```

  ​

* @property

  python中一个属性有getter（读）和setter（写）两种方式，这和以前Java，c#很像，那么py中如何来实现这种机制呢？就是利用@property来实现

  ```python
  class Student(object):

      @property	#读
      def birth(self):
          return self._birth

      @birth.setter	#写
      def birth(self, value):
          self._birth = value

      @property
      def age(self):
          return 2015 - self._birth
  ```

  ​